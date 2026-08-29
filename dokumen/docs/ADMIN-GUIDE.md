---
title: ADMIN-GUIDE — Administrator Manual
document_id: ADMIN-GUIDE
version: 1.0
cb_reference: [CB §25]
status: DRAFT
owner: Operations & Support
last_updated: 2026-08-29
---

# ADMIN-GUIDE — Administrator Manual

Panduan administrator untuk setup dan manajemen.

---

## Initial Admin Setup

### First-Time Admin Account

```bash
# Run during first deployment:
./paugeran-api --init-admin

# Prompts:
Email: admin@company.com
Password: [secure password]
Organization: PT Contoh Legal

# Generated: Admin API key (store securely)
```

---

## Team Member Management

### Invite User

```
Admin Dashboard → Users → Invite User

Fields:
├─ Email
├─ Role (Admin, Manager, User, Viewer)
├─ Department
├─ License tier (Personal, Team, Enterprise)
└─ Expiration date (optional)

User receives email with activation link
└─ Valid for 7 days
└─ Must set password
└─ Account activated
```

### Manage Roles

| Role | Permissions |
|------|-------------|
| Admin | Full access, manage users, billing |
| Manager | Manage users in team, view usage |
| User | Full feature access |
| Viewer | Read-only access |

---

## LLM Provider Configuration (Global)

### Setup Primary Provider

```
Admin Dashboard → Settings → LLM Providers

1. Provider Selection
   ├─ Anthropic Claude
   ├─ OpenAI
   ├─ OpenAI-compatible
   └─ Ollama (local)

2. Configure Anthropic
   ├─ API Key: [paste key]
   ├─ Model: claude-3-sonnet
   ├─ Max tokens: 4096
   ├─ Temperature: 0.7
   └─ Save

3. Configure Backup Provider
   ├─ Provider: OpenAI
   ├─ API Key: [paste key]
   ├─ Model: gpt-4-turbo
   └─ Only use if Anthropic fails
```

### Monitor LLM Usage

```
Dashboard → Analytics → LLM Usage

Metrics:
├─ Total API calls: 15,234
├─ Total cost: $342.50
├─ Avg latency: 1.2s
├─ Error rate: 0.1%
└─ Top queries (by token count)

Cost breakdown by provider:
├─ Anthropic: 60% ($205.50)
├─ OpenAI: 40% ($137.00)
```

---

## Knowledge Base Management

### Import Documents

```
KB → Upload Documents

Supported formats:
├─ PDF
├─ DOCX
├─ TXT
└─ Markdown

Bulk import:
1. Create ZIP with documents
2. Upload ZIP
3. System extracts & processes
4. Status: Processing (15 minutes for 100 docs)
5. Indexed and searchable
```

### Configure Whitelisted Domains

```
Settings → Web Research → Whitelist Domains

Current domains:
├─ jdih.kemenkumham.go.id
├─ mahkamahkonstitusi.go.id
├─ peraturan.bpk.go.id
└─ [+Add new domain]

Save and activate
```

### Monitor KB Health

```
Dashboard → KB Analytics

Metrics:
├─ Total documents: 5,234
├─ Total chunks: 45,120
├─ Index size: 2.3 GB
├─ Last update: 2026-08-29
└─ Search performance: ✅ Good

Recommendations:
├─ [ ] Remove duplicate documents
├─ [ ] Update outdated regulations
├─ [ ] Reindex embeddings (monthly)
```

---

## License Management

### Activate License

```
Settings → Licenses → Add License

Input:
├─ License Key: XXXX-XXXX-XXXX-XXXX
├─ License Type: Team (5 users)
├─ Expiration: 2027-08-29
└─ Activate

System validates against license server
└─ If offline, accepts local cache
```

### Manage Team Licenses

```
Dashboard → Licenses → Team Licenses

Per user:
├─ Name: John Doe
├─ Email: john@company.com
├─ License tier: Personal
├─ Expiration: 2027-08-29
├─ Last login: 2026-08-29
└─ Action: [Revoke] [Extend]

Add new:
└─ Add License Seat
    ├─ Quantity: 5
    ├─ Tier: Personal
    └─ Cost: $X per seat/year
```

---

## Monitoring & Usage

### Team Usage Dashboard

```
Dashboard → Team Analytics

User Activity:
├─ Active users (7 days): 12
├─ Total chats: 423
├─ Total queries: 1,245
├─ Export count: 34
└─ Top user: John Doe (89 queries)

Feature Usage:
├─ Chat feature: 1,245 queries
├─ KB search: 342 searches
├─ Web research: 156 searches
└─ Export: 34 documents

Storage:
├─ Data used: 450 MB
├─ Limit: 1 GB
├─ Warning: 50% reached
└─ Action: [ ] Upgrade storage
```

---

## Backup & Restore

### Backup Configuration

```
Settings → Backup → Configure

Backup Schedule:
├─ Automatic daily at 02:00 UTC
├─ Retention: 30 days
├─ Storage: Local + S3
└─ Compression: gzip
```

### Manual Backup

```
Settings → Backup → Manual Backup

1. Click "Backup Now"
2. System creates snapshot
3. Status: Backing up... (5 minutes)
4. Download backup (500 MB)
   └─ Encrypted with admin key
5. Or save to S3 automatically
```

### Restore from Backup

```
Settings → Backup → Restore

1. Select backup date
2. Review data that will be restored
3. Confirm: "This will overwrite current data"
4. Restore starts
5. Status: Restoring... (15 minutes)
6. Service restarts
7. Verification: Run health checks
```

---

## Troubleshooting (Admin)

### Issue: License Validation Fails

```
Symptom: Users see "License invalid"

Steps:
1. Check license server status
   Dashboard → Status → License Server
   
2. If down, service continues (grace period)
   Users can continue for 7 days
   
3. If local cache corrupted:
   Settings → Backup → Restore License
   
4. Contact: admin-support@paugeran.dev
```

### Issue: Search Slow

```
Symptom: KB search > 2s

Steps:
1. Check index status
   Dashboard → KB → Index Health
   
2. If needed, rebuild:
   Settings → KB → Reindex All
   (Background task, ~30 min)
   
3. Monitor progress:
   Dashboard → Tasks → Reindex
```

---

## Checklist Implementasi

- [ ] First admin account created
- [ ] Team members invited
- [ ] LLM providers configured
- [ ] KB documents imported
- [ ] Licenses activated
- [ ] Monitoring set up
- [ ] Backup automated
- [ ] Admin trained
- [ ] Support procedures ready

