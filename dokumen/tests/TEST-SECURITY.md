---
title: TEST-SECURITY — Security Testing Protocol
document_id: TEST-SECURITY
version: 1.0
cb_reference: [CB §26]
status: DRAFT
owner: Security & QA Team
last_updated: 2026-08-29
---

# TEST-SECURITY — Security Testing Protocol

Protokol testing keamanan OWASP Top 10.

---

## OWASP Top 10 (2021) Testing

### 1. Broken Access Control
```
Test: User A accesses User B's chat
Steps:
1. Login as User A (ID: 1001)
2. Extract session token
3. Access /api/chats/1002 (User B's ID)
Expected: Error 403 Forbidden
Not OK: User B's data visible

Test: Query other user's KB
Steps:
1. Token for User A
2. GET /api/kb/docs?user_id=1002
Expected: Error 403
Not OK: Other KB visible

Test: Admin permission bypass
Steps:
1. Modify JWT payload (claim admin=true)
2. Access admin endpoints
Expected: JWT validation fails
Not OK: Admin access granted
```

### 2. Cryptographic Failures
```
Test: Password storage
Check: Passwords hashed with Argon2id
Not OK: Plain text or weak hash (MD5/SHA1)

Test: API key exposure
Check: Keys never in logs
Check: Keys encrypted at rest
Not OK: Key visible in error messages

Test: TLS enforcement
Check: HTTPS required
Check: No mixed content
Not OK: HTTP allowed
```

### 3. Injection (SQL, Command, etc.)
```
Test: SQL Injection
Query: "chat?query='; DROP TABLE users; --"
Expected: Parameterized queries used, safe
Not OK: SQL error or table deleted

Test: NoSQL Injection
Query: "search?term={$ne: null}"
Expected: Safe parsing
Not OK: Injection succeeds

Test: Command Injection
Query: "export?format=pdf; rm -rf /"
Expected: Format validation, safe
Not OK: Command executed
```

### 4. Insecure Design
```
Test: No rate limiting
Steps:
1. Brute force login (1000 attempts/min)
Expected: Throttled after N attempts
Not OK: No limit

Test: Account enumeration
Steps:
1. Register check with common emails
Expected: Response same for all
Not OK: "User exists" message leaks info

Test: No auth on sensitive endpoints
Steps:
1. GET /api/health without token
Expected: Depends on endpoint security policy
```

### 5. Security Misconfiguration
```
Test: Debug mode enabled
Check: Stack traces hidden in production
Not OK: Sensitive info in error messages

Test: Default credentials
Check: No default admin/admin account
Not OK: Default creds still active

Test: Outdated dependencies
Check: No known CVEs in dependencies
Command: cargo audit, npm audit
```

### 6. Vulnerable Components
```
Command: cargo audit
Action: Review and patch
Command: npm audit
Action: Update dependencies

Test: Dependency confusion
Steps:
1. Create malicious package with same name
Expected: Only trusted registry used
Not OK: Malicious package installed
```

### 7. Authentication Failures
```
Test: Session fixation
Steps:
1. Get session ID before login
2. Try using after login
Expected: Session invalidated
Not OK: Pre-login session still valid

Test: Weak password validation
Expected: Min 12 chars, mixed case, special
Not OK: Simple password accepted

Test: Account lockout
Steps:
1. 5 failed logins
Expected: Account locked for 15 min
Not OK: No lockout
```

### 8. Software and Data Integrity Failures
```
Test: Code signing
Check: Release binaries signed
Check: Signature verification before use

Test: Dependency updates
Check: Only from trusted sources
Check: No tampering in transit

Test: Update verification
Check: Downloads verified
Check: No automatic execution
```

### 9. Logging & Monitoring Failures
```
Test: Sensitive data in logs
Check: Passwords not logged
Check: API keys not logged
Check: PII not logged

Test: Failed login logging
Check: All failed attempts logged
Check: Alert on N failed attempts

Test: Admin action logging
Check: All admin actions logged
Check: Audit trail immutable
```

### 10. SSRF (Server-Side Request Forgery)
```
Test: Access internal URLs
Query: "web_search?url=http://localhost:8000"
Expected: Internal URLs blocked
Not OK: Internal service accessed

Test: EC2 metadata bypass
Query: "web_search?url=http://169.254.169.254/latest/meta-data/"
Expected: Blocked
Not OK: Credentials exposed
```

---

## Penetration Testing Checklist

- [ ] Try bypass authentication
- [ ] Access data of other users
- [ ] Modify other user's data
- [ ] Escalate privileges
- [ ] Export full database
- [ ] Disrupt service (DoS)
- [ ] Modify audit logs
- [ ] Install backdoor
- [ ] Exfiltrate all data
- [ ] Achieve RCE (remote code execution)

---

## API Security Testing

```
Tool: Burp Suite / OWASP ZAP

Test:
1. Replay request without auth token
   Expected: 401 Unauthorized
   
2. Modify request body (change user_id)
   Expected: Rejected or filtered
   
3. Try all HTTP methods (PUT, DELETE, PATCH)
   Expected: Only allowed methods work
   
4. Bypass CORS
   Expected: Only trusted origins
   
5. Test rate limiting
   Expected: After N requests, 429 Too Many Requests
```

---

## Checklist Implementasi

- [ ] All OWASP Top 10 scenarios tested
- [ ] Penetration test completed
- [ ] Dependency audit passed
- [ ] Logging verified (no sensitive data)
- [ ] Monitoring alerts configured
- [ ] Security patch procedure established
- [ ] Incident response plan ready
- [ ] Zero critical vulns before release

