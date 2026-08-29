---
title: OPS-RUNBOOK — Operational Runbook
document_id: OPS-RUNBOOK
version: 1.0
cb_reference: [CB §23]
status: DRAFT
owner: DevOps & Support Team
last_updated: 2026-08-29
---

# OPS-RUNBOOK — Operational Runbook

Panduan operasional dan penanganan incident umum.

---

## Monitoring Setup

### Metrics to Monitor

```
Application Metrics:
├─ HTTP request latency (P50, P95, P99)
├─ Chat response time
├─ API error rate
├─ Active user sessions
└─ LLM API usage/costs

Infrastructure:
├─ CPU usage (target: < 70%)
├─ Memory usage (target: < 80%)
├─ Disk I/O
├─ Network I/O
└─ Database connections

Database:
├─ Query latency
├─ Slow query count
├─ Connection pool usage
├─ Replication lag (if replicated)
└─ Backup status
```

### Alerting Rules

| Alert | Threshold | Action |
|-------|-----------|--------|
| CPU high | > 80% | Page on-call |
| Memory high | > 90% | Page on-call |
| Disk full | > 95% | Immediate action |
| API errors | > 5% | Investigate |
| Response time | P95 > 5s | Investigate |
| Chat timeout | > 10/hour | Escalate |
| DB down | Yes | Critical |
| License server down | Yes | Warning |

---

## Common Incidents & Procedures

### Incident: Server Down

**Detection:** Monitoring alert

**Immediate Actions (< 5 min)**
```bash
# 1. SSH to server
ssh ops@paugeran-prod.example.com

# 2. Check system status
systemctl status paugeran
sudo journalctl -u paugeran -n 50

# 3. Check logs
tail -100 /var/log/paugeran/app.log

# 4. Check system resources
top
df -h
```

**Diagnosis & Fix (< 15 min)**
```bash
# If OOM (out of memory):
free -h
sudo systemctl restart paugeran

# If disk full:
df -h /
du -sh /data/*  # Find large files
# Archive old logs or delete cache

# If hanging:
ps aux | grep paugeran
kill -9 <PID>
systemctl restart paugeran
```

**Verification**
```bash
curl -s http://localhost:8000/api/health | jq .
# Should return: {"status": "ok"}
```

---

### Incident: Database Corruption

**Detection:** Slow queries or connection errors

**Immediate Actions**
```bash
# 1. Check DB status
psql -U postgres -d paugeran -c "SELECT 1;"

# 2. Check indexes
psql -U postgres -d paugeran -c "REINDEX DATABASE paugeran;"

# 3. Run VACUUM
psql -U postgres -d paugeran -c "VACUUM ANALYZE;"
```

**If Still Failing**
```bash
# 1. Backup current (corrupted) DB
pg_dump -U postgres paugeran > corrupted_backup.sql

# 2. Restore from last known good backup
pg_restore -U postgres -d paugeran backup.dump

# 3. Verify
psql -U postgres -d paugeran -c "SELECT COUNT(*) FROM chats;"

# 4. Restart app
systemctl restart paugeran
```

---

### Incident: LLM API Failure

**Detection:** Chat responses fail or timeout

**Immediate Actions**
```bash
# 1. Check configuration
cat /etc/paugeran/config.yaml | grep -A5 llm_provider

# 2. Test connectivity
curl -s https://api.anthropic.com/v1/health
# Or: curl -s https://api.openai.com/v1/health

# 3. Check API key validity
curl -H "Authorization: Bearer $ANTHROPIC_API_KEY" \
  https://api.anthropic.com/v1/models
```

**Failover Strategy**
```bash
# If provider 1 down, try provider 2

# Check current provider priority
cat /etc/paugeran/providers.yaml

# Manually switch provider (temp)
curl -X POST http://localhost:8000/admin/set-provider \
  -H "Authorization: Bearer $ADMIN_TOKEN" \
  -d '{"primary_provider": "openai"}'

# Restart app to apply
systemctl restart paugeran
```

---

### Incident: License Server Unreachable

**Detection:** License validation fails

**Immediate Actions**
```bash
# 1. Check connectivity
curl -s https://license.paugeran.dev/api/health
ping license.paugeran.dev

# 2. Check local license cache
cat ~/.paugeran/license.json
```

**Graceful Degradation**
```
License validation failed?
├─ Offline mode → Use local cache (if available)
├─ Grace period → Allow 7 days without server
├─ Notify user: "License server temporary down"
└─ Continue service with local license
```

---

### Incident: Disk Full

**Immediate Actions (< 5 min)**
```bash
df -h /  # Check usage
du -sh /data/*  # Find what's using space

# Priority deletions:
1. Delete old log files
   rm -rf /var/log/paugeran/archive/*.log.*

2. Clear cache
   rm -rf /data/cache/*

3. Archive old KB documents
   tar -czf archive.tar.gz /data/kb/old/
   rm -rf /data/kb/old/
```

**Permanent Fix**
```
├─ Add more disk space (cloud provider)
├─ Implement retention policies
├─ Move old KB to archive storage
└─ Monitor disk usage
```

---

### Incident: High Memory Usage

**Diagnosis**
```bash
top  # Find process using memory
java -XX:+PrintFlagsFinal -version 2>&1 | grep -i heapsize
```

**Fix**
```bash
# Restart application
systemctl restart paugeran

# If persists, increase memory limit
# Edit /etc/systemd/system/paugeran.service
# [Service]
# MemoryLimit=8G

systemctl daemon-reload
systemctl restart paugeran
```

---

## Scheduled Maintenance Windows

```
Daily (02:00 UTC):
├─ Database backup
├─ Log rotation
└─ Cache cleanup

Weekly (Sundays 03:00 UTC):
├─ Full system backup
├─ Dependency updates
└─ Security patch check

Monthly (1st day 04:00 UTC):
├─ Major updates (if any)
├─ Database optimization
└─ Disaster recovery test
```

---

## Escalation Procedure

```
Level 1 (Auto-recovery):
└─ Automated restart, failover

Level 2 (On-call engineer):
└─ Diagnosis within 15 min
└─ Fix or escalate within 1 hour

Level 3 (Engineering manager):
└─ Escalate if Level 2 can't fix
└─ Major incident (> 1 hour downtime)

Level 4 (Executive):
└─ > 4 hours downtime
└─ Data loss risk
└─ Security breach
```

---

## Checklist Implementasi

- [ ] Monitoring configured
- [ ] Alert rules set
- [ ] Incident procedures documented
- [ ] On-call rotation established
- [ ] Escalation procedures clear
- [ ] Runbook tested
- [ ] Team trained
- [ ] Communication channels ready

