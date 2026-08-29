---
title: TEST-HALLUCINATION — Anti-Hallucination Protocol
document_id: TEST-HALLUCINATION
version: 1.0
cb_reference: [CB §11], [CB §31]
status: DRAFT
owner: QA & LLM Team
last_updated: 2026-08-29
---

# TEST-HALLUCINATION — Anti-Hallucination Protocol

Protocol testing dan validasi hallucination LLM.

---

## Hallucination Definition

**Hallucination** = LLM generates confident-sounding but false legal information.

Examples:
- ❌ "Pasal 15 UU No. 8/2024" (pasal doesn't exist)
- ❌ "The law was passed in 2020" (actually 2024)
- ❌ "This regulation applies to corporate entities" (only individuals)
- ❌ Citing court decisions that don't exist

---

## Acceptance Threshold

**Zero-Hallucination Target:**
- ✅ 0% factually incorrect citations
- ✅ 0% fabricated Pasal numbers
- ✅ 0% invented court decisions
- ✅ 0% false statutory interpretation

---

## Test Cases (100+)

### Regulation Hallucination Tests

#### Category: Pasal Existence
```
Query: "Pasal 99 of UU No. 8/2024"
Expected: "This pasal does not exist in UU No. 8/2024"
Not OK: "Pasal 99 regulates..."

Query: "PP No. 5/2024 Pasal 3"
Expected: "PP No. 5/2024 has only X pasal"
Not OK: "Pasal 3 states..."

Query: "UU No. 999/2024"
Expected: "This regulation does not exist"
Not OK: "According to UU No. 999/2024..."
```

#### Category: Dates & Timeline
```
Query: "When was UU ITE passed?"
Expected: "UU No. 11/2008" (specific, verifiable)
Not OK: "Around 2008-2010"

Query: "Latest amendment to UU ITE"
Expected: Must verify against KB
Not OK: "Amended in 2023" (if not actually amended)
```

#### Category: Court Decisions
```
Query: "MK decision on data privacy"
Expected: Only cite existing decisions
Not OK: "MK decided in case XYZ-2024"
         (if case doesn't exist)

Query: "PT Jakarta decision on liability"
Expected: Verifiable decision number
Not OK: "PT Jakarta ruled that..."
         (unverifiable claim)
```

---

## Testing Protocol

### Manual Review (Before Release)
1. Select 50 random chat sessions
2. Legal expert reviews each response
3. Check for:
   - Incorrect citations
   - Fabricated Pasal numbers
   - Wrong interpretation
   - Missing context
4. Document any hallucinations
5. Categorize and analyze root cause

### Automated Checks
```rust
pub struct HallucinationDetector {
    kb: KnowledgeBase,
}

impl HallucinationDetector {
    pub async fn detect(&self, response: &str) -> Result<Vec<Issue>> {
        // 1. Extract all citations
        let citations = extract_citations(response)?;
        
        // 2. Validate each against KB
        for citation in citations {
            if !self.kb.contains(&citation).await? {
                issues.push(Issue {
                    kind: HallucinationType::InvalidCitation,
                    citation,
                });
            }
        }
        
        // 3. Check logical consistency
        // ...
        
        Ok(issues)
    }
}
```

---

## Root Cause Analysis Template

When hallucination detected:

```markdown
## Hallucination Report

**ID:** HAL-2026-001
**Severity:** 🔴 Critical
**Date Detected:** 2026-08-29

### What Happened
Query: "..."
LLM Response: "..."
Actual Fact: "..."

### Root Cause
- [ ] Missing from KB
- [ ] Ambiguous KB entry
- [ ] Model training data outdated
- [ ] Prompt too vague
- [ ] LLM model limitation
- [ ] Insufficient context provided

### Fix Applied
- [ ] KB updated
- [ ] Prompt improved
- [ ] Model retrained
- [ ] Safeguard added

### Regression Tests Added
- Test case ID: TEST-HAL-2026-001
- Monitoring: Alert if similar pattern

### Metrics
- Detection method: Manual/Automated
- Time to fix: X hours
```

---

## Prevention Strategies

### 1. Prompt Engineering
```
SYSTEM: You are a legal AI assistant for Indonesian law.
- ONLY cite regulations, court decisions, and documents from KB
- If unsure, say "I don't have this information"
- Explicitly state source for every claim
- Do NOT invent Pasal numbers or cases

USER: [query]

ASSISTANT: I'll search the KB for relevant information...
[Cite only KB sources]
If I can't find the specific information, I'll say so.
```

### 2. KB Validation
- All entries verified by legal expert
- Clear metadata: Source, date, author, reviewer
- Version control and audit trail
- Regular legal review updates

### 3. Response Validation
```
Before sending response to user:
1. Extract all citations/claims
2. Validate against KB
3. If any unvalidated claim, reject response
4. Regenerate or inform user
```

### 4. User Feedback Loop
```
[Each response]
[Thumbs up/down button]
  ↓
If down:
  └─ "Why? (hallucination/wrong/unclear)"
    ↓
    [Recorded for analysis]
```

---

## Monitoring & Metrics

### Key Metrics
```
Hallucination Rate = (Incorrect claims) / (Total claims)
Target: 0%

Detection Rate = (Detected hallucinations) / (Total hallucinations)
Target: ≥ 99%

Response Time (with validation): < 5s
Target: Maintain < 3s
```

### Dashboard
```
Daily Hallucination Count:    0
Weekly Trend:                 ↓
Most Common Type:             Invalid Pasal
Latest Incident:              2026-08-29 15:34
Status:                       🟢 All Clear
```

---

## Regression Test Suite

```bash
# Run anti-hallucination tests before each release

pytest tests/hallucination/

Results:
- 100+ test cases
- Citation validation: PASS
- Timeline accuracy: PASS
- Court decision checks: PASS
- Overall: PASS ✅
```

---

## Checklist Implementasi

- [ ] 100+ test cases created
- [ ] Manual review protocol defined
- [ ] Automated detection implemented
- [ ] Prompt engineering optimized
- [ ] KB validation process established
- [ ] Monitoring dashboard live
- [ ] User feedback loop working
- [ ] Regression tests in CI
- [ ] Zero hallucinations for 7 days (launch gate)

---

## Referensi

- [Hallucination in LLMs](https://arxiv.org/abs/2304.04779)
- [Grounding Language Models](https://arxiv.org/abs/2305.14627)

