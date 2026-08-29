---
title: TEST-CASES — Comprehensive Test Cases
document_id: TEST-CASES
version: 1.0
cb_reference: [CB §27]
status: DRAFT
owner: QA Team
last_updated: 2026-08-29
---

# TEST-CASES — Comprehensive Test Cases

Test coverage untuk semua API endpoints dan features.

---

## Authentication Test Cases

### Login Flow
| ID | Test Case | Steps | Expected | Priority |
|----|-----------|-------|----------|----------|
| AUTH-001 | Valid credentials | Login with correct email/password | JWT token returned | 🔴 WAJIB |
| AUTH-002 | Invalid password | Login with wrong password | Error 401 | 🔴 WAJIB |
| AUTH-003 | Non-existent user | Login with unknown email | Error 404 | 🔴 WAJIB |
| AUTH-004 | Empty fields | Submit empty form | Validation error | 🔴 WAJIB |
| AUTH-005 | Token expiration | Use expired token | Error 401 | 🔴 WAJIB |

### Registration
| ID | Test Case | Steps | Expected | Priority |
|----|-----------|-------|----------|----------|
| AUTH-010 | Valid registration | Register with new email/password | Account created | 🔴 WAJIB |
| AUTH-011 | Duplicate email | Register with existing email | Error 409 | 🔴 WAJIB |
| AUTH-012 | Weak password | Register with weak password | Validation error | 🟡 PENTING |
| AUTH-013 | Invalid email | Register with invalid format | Validation error | 🟡 PENTING |

---

## Chat API Test Cases

### Initiate Chat
| ID | Test Case | Expected | Priority |
|----|-----------|----------|----------|
| CHAT-001 | Create new session | session_id returned | 🔴 WAJIB |
| CHAT-002 | Invalid token | Error 401 | 🔴 WAJIB |
| CHAT-003 | Expired license | Error 403 | 🔴 WAJIB |

### Send Message
| ID | Test Case | Query | Expected | Priority |
|----|-----------|-------|----------|----------|
| CHAT-010 | Normal query | "Apa itu UU ITE?" | Response generated | 🔴 WAJIB |
| CHAT-011 | Empty query | "" | Validation error | 🔴 WAJIB |
| CHAT-012 | Very long query | 100k chars | Error 413 (Payload too large) | 🟡 PENTING |
| CHAT-013 | Special characters | "SQL; DROP TABLE..." | Safe handling | 🔴 WAJIB |
| CHAT-014 | Multi-language | Indonesian + English mix | Response OK | 🟢 PENDUKUNG |

---

## Knowledge Base Test Cases

### Search
| ID | Test Case | Query | Expected | Priority |
|----|-----------|-------|----------|----------|
| KB-001 | Keyword search | "Pasal 12" | Results returned | 🔴 WAJIB |
| KB-002 | Semantic search | "Perlindungan data pribadi" | Relevant results | 🔴 WAJIB |
| KB-003 | Hybrid search | Same + keyword | Combined results | 🟡 PENTING |
| KB-004 | No results | "Nonexistent..." | Empty results gracefully | 🔴 WAJIB |
| KB-005 | Large result set | Popular query | Pagination working | 🟡 PENTING |

### Upload
| ID | Test Case | File | Expected | Priority |
|----|-----------|------|----------|----------|
| KB-010 | Valid PDF | Legal doc | Chunked + indexed | 🔴 WAJIB |
| KB-011 | Large file | > 50MB | Error 413 | 🟡 PENTING |
| KB-012 | Invalid format | .exe | Error 400 | 🔴 WAJIB |
| KB-013 | Duplicate | Same doc twice | Deduplicated | 🟢 PENDUKUNG |

---

## License Validation Test Cases

| ID | Test Case | License | Expected | Priority |
|----|-----------|---------|----------|----------|
| LICENSE-001 | Valid license | Active personal | Access granted | 🔴 WAJIB |
| LICENSE-002 | Expired license | Expired by 1 day | Error 403 | 🔴 WAJIB |
| LICENSE-003 | Grace period | Within 7 days | Warning, still access | 🔴 WAJIB |
| LICENSE-004 | Trial license | < 30 days left | Trial OK | 🟡 PENTING |
| LICENSE-005 | Team license | 5 users | Team access working | 🟡 PENTING |
| LICENSE-006 | Offline license | No network | License valid offline | 🔴 WAJIB |

---

## Security Test Cases (OWASP Top 10)

| ID | Vulnerability | Test | Expected | Priority |
|----|---------------|------|----------|----------|
| SEC-001 | SQL Injection | Query: `"; DROP TABLE users; --` | Escaped safely | 🔴 WAJIB |
| SEC-002 | XSS | HTML in query: `<script>alert()` | Rendered as text | 🔴 WAJIB |
| SEC-003 | CSRF | Cross-origin request | Rejected or CSRF token checked | 🔴 WAJIB |
| SEC-004 | Auth bypass | Modify JWT | Token validation fails | 🔴 WAJIB |
| SEC-005 | API key leak | Expose key in logs | Key never logged | 🔴 WAJIB |
| SEC-006 | Path traversal | `../../../etc/passwd` | Blocked | 🟡 PENTING |
| SEC-007 | Rate limiting | 1000 requests/sec | Throttled to limit | 🟡 PENTING |

---

## Export Test Cases

| ID | Test Case | Content | Format | Expected | Priority |
|----|-----------|---------|--------|----------|----------|
| EXPORT-001 | Simple chat | 5 messages | PDF | PDF generated | 🔴 WAJIB |
| EXPORT-002 | With citations | Cited content | DOCX | Citations preserved | 🔴 WAJIB |
| EXPORT-003 | Large chat | 500 messages | PDF | Handles gracefully | 🟡 PENTING |
| EXPORT-004 | Special chars | Indonesian chars | DOCX | Unicode OK | 🟡 PENTING |

---

## Performance Benchmarks

| Operation | Target | Acceptance |
|-----------|--------|-----------|
| Login | < 500ms | ✅ P95 < 1s |
| Chat response (local LLM) | < 3s | ✅ P95 < 5s |
| Chat response (API) | < 2s | ✅ P95 < 4s |
| Search (< 1000 results) | < 500ms | ✅ P95 < 1s |
| Export PDF | < 3s | ✅ P95 < 5s |
| License validation | < 100ms | ✅ P95 < 200ms |

---

## Checklist Implementasi

- [ ] All test cases automated
- [ ] Test data fixtures created
- [ ] CI integration complete
- [ ] Manual testing documented
- [ ] Regression tests passing
- [ ] Performance benchmarks met
- [ ] Security tests passing
- [ ] Test report generated

