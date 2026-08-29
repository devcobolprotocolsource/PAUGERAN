---
title: SPEC-WEB — Spesifikasi Penelitian Web
document_id: SPEC-WEB
version: 1.0
cb_reference: [CB §17]
status: DRAFT
owner: Backend Team
last_updated: 2026-08-29
---

# SPEC-WEB — Spesifikasi Penelitian Web

Detail implementasi web research module.

---

## Whitelist Domains

### Legal Domains

```
Government & Courts:
├─ jdih.kemenkumham.go.id (Legal documents)
├─ mahkamahkonstitusi.go.id (Constitutional court)
├─ ptun.mahkamahagung.go.id (Administrative court)
├─ www.dpr.go.id (Parliament)
└─ peraturan.bpk.go.id (Audit board)

Official News:
├─ kemenkumham.go.id
├─ berita-negara.go.id
└─ medialegal.gov.id

Academic:
├─ scholar.google.com
├─ ssrn.com
└─ researchgate.net

Legal Publishers:
├─ lexisnexis.com
├─ westlaw.com
└─ jeniusindo.com
```

### Mechanism

```rust
pub struct DomainWhitelist {
    domains: Vec<String>,
    max_urls_per_search: usize,
}

impl DomainWhitelist {
    pub fn is_allowed(&self, url: &str) -> bool {
        let domain = extract_domain(url)?;
        self.domains.contains(&domain)
    }
}
```

---

## HTTP Client Configuration

```rust
let client = reqwest::Client::builder()
    .timeout(Duration::from_secs(10))
    .connect_timeout(Duration::from_secs(5))
    .user_agent("PAUGERAN/1.0 (+https://paugeran.dev/bot)")
    .build()?;
```

- Timeout: 10 seconds
- User-Agent: Identify as bot
- Redirects: Max 3
- Retries: Max 2 on transient errors

---

## Content Extraction

```rust
pub struct ContentExtractor {
    // Extract main content, remove noise
}

impl ContentExtractor {
    pub fn extract(&self, html: &str) -> Result<String> {
        // 1. Parse HTML
        // 2. Remove scripts, styles
        // 3. Extract main content (heuristics)
        // 4. Convert to plain text
        // 5. Limit to 2000 chars
    }
}
```

---

## Robots.txt Compliance

```rust
pub struct RobotsCompliance {
    robot_parser: RobotParser,
}

impl RobotsCompliance {
    pub async fn is_allowed(&self, url: &str) -> Result<bool> {
        // 1. Fetch /robots.txt
        // 2. Parse rules
        // 3. Check if URL allowed
        // 4. Respect Disallow & Crawl-delay
    }
}
```

---

## Rate Limiting

```
Per domain:
├─ Max 1 request/second
├─ Max 10 requests/minute
├─ Respect Retry-After header
└─ Cache results 1 hour
```

---

## Metadata Extraction

For legal pages, extract:
- Title
- Publication date
- Author/Source
- Document type (regulation, court decision, news)
- Related documents

---

## Error Handling

| Error | Action |
|-------|--------|
| 404 Not Found | Skip |
| 429 Too Many Requests | Backoff |
| 503 Service Unavailable | Retry (max 3x) |
| Connection timeout | Skip |
| Invalid domain | Block |

---

## Integration with KB

```
Web Research Results
    ↓
Extract metadata + content
    ↓
Check if already in KB
├─ Yes → Link to existing
└─ No → Add as new entry
    ↓
Update embeddings
    ↓
Update citations
```

---

## Legal Considerations

- Respect copyright (fair use, limited excerpts)
- Cite sources properly
- Don't republish full documents
- Check terms of use per domain
- Request permission if needed (commercial use)

---

## Checklist Implementasi

- [ ] Domain whitelist finalized
- [ ] HTTP client configured
- [ ] Content extraction working
- [ ] Robots.txt compliance
- [ ] Rate limiting enforced
- [ ] Metadata extraction
- [ ] KB integration
- [ ] Legal review completed

