---
title: OPS-BACKUP — Backup & Recovery Procedures
document_id: OPS-BACKUP
version: 1.0
cb_reference: [CB §22]
status: DRAFT
owner: DevOps Team
last_updated: 2026-08-29
---

# OPS-BACKUP — Backup & Recovery Procedures

Panduan backup, restore, dan disaster recovery.

---

## Backup Strategy

### Full Backup
```
Frequency: Daily (02:00 UTC)
Target: Entire database
Duration: ~30 minutes
Storage: Local + S3
Retention: 30 days
```

### Incremental Backup
```
Frequency: Every 6 hours
Target: Changes since last full backup
Duration: ~5 minutes
Storage: Local only
Retention: Until next full backup
```

### Monitoring Backup
```
Schedule: Every 6 hours
Target: Backups of Prometheus + Grafana configs
Duration: ~2 minutes
Storage: Git repo + S3
Retention: 90 days
```

---

## Backup Methods

### Database Backup (PostgreSQL)
```bash
# Full backup
pg_dump -U postgres paugeran > backup.sql

# Compressed backup
pg_dump -U postgres -Fc paugeran > backup.dump

# Parallel backup
pg_dump -U postgres -j 4 paugeran > backup/

# Point-in-time backup
BACKUP_LABEL=backup_label WAL_LEVEL=replica

# Automated script
#!/bin/bash
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
pg_dump -U postgres -Fc paugeran > backups/paugeran_${TIMESTAMP}.dump
aws s3 cp backups/paugeran_${TIMESTAMP}.dump s3://paugeran-backups/
```

### SQLite Backup (Offline/Local)
```bash
# Copy database file
cp ~/.paugeran/data.sqlite ~/.paugeran/backups/data_$(date +%Y%m%d).sqlite

# In-transaction backup
sqlite3 data.sqlite ".backup backup.sqlite"
```

### File Backup
```bash
# KB documents
tar -czf kb_backup_$(date +%Y%m%d).tar.gz /data/kb/

# Configuration
tar -czf config_backup_$(date +%Y%m%d).tar.gz /etc/paugeran/
```

---

## S3 Backup Configuration

```rust
pub struct S3BackupManager {
    s3_client: S3Client,
    bucket: String,
}

impl S3BackupManager {
    pub async fn upload_backup(&self, local_path: &str) -> Result<()> {
        let file_key = format!(
            "backups/{}/{}",
            Utc::now().format("%Y/%m/%d"),
            Path::new(local_path).file_name().unwrap().to_str().unwrap()
        );
        
        let body = ByteStream::from_path(local_path).await?;
        
        self.s3_client
            .put_object()
            .bucket(&self.bucket)
            .key(file_key)
            .body(body)
            .storage_class("GLACIER")  // Cost-effective for old backups
            .send()
            .await?;
        
        Ok(())
    }
}
```

---

## Backup Verification

```bash
#!/bin/bash
# Daily verification

BACKUP_FILE=$1

# Check file size
SIZE=$(du -h "$BACKUP_FILE" | cut -f1)
if [[ $SIZE -lt "100M" ]]; then
  echo "ERROR: Backup too small"
  exit 1
fi

# Verify dump integrity
pg_restore -l "$BACKUP_FILE" > /dev/null
if [[ $? -ne 0 ]]; then
  echo "ERROR: Backup corrupt"
  exit 1
fi

echo "✅ Backup verified: $SIZE"
```

---

## Recovery Procedures

### Full Database Recovery
```bash
#!/bin/bash
# Restore from backup.dump

# 1. Stop application
systemctl stop paugeran

# 2. Create new database
createdb -U postgres paugeran_recovery

# 3. Restore
pg_restore -U postgres -d paugeran_recovery backup.dump

# 4. Verify
psql -U postgres paugeran_recovery -c "SELECT COUNT(*) FROM chats;"

# 5. Swap databases (if OK)
psql -U postgres -c "
  DROP DATABASE paugeran;
  ALTER DATABASE paugeran_recovery RENAME TO paugeran;
"

# 6. Start application
systemctl start paugeran
```

### Partial Recovery (Specific Table)
```bash
# Restore single table
pg_restore -U postgres -d paugeran -t chats backup.dump
```

### Point-in-Time Recovery
```bash
# Restore to specific timestamp
psql -U postgres paugeran << EOF
SELECT pg_wal_replay_pause();
-- Database paused at chosen point
SELECT pg_wal_replay_resume();
EOF
```

---

## RTO/RPO Targets

| Scenario | Recovery Time Objective | Recovery Point Objective |
|----------|------------------------|------------------------|
| Server crash | 30 min | 6 hours |
| Database corruption | 1 hour | 1 hour |
| Data deletion | 2 hours | 1 hour |
| Full DC failure | 4 hours | 6 hours |
| Ransomware attack | 8 hours | 24 hours |

---

## Disaster Recovery Plan

### Single-Region Failure
```
1. Detect: Monitoring alert
2. Notify: Page on-call engineer
3. Failover: Switch to backup DB (if replicated)
4. Restore: From S3 backup
5. Verify: Run tests
6. Communicate: Status update
7. Post-mortem: Root cause analysis
```

### Multi-Region Setup (Future)
```
Primary DC (US-East)
├─ Master database
├─ Real-time replication
└─ Backup to S3

Secondary DC (US-West)
├─ Replica database (hot standby)
├─ Can promote if primary fails
└─ Keeps < 1s lag
```

---

## Monitoring

```
Daily Checks:
├─ Backup job completion
├─ Backup file size
├─ S3 upload success
└─ Backup verification

Weekly:
├─ Test restore procedure
└─ Verify data integrity

Monthly:
├─ Review backup logs
├─ Update runbook
└─ Practice full recovery
```

---

## Checklist Implementasi

- [ ] Backup scripts created
- [ ] S3 configured
- [ ] Backup verification automated
- [ ] Recovery procedures documented
- [ ] RTO/RPO targets set
- [ ] Monitoring alerts active
- [ ] Full recovery tested
- [ ] Disaster recovery plan reviewed

