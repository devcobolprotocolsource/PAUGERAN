---
title: TEST-PERFORMANCE — Performance Testing Protocol
document_id: TEST-PERFORMANCE
version: 1.0
cb_reference: [CB §29]
status: DRAFT
owner: DevOps & QA Team
last_updated: 2026-08-29
---

# TEST-PERFORMANCE — Performance Testing Protocol

Protokol dan benchmarks untuk performance testing.

---

## Load Testing Targets

### Concurrent Users
```
Scenario 1: Normal Day
├─ 100 concurrent users
├─ Average session duration: 15 minutes
├─ Requests per session: 10-20
└─ Expected: All respond < 2s

Scenario 2: Peak Hour
├─ 500 concurrent users
├─ Average session duration: 10 minutes
├─ Requests per session: 15-30
└─ Expected: All respond < 3s

Scenario 3: High Load
├─ 1000 concurrent users
├─ Short burst (5 minutes)
├─ Expected: Graceful degradation, error rate < 5%
└─ No data loss
```

---

## Stress Testing

```
Gradual increase: 10 → 100 → 500 → 1000 → 2000 users
└─ Measure: CPU, Memory, DB connections, Response time
└─ Stop when: First component maxes out
└─ Record: Max sustainable load
```

---

## Endurance Testing

```
Duration: 24+ hours
Load: 70% of peak
Measure:
├─ Memory leak detection
├─ Connection pool stability
├─ DB performance degradation
└─ Disk I/O patterns

Tools:
├─ Locust (load generation)
├─ Grafana (metrics)
├─ Prometheus (collection)
└─ JVM profiler (Java components)
```

---

## Benchmarks by Operation

| Operation | Latency (P95) | Throughput | Acceptance |
|-----------|---------------|-----------|-----------|
| Login | < 500ms | 100/s | ✅ SLA |
| Chat query (local) | < 3s | 10/s | ✅ SLA |
| Chat query (API) | < 2s | 50/s | ✅ SLA |
| Search KB | < 500ms | 100/s | ✅ SLA |
| Export PDF | < 3s | 5/s | ✅ SLA |
| List sessions | < 200ms | 200/s | ✅ SLA |
| License validation | < 100ms | 1000/s | ✅ SLA |

---

## Test Setup (Locust)

```python
from locust import HttpUser, task, between

class ChatUser(HttpUser):
    wait_time = between(1, 3)
    
    @task
    def chat(self):
        response = self.client.post(
            "/api/chat",
            json={"query": "Apa itu UU ITE?"},
            headers={"Authorization": f"Bearer {token}"}
        )
        assert response.status_code == 200

if __name__ == "__main__":
    # locust -f perf_test.py --headless -u 100 -r 10 -t 5m
```

---

## Database Performance

### Query Benchmarks
```
SELECT * FROM chats WHERE user_id = ?
├─ Expected: < 10ms

SELECT * FROM embeddings WHERE ...
├─ Expected: < 50ms (vector similarity)

INSERT chunk INTO kb ...
├─ Expected: < 100ms

Reindex full KB (100k docs)
├─ Expected: < 5 minutes (background)
```

---

## Frontend Performance

### Metrics (Lighthouse)
```
Performance:        ≥ 90
Accessibility:      ≥ 95
Best Practices:     ≥ 90
SEO:               ≥ 90

Core Web Vitals:
├─ LCP < 2.5s      ✅
├─ FID < 100ms     ✅
└─ CLS < 0.1       ✅
```

### Bundle Size
```
app.js:     < 500KB (gzip)
styles.css: < 100KB (gzip)
Total:      < 600KB (gzip)
```

---

## Profiling Tools

### CPU Profiling
```bash
# Rust backend
cargo flamegraph --bin paugeran-api

# Results: Identify hotspots
# Top 5 functions by CPU time
```

### Memory Profiling
```bash
# Check for leaks
valgrind --leak-check=full ./paugeran-api
```

### Database Profiling
```sql
-- Slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 0.5;
```

---

## Regression Detection

### Automated Regression Tests
```bash
# Before each release:
./perf_tests.sh

# Compare with baseline:
Baseline (v1.0.0):  150ms
Current (v1.0.1):   160ms
Regression:         6.7% ⚠️

Action:
├─ If < 5%:  OK, deploy
├─ If 5-10%: Review, consider delay
└─ If > 10%: BLOCK, must optimize
```

---

## Reporting

### Weekly Performance Report
```markdown
## Week of 2026-08-29

### Key Metrics
- P95 Chat Latency: 2.1s (target: 2.0s) ⚠️ Investigate
- Throughput: 45 req/s (target: 50 req/s) ⚠️
- CPU usage: 65% (target: 70% max) ✅
- Memory: 4.2GB (target: 5GB max) ✅

### Incidents
- 2026-08-29 15:00: Query timeout (fixed in 30m)

### Optimizations
- Implemented query result caching → 20% faster searches
- Upgraded DB indexes → 15% improvement

### Next Week
- [ ] Vector DB optimization
- [ ] Cache invalidation strategy review
```

---

## Checklist Implementasi

- [ ] Load test scripts created
- [ ] Stress test procedures documented
- [ ] 24h+ endurance test executed
- [ ] Benchmarks established for all operations
- [ ] Regression detection automated
- [ ] Performance monitoring live
- [ ] Weekly reports generated
- [ ] All SLAs met before release

