---
title: SPEC-AUTH — Spesifikasi Autentikasi & Otorisasi
document_id: SPEC-AUTH
version: 1.0
cb_reference: [CB §25]
status: DRAFT
owner: Security Team
last_updated: 2026-08-29
---

# SPEC-AUTH — Spesifikasi Autentikasi & Otorisasi

Detail implementasi sistem auth.

## Referensi CB
- [CB §25] — Authentication & authorization requirements

---

## JWT Token Structure

### Token Format

```json
{
  "sub": "user_id_uuid",
  "email": "user@example.com",
  "username": "username",
  "role": "user",
  "permissions": ["read:chat", "write:export", "read:kb"],
  "iat": 1693411200,
  "exp": 1693497600,
  "iss": "paugeran",
  "aud": "paugeran-app"
}
```

### Token Lifecycle

```
User login successful
    ↓
Generate JWT:
  - iat (issued at): now
  - exp (expiration): now + 24 hours
  - refresh_token: now + 7 days
    ↓
Return to client:
  - access_token (24h) — Use for API calls
  - refresh_token (7d) — Use to get new access_token
  - expires_in: 86400
    ↓
Client stores tokens:
  - access_token: Memory (cleared on logout)
  - refresh_token: Secure cookie (httpOnly, Secure)
    ↓
Subsequent requests:
  Authorization: Bearer <access_token>
    ↓
Token expired?
  ├─ No → Grant access
  └─ Yes → 
      Use refresh_token to get new access_token
      If refresh also expired → Re-login required
```

---

## Password Hashing Strategy

### Algorithm: Argon2id

```rust
use argon2::{Argon2, ParamString, PasswordHasher};

pub struct PasswordHasher {
    argon2: Argon2<'static>,
}

impl PasswordHasher {
    pub fn new() -> Self {
        // Argon2id with strong parameters
        let params = ParamString::try_from(
            "m=19456,t=2,p=1"  // 19MB memory, 2 iterations, 1 parallelism
        ).unwrap();
        
        Self {
            argon2: Argon2::default(),
        }
    }
    
    pub fn hash_password(&self, password: &str, salt: &[u8]) -> Result<String> {
        // Hash password with salt
        let password_hash = self.argon2
            .hash_password(password.as_bytes(), salt)?
            .to_string();
        
        Ok(password_hash)
    }
    
    pub fn verify_password(
        &self,
        password: &str,
        hash: &str,
    ) -> Result<bool> {
        let parsed_hash = PasswordHash::new(hash)?;
        Ok(self.argon2.verify_password(
            password.as_bytes(),
            &parsed_hash
        ).is_ok())
    }
}

// Registration
pub async fn register(
    email: String,
    password: String,
) -> Result<User> {
    // Validate password strength
    validate_password_strength(&password)?;
    
    // Generate salt
    let salt = SaltString::generate(rand::thread_rng());
    
    // Hash password
    let password_hash = password_hasher.hash_password(&password, salt.as_str())?;
    
    // Create user
    let user = User {
        user_id: Uuid::new_v4(),
        email,
        password_hash,
        created_at: SystemTime::now(),
    };
    
    db.insert_user(&user).await?;
    Ok(user)
}
```

**Password Requirements:**
- Minimum 8 characters
- At least 1 uppercase letter
- At least 1 lowercase letter
- At least 1 number
- At least 1 special character

---

## Role-Based Access Control (RBAC)

### Role Definitions

```rust
pub enum Role {
    Admin,      // Full system access
    User,       // Standard user
    Guest,      // Read-only access
}

pub enum Permission {
    // Chat
    CreateChat,
    ReadChat,
    UpdateChat,
    DeleteChat,
    
    // Export
    Export,
    
    // Knowledge Base
    ReadKb,
    CreateKbDocument,
    UpdateKbDocument,
    DeleteKbDocument,
    ManageKb,
    
    // Admin
    ManageUsers,
    ManageLicense,
    ViewAnalytics,
    SystemConfig,
}

pub struct RoleMatrix {
    roles: HashMap<Role, Vec<Permission>>,
}

impl RoleMatrix {
    pub fn new() -> Self {
        let mut roles = HashMap::new();
        
        // Admin: all permissions
        roles.insert(Role::Admin, vec![
            Permission::CreateChat,
            Permission::ReadChat,
            Permission::UpdateChat,
            Permission::DeleteChat,
            Permission::Export,
            Permission::ReadKb,
            Permission::CreateKbDocument,
            Permission::UpdateKbDocument,
            Permission::DeleteKbDocument,
            Permission::ManageKb,
            Permission::ManageUsers,
            Permission::ManageLicense,
            Permission::ViewAnalytics,
            Permission::SystemConfig,
        ]);
        
        // User: most permissions except admin
        roles.insert(Role::User, vec![
            Permission::CreateChat,
            Permission::ReadChat,
            Permission::UpdateChat,
            Permission::DeleteChat,
            Permission::Export,
            Permission::ReadKb,
            Permission::CreateKbDocument,
            Permission::UpdateKbDocument,
            Permission::DeleteKbDocument,
        ]);
        
        // Guest: read-only
        roles.insert(Role::Guest, vec![
            Permission::ReadChat,
            Permission::ReadKb,
        ]);
        
        Self { roles }
    }
    
    pub fn has_permission(
        &self,
        role: &Role,
        permission: &Permission,
    ) -> bool {
        self.roles
            .get(role)
            .map(|perms| perms.contains(permission))
            .unwrap_or(false)
    }
}
```

### RBAC Matrix

| Permission | Admin | User | Guest |
|-----------|-------|------|-------|
| CreateChat | ✅ | ✅ | ❌ |
| ReadChat | ✅ | ✅ | ✅ |
| UpdateChat | ✅ | ✅ | ❌ |
| DeleteChat | ✅ | ✅ | ❌ |
| Export | ✅ | ✅ | ❌ |
| ReadKb | ✅ | ✅ | ✅ |
| CreateKbDocument | ✅ | ✅ | ❌ |
| UpdateKbDocument | ✅ | ✅ | ❌ |
| DeleteKbDocument | ✅ | ✅ | ❌ |
| ManageKb | ✅ | ❌ | ❌ |
| ManageUsers | ✅ | ❌ | ❌ |
| ManageLicense | ✅ | ❌ | ❌ |
| ViewAnalytics | ✅ | ❌ | ❌ |
| SystemConfig | ✅ | ❌ | ❌ |

---

## Invitation System Flow

### Admin Invites User

```
Admin clicks "Invite User"
    ↓
Admin enters email
    ↓
System generates:
  - invitation_token (random, 32 chars)
  - expires_at: now + 7 days
    ↓
Store invitation in DB:
  CREATE invitation {
    invitation_id,
    email,
    token,
    created_by: admin_id,
    expires_at,
    status: 'pending'
  }
    ↓
Send email with link:
  https://paugeran.dev/auth/accept-invite?token=<token>
    ↓
User receives email + clicks link
    ↓
Verify token:
  ├─ Valid + not expired → Show registration form
  ├─ Valid + expired → Show "invite expired" message
  └─ Invalid → Show "invalid invite" message
    ↓
User enters password + registers
    ↓
Account created with status "active"
    ↓
Mark invitation as "accepted"
```

### Implementation

```rust
pub async fn create_invitation(
    admin_id: Uuid,
    email: String,
) -> Result<Invitation> {
    let invitation_token = generate_secure_random_token(32)?;
    
    let invitation = Invitation {
        invitation_id: Uuid::new_v4(),
        email,
        token: invitation_token,
        created_by: admin_id,
        created_at: SystemTime::now(),
        expires_at: SystemTime::now() + Duration::from_secs(7 * 24 * 3600),
        status: InvitationStatus::Pending,
    };
    
    db.insert_invitation(&invitation).await?;
    
    // Send email
    send_invitation_email(&invitation).await?;
    
    Ok(invitation)
}

pub async fn accept_invitation(
    token: String,
    password: String,
    username: String,
) -> Result<User> {
    // Validate token
    let invitation = db.find_invitation_by_token(&token).await?;
    
    if SystemTime::now() > invitation.expires_at {
        return Err("Invitation expired".into());
    }
    
    if invitation.status != InvitationStatus::Pending {
        return Err("Invitation already used".into());
    }
    
    // Create user
    let user = User {
        user_id: Uuid::new_v4(),
        email: invitation.email,
        username,
        password_hash: hash_password(&password)?,
        role: Role::User,
        created_at: SystemTime::now(),
    };
    
    db.insert_user(&user).await?;
    
    // Mark invitation as accepted
    db.update_invitation_status(
        &invitation.invitation_id,
        InvitationStatus::Accepted,
    ).await?;
    
    Ok(user)
}
```

---

## Admin Privileges & Limitations

### Admin Capabilities
- ✅ Create invitations
- ✅ Manage users (activate/deactivate)
- ✅ View user list
- ✅ View system analytics
- ✅ Manage KB (if KB is team-based)
- ✅ Configure LLM providers
- ✅ Manage license

### Admin Limitations
- ❌ Cannot read other users' chat sessions
- ❌ Cannot modify other users' passwords
- ❌ Cannot impersonate other users
- ❌ Cannot export other users' documents without permission
- ❌ Cannot access without valid license

---

## Session Management

### Session Tracking

```rust
pub struct Session {
    session_id: Uuid,
    user_id: Uuid,
    token: String,
    ip_address: IpAddr,
    user_agent: String,
    created_at: SystemTime,
    last_activity: SystemTime,
    expires_at: SystemTime,
}

pub struct SessionManager {
    db: Database,
}

impl SessionManager {
    pub async fn create_session(
        &self,
        user_id: Uuid,
        token: String,
        ip_address: IpAddr,
        user_agent: String,
    ) -> Result<Session> {
        let session = Session {
            session_id: Uuid::new_v4(),
            user_id,
            token,
            ip_address,
            user_agent,
            created_at: SystemTime::now(),
            last_activity: SystemTime::now(),
            expires_at: SystemTime::now() + Duration::from_secs(24 * 3600),
        };
        
        self.db.insert_session(&session).await?;
        Ok(session)
    }
    
    pub async fn validate_session(&self, token: &str) -> Result<Session> {
        let session = self.db.find_session_by_token(token).await?;
        
        if SystemTime::now() > session.expires_at {
            self.db.delete_session(&session.session_id).await?;
            return Err("Session expired".into());
        }
        
        // Update last activity
        self.db.update_last_activity(&session.session_id).await?;
        
        Ok(session)
    }
    
    pub async fn logout(&self, session_id: &Uuid) -> Result<()> {
        self.db.delete_session(session_id).await?;
        Ok(())
    }
}
```

---

## Security Considerations

### Best Practices

1. **HTTPS/TLS Only**
   - All auth endpoints require HTTPS
   - HSTS headers enforced

2. **Token Security**
   - Tokens stored in memory only (not localStorage)
   - Refresh tokens in secure cookies (httpOnly, Secure, SameSite)

3. **Rate Limiting**
   - Login attempts: 5 per minute per IP
   - Password reset: 3 per hour per email
   - API calls: Per user quota

4. **Account Lockout**
   - Lock after 5 failed login attempts
   - Unlock after 15 minutes or admin action

5. **Password Reset**
   - Send reset link via email (expires in 1 hour)
   - Require entering current password or email verification
   - Invalidate all existing sessions after password change

---

## Testing Scenarios for Auth Bypass

| Scenario | Test | Result |
|----------|------|--------|
| Missing token | Call API without Authorization header | 401 Unauthorized |
| Expired token | Use token older than 24h | 401 Unauthorized |
| Invalid signature | Tamper with token | 401 Unauthorized |
| Wrong audience | Token for different app | 401 Unauthorized |
| Role mismatch | User without permission tries operation | 403 Forbidden |
| Privilege escalation | User tries to change own role to Admin | 403 Forbidden |
| Session hijacking | Use token from another session | 401 Unauthorized |

---

## Checklist Implementasi

- [ ] JWT implementation working
- [ ] Password hashing with Argon2id
- [ ] RBAC matrix implemented
- [ ] Invitation system end-to-end
- [ ] Session management
- [ ] Security best practices implemented
- [ ] Comprehensive auth testing
- [ ] Documentation complete

---

## Referensi Tambahan

- [JWT Best Practices](https://tools.ietf.org/html/rfc8949)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [Argon2 Documentation](https://github.com/P-H-C/phc-winner-argon2)
