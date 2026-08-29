---
title: SPEC-GRAPH — Spesifikasi Custom Graph Engine
document_id: SPEC-GRAPH
version: 1.0
cb_reference: [CB §8], [CB §14]
status: DRAFT
owner: Backend Team
last_updated: 2026-08-29
---

# SPEC-GRAPH — Spesifikasi Custom Graph Engine

Detail implementasi state machine 11 fase.

## Referensi CB
- [CB §8] — Graph engine dan state machine
- [CB §14] — Phase transitions dan workflow

---

## State Machine Diagram

```mermaid
stateDiagram-v2
    [*] --> Phase0: Start Session
    
    Phase0 --> Phase1: Init Complete
    Phase1 --> Phase2: Intent Classified
    Phase2 --> Phase3: Context Retrieved
    Phase3 --> Phase4: Initial Response
    Phase4 --> Phase5: Citations Generated
    Phase5 --> Phase6: Validated
    Phase6 --> Phase7: Confidence Score
    
    Phase7 --> Phase8: Low Confidence?
    Phase8 --> Phase9: Web Research Done
    Phase9 --> Phase10: KB Updated
    Phase10 --> Phase11: Refinement
    
    Phase7 --> Phase9: High Confidence?
    Phase11 --> Phase12: Final Validation
    Phase12 --> [*]: Completion
    
    Phase3 -.->|Error| Recovery: Error Handler
    Phase6 -.->|Critical Error| Recovery: Error Handler
    Recovery --> Phase12: Resume
```

---

## Phase Specifications

### Phase 0: Initialization
- **Input:** User query, session context
- **Process:**
  - Load previous session messages (if resuming)
  - Initialize conversation state
  - Set up error handlers
  - Allocate resources
- **Output:** Session state, context loaded
- **Duration Target:** < 500ms
- **Error Handling:** Graceful initialization failure

```rust
pub struct Phase0State {
    session_id: Uuid,
    user_id: Uuid,
    initial_query: String,
    previous_messages: Vec<Message>,
    system_prompt: String,
    start_time: Instant,
}
```

### Phase 1: Query Understanding
- **Input:** User query
- **Process:**
  - Classify intent (legal question, research request, document analysis)
  - Extract keywords and entities
  - Identify document type needed
  - Detect language
- **Output:** Intent classification, keywords
- **Duration Target:** < 1s
- **Metrics:** Intent confidence score

```rust
pub enum Intent {
    LegalQuestion,
    ResearchRequest,
    DocumentAnalysis,
    Citation,
    General,
}

pub struct Phase1Output {
    intent: Intent,
    confidence: f32,
    keywords: Vec<String>,
    entities: Vec<Entity>,
}
```

### Phase 2: Context Retrieval
- **Input:** Intent, keywords
- **Process:**
  - Query knowledge base (semantic + keyword)
  - Retrieve top 5-10 relevant documents
  - Extract relevant sections (Pasal, BAB)
  - Calculate relevance scores
- **Output:** Relevant context documents
- **Duration Target:** < 2s
- **Metrics:** Retrieval precision, recall

### Phase 3: Initial Response
- **Input:** Context documents, query
- **Process:**
  - Format context for LLM
  - Call LLM with context
  - Stream response to user
  - Collect response chunks
- **Output:** Draft response
- **Duration Target:** < 15s (including LLM latency)
- **Provider:** Multi-provider LLM

### Phase 4: Citation Generation
- **Input:** Draft response, context
- **Process:**
  - Extract claim statements from response
  - Match claims to context sources
  - Generate inline citations
  - Validate citation accuracy
- **Output:** Response with citations
- **Duration Target:** < 2s
- **Format:** `[CB] Pasal 12 Ayat 3` atau `[Web] URL`

### Phase 5: Validation
- **Input:** Response with citations
- **Process:**
  - Verify each citation exists in KB
  - Check citation accuracy
  - Verify pasal/section references
  - Flag suspicious citations
- **Output:** Validation report
- **Duration Target:** < 3s
- **Pass Rate:** Must be 100% (no hallucinated citations)

### Phase 6: Hallucination Check
- **Input:** Validated response
- **Process:**
  - Compare response against KB
  - Check for made-up regulations
  - Detect contradictions with KB
  - Calculate hallucination score
- **Output:** Hallucination confidence score
- **Duration Target:** < 2s
- **Threshold:** 0% hallucination tolerance

```rust
pub struct HallucinationReport {
    hallucination_score: f32,  // 0.0 = no hallucination, 1.0 = full hallucination
    suspicious_claims: Vec<String>,
    confidence: f32,
}
```

### Phase 7: Confidence Evaluation
- **Input:** Hallucination score, validation report
- **Process:**
  - Aggregate confidence from all previous phases
  - Calculate overall confidence score
  - Determine if web research needed
  - Set confidence threshold
- **Output:** Confidence score, research needed flag
- **Duration Target:** < 500ms
- **Threshold:** < 0.7 → trigger web research

### Phase 8: Web Research (Conditional)
- **Input:** Confidence score, original query
- **Process:**
  - Generate search terms if needed
  - Query whitelisted web sources only
  - Extract relevant content
  - Verify content quality
  - Link to KB entries
- **Output:** Web research results
- **Duration Target:** < 10s
- **Condition:** Only if confidence < 0.7
- **Error Handling:** Skip if web unavailable

### Phase 9: Knowledge Base Update
- **Input:** Web research results
- **Process:**
  - Extract new legal information
  - Chunk and embed content
  - Store in KB with source tracking
  - Link to original sources
  - Update related entries
- **Output:** Updated KB
- **Duration Target:** < 3s
- **Safety:** No duplicate entries

### Phase 10: Refinement
- **Input:** Updated KB, original query
- **Process:**
  - Call LLM again with new information
  - Generate refined response
  - Regenerate citations
  - Stream refined response
- **Output:** Refined response
- **Duration Target:** < 15s
- **Improvement Metric:** Confidence increase

### Phase 11: Final Validation
- **Input:** Refined response
- **Process:**
  - Run same validation as Phase 5
  - Verify no new hallucinations
  - Check response consistency
  - Format output
- **Output:** Validation passed/failed
- **Duration Target:** < 2s
- **Pass Rate:** 100%

### Phase 12: Completion
- **Input:** Validated response
- **Process:**
  - Format final response
  - Attach metadata (timing, sources, confidence)
  - Store in session history
  - Clean up temporary data
  - Mark session complete
- **Output:** Final result + metadata
- **Duration Target:** < 1s

---

## Conditional Edges

| From Phase | To Phase | Condition | Logic |
|-----------|----------|-----------|-------|
| 7 | 8 | confidence < 0.7 | Trigger web research |
| 7 | 12 | confidence >= 0.7 | Skip web research |
| 8 | Error | web unavailable | Continue to Phase 10 anyway |
| 10 | 11 | Always | Next phase |
| 11 | 12 | validation passed | Continue |
| 11 | Error | validation failed | Restart with fallback |

---

## Error Handling & Recovery

### Error Types

```rust
pub enum GraphError {
    SessionNotFound,
    LLMProviderError(String),
    KnowledgeBaseError(String),
    ValidationError(String),
    WebResearchError(String),
    DatabaseError(String),
    TimeoutError,
}
```

### Recovery Strategy

```
Error at Phase N
├─ Retriable? (network, timeout)
│   ├─ Yes → Retry with backoff (max 3x)
│   └─ Fail → Use fallback
├─ Fallback? (cached response, previous KB)
│   ├─ Yes → Return fallback with warning
│   └─ No → Return partial result
└─ User notification
    ├─ Graceful degradation message
    └─ Suggest retry later
```

### Fallback Strategy
- **Phase 0-2:** Fail fast, no fallback
- **Phase 3:** Use previous response if available
- **Phase 4-6:** Return response without citations
- **Phase 7-12:** Return best-effort response with confidence score

---

## Event Streaming

### Event Types

```rust
pub enum GraphEvent {
    PhaseStart { phase: u8, timestamp: Instant },
    PhaseEnd { phase: u8, duration: Duration },
    ResponseChunk { content: String },
    CitationFound { citation: Citation },
    SourceAdded { source: String },
    WarningIssued { message: String },
    ErrorOccurred { error: String },
    Complete { metadata: Metadata },
}
```

### WebSocket Protocol

```json
{
  "type": "phase_start",
  "phase": 3,
  "timestamp": "2026-08-29T10:30:00Z"
}
```

```json
{
  "type": "response_chunk",
  "content": "This is a chunk of text from the LLM...",
  "phase": 3
}
```

```json
{
  "type": "citation_found",
  "citation": {
    "source_type": "uu",
    "source_reference": "UU No. 8 Tahun 2024",
    "section": "Pasal 12 Ayat 3",
    "excerpt": "..."
  },
  "phase": 5
}
```

---

## Cancellation Mechanism

### User-Initiated Cancellation

```
User clicks Cancel
│
├─ Current phase < 3 → Immediate termination
├─ Current phase 3-6 → Finish current LLM call, then stop
├─ Current phase 7-12 → Finish current phase, then stop
│
└─ Cleanup
    ├─ Release resources
    ├─ Cancel pending requests
    ├─ Save partial session
    └─ Return partial result
```

### System-Initiated Cancellation

```
Timeout or Resource Limit Exceeded
│
├─ Phase duration > 30s → Cancel
├─ Memory usage > 500MB → Cancel
├─ Token usage > limit → Cancel
│
└─ Graceful shutdown + save state
```

---

## State Persistence

### Session State Structure

```json
{
  "session_id": "uuid",
  "user_id": "uuid",
  "current_phase": 5,
  "start_time": "2026-08-29T10:30:00Z",
  "messages": [...],
  "context": {
    "intent": "legal_question",
    "keywords": ["UU No. 8", "Pasal 12"],
    "relevant_documents": [...]
  },
  "confidence_scores": {
    "intent_confidence": 0.95,
    "hallucination_score": 0.1,
    "overall_confidence": 0.87
  },
  "citations": [...],
  "web_research_results": [...],
  "errors": [...]
}
```

### Persistence Strategy
- Auto-save to database after each phase
- Enable session resumption on disconnect
- Keep session for 24 hours or until manual delete
- Audit trail of all phase transitions

---

## Test Scenarios for Each Phase

### Phase 0: Initialization Tests
- [ ] New session creation
- [ ] Session resumption
- [ ] Resource allocation
- [ ] Error handling on init

### Phase 1: Query Understanding Tests
- [ ] Correct intent classification
- [ ] Keyword extraction
- [ ] Entity recognition
- [ ] Non-legal query handling

### Phase 2: Context Retrieval Tests
- [ ] Semantic search accuracy
- [ ] Keyword search accuracy
- [ ] Hybrid search ranking
- [ ] No results handling

### Phase 3: Initial Response Tests
- [ ] LLM response quality
- [ ] Response streaming
- [ ] Context injection
- [ ] Provider fallback

### Phase 4: Citation Generation Tests
- [ ] Citation accuracy
- [ ] Citation format consistency
- [ ] Missing citation detection
- [ ] Hallucinated citation detection

### Phase 5: Validation Tests
- [ ] Valid citations pass
- [ ] Invalid citations fail
- [ ] False positive handling
- [ ] Edge cases (short pasal names)

### Phase 6: Hallucination Check Tests
- [ ] Detects hallucinated regulations
- [ ] Detects false pasal numbers
- [ ] Allows legitimate interpretations
- [ ] Confidence score accuracy

### Phase 7: Confidence Evaluation Tests
- [ ] Score calculation
- [ ] Threshold comparison
- [ ] Web research triggering
- [ ] Score range (0-1)

### Phase 8: Web Research Tests
- [ ] Whitelisted domains only
- [ ] Robots.txt compliance
- [ ] Content extraction
- [ ] Timeout handling

### Phase 9: KB Update Tests
- [ ] Duplicate prevention
- [ ] Embedding generation
- [ ] KB consistency
- [ ] Link creation

### Phase 10: Refinement Tests
- [ ] Improved response quality
- [ ] Citation regeneration
- [ ] Confidence improvement
- [ ] No regression

### Phase 11: Final Validation Tests
- [ ] Validation consistency
- [ ] No new hallucinations
- [ ] Response completeness
- [ ] Format compliance

### Phase 12: Completion Tests
- [ ] Metadata correctness
- [ ] Session storage
- [ ] Timing accuracy
- [ ] Resource cleanup

---

## Performance Metrics

| Phase | Target | SLA | Notes |
|-------|--------|-----|-------|
| 0 | 500ms | 95th percentile | Initialization |
| 1 | 1s | 95th percentile | Fast intent classification |
| 2 | 2s | 95th percentile | DB query time |
| 3 | 15s | 95th percentile | Includes LLM latency |
| 4 | 2s | 95th percentile | Post-processing |
| 5 | 3s | 95th percentile | Validation |
| 6 | 2s | 95th percentile | Scoring |
| 7 | 500ms | 95th percentile | Decision point |
| 8 | 10s | 95th percentile | Web fetch |
| 9 | 3s | 95th percentile | KB update |
| 10 | 15s | 95th percentile | LLM refinement |
| 11 | 2s | 95th percentile | Validation |
| 12 | 1s | 95th percentile | Cleanup |
| **Total** | **< 60s** | **99th percentile** | End-to-end SLA |

---

## Monitoring & Observability

### Metrics to Track
- Phase duration per stage
- Error rate per phase
- Confidence score distribution
- Web research trigger rate
- Hallucination detection rate
- User cancellation rate

### Dashboards
- Real-time phase progress
- Error rate trends
- Confidence score trends
- Performance SLA compliance

### Alerts
- Phase timeout (> target duration)
- High error rate (> 5%)
- High hallucination rate (> 0%)
- Web research failures

---

## Checklist Implementasi

- [ ] State machine implementation in Rust
- [ ] All 11 phases implemented
- [ ] Event streaming via WebSocket
- [ ] Error handling and recovery
- [ ] Cancellation mechanism
- [ ] State persistence
- [ ] Comprehensive test coverage (>80%)
- [ ] Performance benchmarks
- [ ] Monitoring and alerting

