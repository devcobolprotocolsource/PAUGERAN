---
title: SPEC-KB — Spesifikasi Legal Knowledge Base
document_id: SPEC-KB
version: 1.0
cb_reference: [CB §11]
status: DRAFT
owner: Backend Team
last_updated: 2026-08-29
---

# SPEC-KB — Spesifikasi Legal Knowledge Base

Detail implementasi basis pengetahuan hukum.

## Referensi CB
- [CB §11] — Knowledge base architecture dan management

---

## Storage Schema

### Knowledge Base Structure

```sql
-- Knowledge base (collection of documents)
CREATE TABLE knowledge_base (
    kb_id UUID PRIMARY KEY,
    title TEXT NOT NULL,
    description TEXT,
    is_system BOOLEAN DEFAULT FALSE,  -- System KB cannot be deleted
    language TEXT DEFAULT 'id',       -- Language code
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);

-- Documents
CREATE TABLE kb_documents (
    document_id UUID PRIMARY KEY,
    kb_id UUID REFERENCES knowledge_base,
    title TEXT NOT NULL,
    document_type TEXT CHECK (type IN ('uu', 'pp', 'putusan', 'document')),
    full_text TEXT NOT NULL,
    metadata JSONB,  -- {year, number, date, source, url}
    document_hash TEXT UNIQUE,  -- For deduplication
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);

-- Chunks (for embedding)
CREATE TABLE kb_chunks (
    chunk_id UUID PRIMARY KEY,
    document_id UUID REFERENCES kb_documents ON DELETE CASCADE,
    chunk_index INTEGER,
    chunk_text TEXT NOT NULL,
    token_count INTEGER,
    created_at TIMESTAMP
);

-- Embeddings
CREATE TABLE embeddings (
    embedding_id UUID PRIMARY KEY,
    chunk_id UUID REFERENCES kb_chunks ON DELETE CASCADE,
    embedding vector(1536),  -- Anthropic Claude embeddings
    model TEXT DEFAULT 'claude-3-embedding-v1',
    created_at TIMESTAMP
);
```

---

## Metadata Schema

### Document Metadata

```json
{
  "document_id": "uuid",
  "document_type": "uu",
  "metadata": {
    "year": 2024,
    "number": 8,
    "full_name": "Undang-Undang Nomor 8 Tahun 2024",
    "source": "jdih.kemenkumham.go.id",
    "url": "https://...",
    "effective_date": "2024-01-01",
    "status": "in_force",
    "revisions": [
      {
        "type": "amendment",
        "document_id": "uuid-of-amendment",
        "date": "2025-01-01"
      }
    ],
    "tags": ["hukum-pidana", "korupsi", "aset"],
    "authority": "Dewan Perwakilan Rakyat"
  }
}
```

### Pasal/Section Metadata

```json
{
  "pasal_number": "12",
  "ayat": "3",
  "jo": "Pasal 14 Ayat 2",  // Jo = juncto (conjunction)
  "text": "Setiap orang yang melanggar...",
  "related_pasals": ["13", "14", "15"],
  "punishments": ["penjara", "denda"],
  "last_amendment_date": "2024-01-01"
}
```

---

## Embedding Generation

### Strategy

```rust
pub struct EmbeddingStrategy {
    model: String,           // "claude-3-embedding"
    chunk_size: usize,       // 512 tokens
    chunk_overlap: usize,    // 100 tokens
    batch_size: usize,       // Process 10 chunks at once
}

impl EmbeddingStrategy {
    pub async fn embed_document(
        &self,
        document: &KBDocument,
        llm_provider: &LlmProvider,
    ) -> Result<Vec<EmbeddingChunk>> {
        // 1. Chunk document
        let chunks = self.chunk_text(&document.full_text)?;
        
        // 2. Batch embed
        let embeddings = llm_provider.embed_batch(&chunks).await?;
        
        // 3. Store with metadata
        // ...
        
        Ok(embeddings)
    }
    
    fn chunk_text(&self, text: &str) -> Result<Vec<Chunk>> {
        // Tokenize using tiktoken or similar
        // Chunk based on token count, not character count
        // Preserve pasal boundaries when possible
        // Maintain chunk overlap for context
    }
}
```

### Embedding Dimensions
- **Model:** Anthropic Claude 3 Embeddings
- **Dimension:** 1536
- **Cost:** ~$0.02 per M tokens

---

## Search Algorithms

### Semantic Search

```rust
pub struct SemanticSearch {
    vector_db: PgvectorDB,
    similarity_threshold: f32,  // Default: 0.7
}

impl SemanticSearch {
    pub async fn search(
        &self,
        query: &str,
        limit: usize,
    ) -> Result<Vec<SearchResult>> {
        // 1. Embed query
        let query_embedding = embed_query(query).await?;
        
        // 2. Vector similarity search (cosine)
        let results = self.vector_db.similarity_search(
            &query_embedding,
            limit,
            self.similarity_threshold,
        ).await?;
        
        // 3. Re-rank by relevance
        let ranked = self.rerank(results).await?;
        
        Ok(ranked)
    }
}
```

**Performance:**
- Latency: < 500ms for 1M documents
- Accuracy: ~0.85 NDCG@10

### Keyword Search

```rust
pub struct KeywordSearch {
    full_text_index: PostgresFullText,
    weights: HashMap<String, f32>,  // Field weights
}

impl KeywordSearch {
    pub async fn search(
        &self,
        query: &str,
        limit: usize,
    ) -> Result<Vec<SearchResult>> {
        // Use PostgreSQL full-text search
        // Weight: title (1.0) > section (0.8) > content (0.5)
        
        let sql = r#"
            SELECT * FROM kb_chunks
            WHERE to_tsvector('indonesian', chunk_text) @@ plainto_tsquery('indonesian', $1)
            ORDER BY ts_rank(to_tsvector('indonesian', chunk_text), 
                             plainto_tsquery('indonesian', $1)) DESC
            LIMIT $2
        "#;
        
        // Execute query
    }
}
```

**Performance:**
- Latency: < 100ms for 1M documents
- Accuracy: Exact matches only

### Hybrid Search

```rust
pub struct HybridSearch {
    semantic: SemanticSearch,
    keyword: KeywordSearch,
}

impl HybridSearch {
    pub async fn search(
        &self,
        query: &str,
        limit: usize,
        semantic_weight: f32,  // 0.5 = 50/50
    ) -> Result<Vec<SearchResult>> {
        // 1. Run both searches in parallel
        let (semantic_results, keyword_results) = tokio::join!(
            self.semantic.search(query, limit * 2),
            self.keyword.search(query, limit * 2)
        );
        
        // 2. Merge and re-rank using weighted score
        let merged = merge_and_rerank(
            semantic_results?,
            keyword_results?,
            semantic_weight,
        );
        
        // 3. Return top N
        Ok(merged.into_iter().take(limit).collect())
    }
}
```

---

## Document Chunking Strategy

### Intelligent Chunking

```rust
pub struct ChunkingStrategy {
    target_tokens: usize,       // 512
    overlap_tokens: usize,      // 100
}

impl ChunkingStrategy {
    pub fn chunk_document(&self, doc: &KBDocument) -> Result<Vec<Chunk>> {
        let tokenized = tokenize(&doc.full_text)?;
        let mut chunks = Vec::new();
        let mut current_chunk = Vec::new();
        
        for token in tokenized {
            current_chunk.push(token);
            
            if current_chunk.len() >= self.target_tokens {
                // Save current chunk
                chunks.push(Chunk::from_tokens(current_chunk.clone()));
                
                // Overlap
                current_chunk = current_chunk
                    .iter()
                    .skip(self.target_tokens - self.overlap_tokens)
                    .cloned()
                    .collect();
            }
        }
        
        // Save last chunk
        if !current_chunk.is_empty() {
            chunks.push(Chunk::from_tokens(current_chunk));
        }
        
        Ok(chunks)
    }
}
```

### Boundary-Aware Chunking

```
Document
├─ Pasal 1
│  ├─ Ayat 1: tokens 0-150
│  ├─ Ayat 2: tokens 151-300
│  └─ Ayat 3: tokens 301-400
├─ Pasal 2
│  ├─ Ayat 1: tokens 401-550
│  ├─ Ayat 2: tokens 551-700
│  ...

Chunks (respecting Pasal boundaries):
├─ Chunk 1: Pasal 1 (Ayat 1-3) - 400 tokens
├─ Chunk 2: Pasal 2 (Ayat 1-2 + overlap) - 550 tokens
├─ Chunk 3: Pasal 3 (Ayat 1-3 + overlap) - 500 tokens
...
```

---

## Metadata Extraction

### From Regulations (UU, PP)

```rust
pub struct MetadataExtractor {
    patterns: Vec<Regex>,
}

impl MetadataExtractor {
    pub fn extract_uu_metadata(&self, text: &str) -> Result<UuMetadata> {
        // Extract: Undang-Undang Nomor X Tahun Y
        let uu_number_pattern = r"Nomor\s+(\d+)\s+Tahun\s+(\d{4})";
        
        // Extract effective date
        let effective_pattern = r"mulai berlaku pada tanggal.*?(\d{1,2}\s+\w+\s+\d{4})";
        
        // Extract amendments
        // ...
        
        Ok(UuMetadata {
            number: extracted_number,
            year: extracted_year,
            effective_date: extracted_date,
            amendments: vec![],
        })
    }
}
```

### From Court Decisions (Putusan)

```rust
pub fn extract_putusan_metadata(&self, text: &str) -> Result<PutusanMetadata> {
    // Extract: Putusan Nomor X/Pid/X/PN.Xxx
    let putusan_pattern = r"Putusan\s+Nomor\s+([\w./]+)";
    
    // Extract: Mengadili atau Memeriksa
    let pengadilan_pattern = r"PENGADILAN\s+(\w+)";
    
    // Extract date, judges, parties
    // ...
}
```

---

## Update & Versioning Strategy

### Conflict Resolution

When multiple versions of same regulation exist:

```
Regulation: UU No. 8 Tahun 2024

Versions:
├─ v1.0 (2024-01-01) — Original enactment
├─ v1.1 (2024-06-01) — Amendment by Law No. 3/2024
└─ v2.0 (2024-12-01) — Repeal by Law No. 10/2024

Search Results:
├─ If asking about current law → Show v2.0
├─ If asking about historical → Show applicable version at date
├─ If ambiguous → Show all versions with dates
```

### Version Tracking

```sql
CREATE TABLE document_versions (
    version_id UUID PRIMARY KEY,
    document_id UUID REFERENCES kb_documents,
    version_number INTEGER,
    full_text TEXT,
    amendments_applied JSONB,  -- [{ amendment_id, date }]
    effective_date TIMESTAMP,
    expires_date TIMESTAMP,  -- NULL if still valid
    created_at TIMESTAMP
);
```

---

## Bulk Import Format

### CSV Format

```csv
document_type,title,content,metadata_json,source_url
uu,"Undang-Undang Nomor 8 Tahun 2024","Pasal 1...","{"year": 2024, "number": 8}","https://..."
pp,"Peraturan Pemerintah Nomor 10 Tahun 2024","Pasal 1...","{"year": 2024, "number": 10}","https://..."
```

### JSON Line Format

```jsonl
{"document_type":"uu","title":"...","content":"...","metadata":{...}}
{"document_type":"pp","title":"...","content":"...","metadata":{...}}
```

### Import Procedure

```rust
pub async fn bulk_import(
    kb_id: Uuid,
    file_path: &str,
    format: ImportFormat,  // CSV, JSONL
) -> Result<ImportSummary> {
    // 1. Validate file
    validate_file(file_path)?;
    
    // 2. Parse
    let records = parse_file(file_path, format)?;
    
    // 3. Deduplicate (by document hash)
    let unique_records = deduplicate(records)?;
    
    // 4. Process in batches
    for batch in unique_records.chunks(100) {
        // 4a. Insert documents
        insert_documents(kb_id, batch).await?;
        
        // 4b. Chunk and embed
        chunk_and_embed(batch).await?;
        
        // 4c. Index
        create_indices().await?;
    }
    
    // 5. Return summary
    Ok(ImportSummary {
        total: records.len(),
        imported: imported_count,
        duplicates: duplicate_count,
        errors: error_count,
    })
}
```

---

## Performance Benchmarks

| Operation | Target | Actual | Notes |
|-----------|--------|--------|-------|
| Semantic search | < 500ms | ~250ms | 1M documents |
| Keyword search | < 100ms | ~50ms | Index-based |
| Hybrid search | < 1s | ~400ms | Parallel execution |
| Document import | < 5s per doc | ~2s | Includes embedding |
| Chunk embedding | < 50ms per chunk | ~30ms | Batch size 10 |
| Vector similarity | < 100ms | ~50ms | 1M vectors |

---

## Monitoring & Maintenance

### Index Health

```sql
-- Check index size
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan,
    idx_tup_read,
    idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;
```

### Regular Maintenance

- Weekly: Analyze indexes, update statistics
- Monthly: Reindex large tables
- Quarterly: Vacuum full, optimize partitions

---

## Checklist Implementasi

- [ ] KB schema created (documents, chunks, embeddings)
- [ ] Embedding generation working (Anthropic Claude)
- [ ] Semantic search implemented with pgvector
- [ ] Keyword search implemented
- [ ] Hybrid search combining both
- [ ] Chunking strategy tested
- [ ] Metadata extraction automated
- [ ] Version tracking for conflicts
- [ ] Bulk import functionality
- [ ] Performance targets met
- [ ] Monitoring dashboards

---

## Referensi Tambahan

- [pgvector Documentation](https://github.com/pgvector/pgvector)
- [Full-Text Search in PostgreSQL](https://www.postgresql.org/docs/current/textsearch.html)
- [Embedding Best Practices](https://docs.anthropic.com/en/guides/embed-api)
