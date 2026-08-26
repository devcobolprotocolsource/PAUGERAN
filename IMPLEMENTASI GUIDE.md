# IMPLEMENTASI GUIDE — PAUGERAN BASELINE

**Dokumen:** End-to-End Implementation Guide
**Versi:** 1.0
**Referensi:** PRD Contract Baseline v1.0
**Status:** Siap Dieksekusi

---

## DAFTAR FASE

1. **Fase 1:** Setup Infrastruktur & Environment
2. **Fase 2:** Database & Data Layer
3. **Fase 3:** Backend API Core
4. **Fase 4:** Agent Orchestrator (LangGraph)
5. **Fase 5:** LLM Integration & RAG
6. **Fase 6:** Frontend Development
7. **Fase 7:** Integration & Testing
8. **Fase 8:** Deployment & Security
9. **Fase 9:** Monitoring & Launch Readiness

---

# FASE 1: SETUP INFRASTRUKTUR & ENVIRONMENT

## Tujuan
Membangun fondasi infrastruktur dan environment development yang siap untuk seluruh siklus pengembangan.

## Aturan Emas

**1.1 — Infrastruktur sebagai Kode (IaC)**
> Semua konfigurasi infrastruktur harus dapat direproduksi dari kode. Tidak ada konfigurasi manual yang tidak terdokumentasi.

**1.2 — Environment Parity**
> Environment development, staging, dan production harus sedekat mungkin identik. Perbedaan hanya pada data dan secrets.

**1.3 — Zero Trust Network**
> Tidak ada service yang terekspos ke internet kecuali melalui reverse proxy terenkripsi. Database dan cache tidak boleh diakses langsung dari luar.

**1.4 — Secret Management**
> Tidak ada API key, password, atau secret yang ditulis dalam kode atau commit ke repository. Semua secret dikelola melalui environment variables atau secret manager.

**1.5 — Reproducibility**
> Setiap developer harus bisa menjalankan seluruh stack dengan satu command (`pnpm dev` atau `docker-compose up`).

## Ceklist Implementasi

### 1.1 Repository Setup
- [ ] Buat repository Git dengan struktur monorepo (Turborepo)
- [ ] Setup `.gitignore` yang komprehensif (node_modules, .env, __pycache__, *.pyc, .DS_Store, dll)
- [ ] Setup `.nvmrc` dengan versi Node.js 20+
- [ ] Setup `.python-version` dengan versi Python 3.11+
- [ ] Buat `README.md` dengan instruksi setup awal
- [ ] Buat `CONTRIBUTING.md` dengan guidelines kontribusi
- [ ] Setup `LICENSE` file
- [ ] Buat `.editorconfig` untuk konsistensi editor

### 1.2 Package Manager & Workspace
- [ ] Install dan setup pnpm sebagai package manager
- [ ] Buat `pnpm-workspace.yaml` dengan definisi workspace
- [ ] Buat root `package.json` dengan scripts global (dev, build, test, lint)
- [ ] Setup Turborepo dengan `turbo.json`
- [ ] Buat folder `apps/` untuk aplikasi (web, api)
- [ ] Buat folder `packages/` untuk shared packages (shared, database, ui, config)
- [ ] Buat folder `infra/` untuk infrastruktur (docker, nginx, vps, monitoring)
- [ ] Buat folder `docs/` untuk dokumentasi
- [ ] Buat folder `tools/` untuk scripts dan utilities

### 1.3 Development Environment
- [ ] Buat `.env.example` dengan semua environment variables yang diperlukan
- [ ] Buat script `tools/scripts/setup.sh` untuk initial setup
- [ ] Buat script `tools/scripts/dev.sh` untuk menjalankan development environment
- [ ] Setup VS Code settings di `.vscode/settings.json`
- [ ] Setup VS Code extensions recommendations di `.vscode/extensions.json`
- [ ] Setup pre-commit hooks untuk linting dan formatting
- [ ] Setup ESLint configuration di `packages/config/eslint/`
- [ ] Setup Prettier configuration di `packages/config/prettier/`
- [ ] Setup TypeScript base configuration di `packages/config/typescript/`

### 1.4 Docker Infrastructure
- [ ] Buat `infra/docker/docker-compose.yml` untuk development
- [ ] Buat `infra/docker/docker-compose.prod.yml` untuk production
- [ ] Setup PostgreSQL 15 container dengan volume persistence
- [ ] Setup Redis 7 container dengan persistence enabled
- [ ] Setup Nginx container sebagai reverse proxy
- [ ] Buat Dockerfile untuk backend (Python 3.11+)
- [ ] Buat Dockerfile untuk frontend (Node 20+)
- [ ] Setup network isolation antar container
- [ ] Setup health checks untuk semua services
- [ ] Test `docker-compose up` berhasil menjalankan semua services

### 1.5 VPS Provisioning
- [ ] Provision VPS dengan spesifikasi minimum (4 vCPU, 8GB RAM, 100GB SSD)
- [ ] Pilih region Jakarta untuk data residency
- [ ] Install Ubuntu 22.04 LTS
- [ ] Setup SSH access dengan key-based authentication (disable password)
- [ ] Setup firewall (UFW) - hanya allow port 22, 80, 443
- [ ] Install Docker dan Docker Compose
- [ ] Install fail2ban untuk brute force protection
- [ ] Setup automatic security updates (unattended-upgrades)
- [ ] Buat user non-root untuk deployment
- [ ] Setup swap space (minimal 2GB)
- [ ] Test SSH access dan Docker commands

### 1.6 Domain & DNS
- [ ] Beli domain utama (misal: paugeran.com)
- [ ] Setup subdomain untuk frontend: `app.paugeran.com`
- [ ] Setup subdomain untuk API: `api.paugeran.com`
- [ ] Konfigurasi DNS records:
  - [ ] A record untuk VPS IP
  - [ ] CNAME untuk Vercel (frontend)
- [ ] Test DNS propagation
- [ ] Setup email untuk domain (untuk magic link, notifikasi)

### 1.7 Third-Party Services
- [ ] Daftar akun Anthropic untuk API Claude
- [ ] Daftar akun OpenAI untuk API GPT-4o
- [ ] Daftar akun LangSmith untuk observability
- [ ] Daftar akun Vercel untuk frontend hosting
- [ ] Daftar akun Sentry untuk error tracking (opsional)
- [ ] Daftar akun UptimeRobot untuk monitoring (opsional)
- [ ] Simpan semua API keys di `.env` (jangan commit!)
- [ ] Test semua API keys berfungsi

### 1.8 Documentation Setup
- [ ] Buat struktur folder `docs/`
- [ ] Buat `docs/README.md` sebagai index dokumentasi
- [ ] Buat folder `docs/prd/` dan salin PRD Contract Baseline
- [ ] Buat folder `docs/architecture/` untuk diagram arsitektur
- [ ] Buat folder `docs/api/` untuk API documentation
- [ ] Buat folder `docs/deployment/` untuk deployment guides
- [ ] Buat folder `docs/development/` untuk development guides
- [ ] Setup diagram tools (draw.io, Mermaid, atau Excalidraw)

## Output Fase 1
- ✅ Repository monorepo terstruktur
- ✅ Development environment dapat dijalankan dengan `pnpm dev`
- ✅ Docker Compose menjalankan PostgreSQL, Redis, Nginx
- ✅ VPS terprovision dan dapat diakses via SSH
- ✅ Domain dan DNS terkonfigurasi
- ✅ Semua API keys tersedia dan teruji
- ✅ Dokumentasi dasar tersedia

## Definition of Done
Developer baru dapat clone repository, menjalankan `pnpm install`, lalu `pnpm dev`, dan seluruh stack berjalan tanpa error.

---

# FASE 2: DATABASE & DATA LAYER

## Tujuan
Membangun lapisan data yang robust, terisolasi per thread, dan siap untuk menyimpan seluruh entitas PAUGERAN.

## Aturan Emas

**2.1 — Thread Isolation is Sacred**
> Tidak ada query yang boleh mengakses data dari thread lain. Setiap query WAJIB memfilter berdasarkan `thread_id`. Violasi isolasi thread adalah bug kritis.

**2.2 — Schema First**
> Semua entitas harus didefinisikan dalam schema database sebelum implementasi aplikasi. Perubahan schema harus melalui migration, bukan manual.

**2.3 — Data Integrity**
> Foreign keys, constraints, dan indexes harus memastikan integritas data. Tidak boleh ada orphan records atau data corruption.

**2.4 — Encryption at Rest**
> Data sensitif (dokumen, PII) harus dienkripsi saat disimpan di disk. Database dan file storage harus menggunakan enkripsi.

**2.5 — Backup is Non-Negotiable**
> Database harus dapat di-backup dan di-restore kapan saja. Backup harus teruji secara berkala.

## Ceklist Implementasi

### 2.1 Database Schema Design
- [ ] Design schema untuk tabel `users`
  - [ ] Kolom: id (UUID), email (unique), name, profession, created_at, last_login
  - [ ] Index pada email untuk fast lookup
- [ ] Design schema untuk tabel `chat_threads`
  - [ ] Kolom: id (UUID), user_id (FK), title, status, current_phase, facts_complete, created_at, updated_at
  - [ ] Index pada user_id dan status
  - [ ] Foreign key ke users dengan ON DELETE CASCADE
- [ ] Design schema untuk tabel `messages`
  - [ ] Kolom: id (UUID), thread_id (FK), role, content, phase, metadata (JSONB), created_at
  - [ ] Index pada thread_id
  - [ ] Foreign key ke chat_threads dengan ON DELETE CASCADE
- [ ] Design schema untuk tabel `facts`
  - [ ] Kolom: id (UUID), thread_id (FK), content, source, status, relevance, certainty, created_at
  - [ ] Index pada thread_id
  - [ ] Foreign key ke chat_threads dengan ON DELETE CASCADE
- [ ] Design schema untuk tabel `documents`
  - [ ] Kolom: id (UUID), thread_id (FK), filename, file_type, file_size, file_path, uploaded_at
  - [ ] Index pada thread_id
  - [ ] Foreign key ke chat_threads dengan ON DELETE CASCADE
- [ ] Design schema untuk tabel `legal_rules` (global)
  - [ ] Kolom: id (UUID), source, article, content, effective_date, revoked_date, metadata (JSONB), created_at
  - [ ] Index pada source dan article
- [ ] Design schema untuk tabel `traceability_edges`
  - [ ] Kolom: id (UUID), thread_id (FK), conclusion_id, reason, rule_id (FK), fact_id (FK), evidence_source, created_at
  - [ ] Index pada thread_id
  - [ ] Foreign keys ke chat_threads, legal_rules, facts
- [ ] Design schema untuk tabel `audit_logs`
  - [ ] Kolom: id (UUID), user_id (FK), thread_id (FK), action, details (JSONB), ip_address, created_at
  - [ ] Index pada user_id dan thread_id

### 2.2 Database Migration Setup
- [ ] Install Alembic untuk database migration
- [ ] Buat konfigurasi Alembic di `apps/api/alembic/`
- [ ] Setup `alembic.ini` dengan database URL dari environment
- [ ] Buat initial migration dengan semua tabel
- [ ] Test migration: `alembic upgrade head`
- [ ] Test rollback: `alembic downgrade -1`
- [ ] Dokumentasikan cara membuat migration baru
- [ ] Setup script untuk auto-generate migration dari model changes

### 2.3 Database Connection & ORM
- [ ] Install SQLAlchemy sebagai ORM
- [ ] Install asyncpg untuk async PostgreSQL driver
- [ ] Buat database connection pool configuration
- [ ] Setup connection pooling (pool_size=20, max_overflow=40)
- [ ] Implementasi database session management
- [ ] Buat dependency injection untuk database session
- [ ] Test connection pooling under load
- [ ] Setup connection health checks

### 2.4 Model Implementation
- [ ] Implementasi model `User` dengan SQLAlchemy
  - [ ] Definisi kolom dan tipe data
  - [ ] Relationships ke chat_threads
  - [ ] Validation rules
- [ ] Implementasi model `ChatThread`
  - [ ] Definisi kolom dan tipe data
  - [ ] Relationships ke messages, facts, documents, traceability_edges
  - [ ] Validation rules
  - [ ] Status enum (active, archived, deleted)
- [ ] Implementasi model `Message`
  - [ ] Definisi kolom dan tipe data
  - [ ] JSONB metadata handling
  - [ ] Validation rules
- [ ] Implementasi model `Fact`
  - [ ] Definisi kolom dan tipe data
  - [ ] Source enum (user_statement, document, verified)
  - [ ] Status enum (stated, supported, verified, disputed)
  - [ ] Certainty enum (high, medium, low)
- [ ] Implementasi model `Document`
  - [ ] Definisi kolom dan tipe data
  - [ ] File type validation
  - [ ] File size validation
- [ ] Implementasi model `LegalRule`
  - [ ] Definisi kolom dan tipe data
  - [ ] Date validation
  - [ ] Status tracking
- [ ] Implementasi model `TraceabilityEdge`
  - [ ] Definisi kolom dan tipe data
  - [ ] Relationships ke legal_rules dan facts
- [ ] Implementasi model `AuditLog`
  - [ ] Definisi kolom dan tipe data
  - [ ] JSONB details handling

### 2.5 Database Queries & Repositories
- [ ] Implementasi repository pattern untuk setiap model
- [ ] Buat query methods untuk User repository
  - [ ] find_by_id
  - [ ] find_by_email
  - [ ] create
  - [ ] update
  - [ ] delete
- [ ] Buat query methods untuk ChatThread repository
  - [ ] find_by_id (dengan thread isolation check)
  - [ ] find_by_user_id
  - [ ] create
  - [ ] update
  - [ ] delete (soft delete)
  - [ ] archive
- [ ] Buat query methods untuk Message repository
  - [ ] find_by_thread_id (dengan thread isolation check)
  - [ ] create
  - [ ] find_latest
- [ ] Buat query methods untuk Fact repository
  - [ ] find_by_thread_id (dengan thread isolation check)
  - [ ] create
  - [ ] update
  - [ ] delete
- [ ] Buat query methods untuk Document repository
  - [ ] find_by_thread_id (dengan thread isolation check)
  - [ ] create
  - [ ] delete
- [ ] Buat query methods untuk LegalRule repository
  - [ ] find_by_id
  - [ ] search_by_keyword
  - [ ] find_active_rules
- [ ] Buat query methods untuk TraceabilityEdge repository
  - [ ] find_by_thread_id (dengan thread isolation check)
  - [ ] create
  - [ ] find_by_conclusion_id

### 2.6 Thread Isolation Enforcement
- [ ] Implementasi middleware untuk thread isolation check
- [ ] Buat decorator `@require_thread_ownership` untuk endpoint
- [ ] Test thread isolation:
  - [ ] User A tidak bisa akses thread User B
  - [ ] Query tanpa thread_id filter akan error
  - [ ] API endpoint memvalidasi ownership
- [ ] Setup database-level constraints untuk isolasi
- [ ] Audit semua query untuk memastikan thread_id filter

### 2.7 Vector Database (pgvector)
- [ ] Install pgvector extension di PostgreSQL
- [ ] Buat tabel `legal_embeddings` untuk RAG
  - [ ] Kolom: id, rule_id, content, embedding (vector), created_at
  - [ ] Foreign key ke legal_rules
- [ ] Setup IVFFlat index untuk cosine similarity search
- [ ] Implementasi embedding generation function
- [ ] Implementasi similarity search function
- [ ] Test vector search performance
- [ ] Setup batch embedding generation untuk legal rules

### 2.8 Redis Cache Layer
- [ ] Install redis-py untuk Redis client
- [ ] Setup Redis connection pool
- [ ] Implementasi cache wrapper dengan TTL
- [ ] Buat cache keys untuk:
  - [ ] Thread state: `thread:{thread_id}:state`
  - [ ] Messages cache: `thread:{thread_id}:messages`
  - [ ] User session: `user:{user_id}:session`
  - [ ] Rate limiting: `api:rate_limit:{user_id}`
  - [ ] Legal embeddings: `rag:embedding:{rule_id}`
- [ ] Setup cache invalidation strategy
- [ ] Test cache hit/miss scenarios
- [ ] Monitor cache performance

### 2.9 File Storage
- [ ] Setup file storage directory structure:
  ```
  /opt/paugeran/data/documents/
  ├── {user_id}/
  │   ├── {thread_id}/
  │   │   ├── original/
  │   │   ├── processed/
  │   │   └── metadata.json
  ```
- [ ] Implementasi file upload service
- [ ] Implementasi file download service
- [ ] Setup file encryption at rest (LUKS atau application-level)
- [ ] Setup file permissions (600 - owner only)
- [ ] Implementasi file cleanup for deleted threads
- [ ] Test file upload/download with various sizes
- [ ] Setup disk space monitoring

### 2.10 Database Backup & Recovery
- [ ] Buat script backup database (pg_dump)
- [ ] Buat script backup documents (tar)
- [ ] Setup cron job untuk backup otomatis harian
- [ ] Setup backup rotation (keep 30 days)
- [ ] Test restore procedure
- [ ] Document recovery steps
- [ ] Setup backup verification (test restore weekly)
- [ ] Setup backup monitoring dan alerting

### 2.11 Database Performance Optimization
- [ ] Analyze query performance dengan EXPLAIN ANALYZE
- [ ] Add indexes untuk frequently queried columns
- [ ] Setup connection pooling tuning
- [ ] Optimize JSONB queries
- [ ] Setup query logging untuk slow queries
- [ ] Monitor database performance metrics
- [ ] Setup alerts untuk slow queries

### 2.12 Data Seeding
- [ ] Buat seed data untuk testing:
  - [ ] Sample users
  - [ ] Sample threads dengan berbagai status
  - [ ] Sample messages
  - [ ] Sample facts
  - [ ] Sample legal rules (UU, PP, Perpres)
- [ ] Buat script seeding: `pnpm db:seed`
- [ ] Test seeding process
- [ ] Document cara reset database

## Output Fase 2
- ✅ Database schema lengkap dengan semua tabel
- ✅ Migration system berfungsi (upgrade/downgrade)
- ✅ Model ORM terimplementasi untuk semua entitas
- ✅ Repository pattern dengan thread isolation
- ✅ Vector database siap untuk RAG
- ✅ Redis cache layer berfungsi
- ✅ File storage terenkripsi dan terisolasi
- ✅ Backup & recovery procedure teruji
- ✅ Database performance optimal

## Definition of Done
Semua query database terisolasi per thread, data dapat di-backup dan di-restore, dan performance under load acceptable (<100ms untuk query umum).

---

# FASE 3: BACKEND API CORE

## Tujuan
Membangun API backend yang robust, secure, dan ready untuk menerima request dari frontend.

## Aturan Emas

**3.1 — API First Design**
> Semua endpoint harus didefinisikan dalam OpenAPI specification sebelum implementasi. Frontend dan backend harus sepakat pada contract.

**3.2 — Security by Default**
> Semua endpoint harus terautentikasi kecuali yang eksplisit public. Input harus divalidasi. Output harus disanitasi.

**3.3 — Consistent Error Handling**
> Semua error harus mengikuti format yang konsisten. Tidak ada error message yang mengekspos detail sistem.

**3.4 — Rate Limiting is Mandatory**
> Semua endpoint harus memiliki rate limiting untuk mencegah abuse. Limit harus disesuaikan per endpoint.

**3.5 — Idempotency**
> Operasi yang sama dengan parameter yang sama harus menghasilkan hasil yang sama. POST requests harus idempotent jika memungkinkan.

## Ceklist Implementasi

### 3.1 FastAPI Setup
- [ ] Install FastAPI dan dependencies:
  ```bash
  pip install fastapi uvicorn sqlalchemy asyncpg redis python-jose passlib bcrypt python-multipart pydantic[email]
  ```
- [ ] Buat struktur folder `apps/api/app/`
- [ ] Setup main application di `apps/api/app/main.py`
- [ ] Konfigurasi CORS untuk Vercel frontend
- [ ] Setup middleware:
  - [ ] CORS middleware
  - [ ] Request ID middleware
  - [ ] Logging middleware
  - [ ] Error handling middleware
- [ ] Setup lifespan events (startup/shutdown)
- [ ] Test FastAPI server berjalan

### 3.2 Configuration Management
- [ ] Install Pydantic Settings
- [ ] Buat `apps/api/app/core/config.py` dengan semua settings:
  - [ ] Database URL
  - [ ] Redis URL
  - [ ] JWT secret
  - [ ] API keys (Anthropic, OpenAI, LangSmith)
  - [ ] CORS origins
  - [ ] Rate limits
  - [ ] File storage paths
- [ ] Load settings dari environment variables
- [ ] Setup validation untuk required settings
- [ ] Test configuration loading

### 3.3 Authentication System
- [ ] Install authentication libraries:
  ```bash
  pip install python-jose[cryptography] passlib[bcrypt]
  ```
- [ ] Implementasi JWT token generation
  - [ ] Access token (15 menit expiry)
  - [ ] Refresh token (24 jam expiry)
  - [ ] Token signing dengan HS256
- [ ] Implementasi password hashing dengan bcrypt
- [ ] Implementasi token verification
- [ ] Buat dependency `get_current_user` untuk endpoint authentication
- [ ] Buat endpoint authentication:
  - [ ] POST `/auth/register` - registrasi user
  - [ ] POST `/auth/login` - login (magic link)
  - [ ] POST `/auth/verify` - verify magic link token
  - [ ] POST `/auth/refresh` - refresh access token
  - [ ] POST `/auth/logout` - logout
- [ ] Test authentication flow end-to-end
- [ ] Setup token refresh mechanism

### 3.4 Magic Link Authentication
- [ ] Implementasi magic link token generation
- [ ] Setup email service (SMTP atau third-party)
- [ ] Buat template email untuk magic link
- [ ] Implementasi endpoint untuk send magic link
- [ ] Implementasi endpoint untuk verify magic link
- [ ] Setup token expiry (15 menit)
- [ ] Setup one-time use token
- [ ] Test magic link flow:
  - [ ] Request magic link
  - [ ] Receive email
  - [ ] Click link
  - [ ] Verify token
  - [ ] Receive JWT tokens

### 3.5 API Endpoints - Threads
- [ ] Implementasi endpoint `GET /threads`
  - [ ] Return list of threads untuk current user
  - [ ] Pagination support
  - [ ] Filter by status
  - [ ] Sort by updated_at
- [ ] Implementasi endpoint `POST /threads`
  - [ ] Create new thread
  - [ ] Auto-generate title dari first message (optional)
  - [ ] Return created thread
- [ ] Implementasi endpoint `GET /threads/{thread_id}`
  - [ ] Return thread details dengan messages
  - [ ] Verify thread ownership
  - [ ] Return 404 jika thread tidak ditemukan
- [ ] Implementasi endpoint `DELETE /threads/{thread_id}`
  - [ ] Soft delete thread
  - [ ] Verify thread ownership
  - [ ] Return success message
- [ ] Implementasi endpoint `PUT /threads/{thread_id}`
  - [ ] Update thread title
  - [ ] Verify thread ownership
- [ ] Test semua thread endpoints dengan authentication
- [ ] Test thread isolation (user A tidak bisa akses thread user B)

### 3.6 API Endpoints - Messages
- [ ] Implementasi endpoint `POST /threads/{thread_id}/messages`
  - [ ] Accept user message
  - [ ] Validate message content
  - [ ] Save message to database
  - [ ] Trigger agent processing
  - [ ] Return message ID
- [ ] Implementasi endpoint `GET /threads/{thread_id}/messages`
  - [ ] Return list of messages untuk thread
  - [ ] Pagination support
  - [ ] Verify thread ownership
- [ ] Implementasi endpoint `GET /threads/{thread_id}/messages/stream`
  - [ ] Server-Sent Events (SSE) endpoint
  - [ ] Stream agent responses in real-time
  - [ ] Include phase updates
  - [ ] Include token streaming
- [ ] Test message sending dan receiving
- [ ] Test streaming responses

### 3.7 API Endpoints - Documents
- [ ] Implementasi endpoint `POST /threads/{thread_id}/documents`
  - [ ] Accept file upload (multipart/form-data)
  - [ ] Validate file type (PDF, DOCX, TXT)
  - [ ] Validate file size (max 10MB)
  - [ ] Save file to storage
  - [ ] Extract metadata
  - [ ] Trigger document processing
  - [ ] Return document ID
- [ ] Implementasi endpoint `GET /threads/{thread_id}/documents`
  - [ ] Return list of documents untuk thread
  - [ ] Verify thread ownership
- [ ] Implementasi endpoint `GET /threads/{thread_id}/documents/{document_id}`
  - [ ] Return document metadata
  - [ ] Verify thread ownership
- [ ] Implementasi endpoint `DELETE /threads/{thread_id}/documents/{document_id}`
  - [ ] Delete document dari storage
  - [ ] Delete document dari database
  - [ ] Verify thread ownership
- [ ] Implementasi endpoint `GET /threads/{thread_id}/documents/{document_id}/download`
  - [ ] Return file untuk download
  - [ ] Verify thread ownership
- [ ] Test document upload/download dengan berbagai format
- [ ] Test file size limits

### 3.8 API Endpoints - Analysis
- [ ] Implementasi endpoint `POST /threads/{thread_id}/analysis/confirm`
  - [ ] Accept confirmation action (confirm/revise)
  - [ ] Update thread facts_complete status
  - [ ] Trigger next phase jika confirmed
- [ ] Implementasi endpoint `POST /threads/{thread_id}/analysis/start-reasoning`
  - [ ] Start reasoning phase
  - [ ] Return estimated time
- [ ] Implementasi endpoint `GET /threads/{thread_id}/analysis/report`
  - [ ] Return generated report (jika sudah selesai)
  - [ ] Return 404 jika report belum ada
- [ ] Implementasi endpoint `POST /threads/{thread_id}/analysis/export`
  - [ ] Accept export format (pdf/docx)
  - [ ] Generate export file
  - [ ] Return download URL dengan expiry
- [ ] Test analysis flow end-to-end

### 3.9 API Endpoints - Traceability
- [ ] Implementasi endpoint `GET /threads/{thread_id}/traceability`
  - [ ] Return traceability graph (nodes dan edges)
  - [ ] Format untuk React Flow
  - [ ] Verify thread ownership
- [ ] Test traceability data retrieval

### 3.10 API Endpoints - Users
- [ ] Implementasi endpoint `GET /users/me`
  - [ ] Return current user profile
- [ ] Implementasi endpoint `PUT /users/me`
  - [ ] Update user profile
  - [ ] Validate input
- [ ] Implementasi endpoint `DELETE /users/me`
  - [ ] Schedule account deletion (30 days)
  - [ ] Revoke all sessions
- [ ] Test user profile management

### 3.11 Error Handling
- [ ] Buat custom exception classes:
  - [ ] `AuthenticationError`
  - [ ] `AuthorizationError`
  - [ ] `ValidationError`
  - [ ] `NotFoundError`
  - [ ] `ConflictError`
  - [ ] `RateLimitError`
- [ ] Implementasi global exception handler
- [ ] Format error response:
  ```json
  {
    "error": {
      "code": "ERROR_CODE",
      "message": "Human readable message",
      "details": [...],
      "timestamp": "...",
      "request_id": "..."
    }
  }
  ```
- [ ] Test error responses untuk semua error types
- [ ] Pastikan tidak ada sensitive information di error messages

### 3.12 Rate Limiting
- [ ] Install slowapi untuk rate limiting
- [ ] Setup rate limiter dengan Redis backend
- [ ] Define rate limits per endpoint:
  - [ ] Login: 5/minute/IP
  - [ ] Create thread: 20/hour/user
  - [ ] Send message: 60/minute/thread
  - [ ] Upload document: 10/hour/thread
  - [ ] Export report: 30/hour/user
- [ ] Implementasi rate limit headers:
  - [ ] X-RateLimit-Limit
  - [ ] X-RateLimit-Remaining
  - [ ] X-RateLimit-Reset
- [ ] Test rate limiting under load
- [ ] Setup alerts untuk rate limit violations

### 3.13 Request Validation
- [ ] Install Pydantic untuk request validation
- [ ] Buat Pydantic models untuk semua request bodies:
  - [ ] `RegisterRequest`
  - [ ] `LoginRequest`
  - [ ] `CreateThreadRequest`
  - [ ] `SendMessageRequest`
  - [ ] `UploadDocumentRequest`
  - [ ] dll
- [ ] Setup automatic validation di endpoint
- [ ] Test validation errors
- [ ] Setup custom validation rules

### 3.14 Response Serialization
- [ ] Buat Pydantic models untuk semua responses:
  - [ ] `UserResponse`
  - [ ] `ThreadResponse`
  - [ ] `MessageResponse`
  - [ ] `DocumentResponse`
  - [ ] `AnalysisResponse`
  - [ ] dll
- [ ] Setup automatic serialization di endpoint
- [ ] Test response formats
- [ ] Setup response caching untuk read-heavy endpoints

### 3.15 API Documentation
- [ ] Setup OpenAPI/Swagger UI
- [ ] Generate OpenAPI specification otomatis dari FastAPI
- [ ] Add descriptions dan examples untuk semua endpoints
- [ ] Setup authentication di Swagger UI
- [ ] Test all endpoints dari Swagger UI
- [ ] Export OpenAPI spec ke `docs/api/openapi.yaml`
- [ ] Setup Postman collection untuk testing

### 3.16 Health Check & Monitoring
- [ ] Implementasi endpoint `GET /health`
  - [ ] Check database connection
  - [ ] Check Redis connection
  - [ ] Return status dan version
- [ ] Implementasi endpoint `GET /metrics` (untuk Prometheus)
  - [ ] Request count
  - [ ] Request duration
  - [ ] Error count
  - [ ] Active connections
- [ ] Test health check endpoints
- [ ] Setup monitoring integration

### 3.17 Logging
- [ ] Setup structured logging dengan JSON format
- [ ] Log semua requests:
  - [ ] Request ID
  - [ ] User ID
  - [ ] Thread ID
  - [ ] Endpoint
  - [ ] Method
  - [ ] Status code
  - [ ] Duration
- [ ] Log all errors dengan stack trace
- [ ] Setup log rotation
- [ ] Setup log levels (DEBUG, INFO, WARNING, ERROR)
- [ ] Test logging output

### 3.18 Testing
- [ ] Install pytest dan test dependencies
- [ ] Setup test database (separate dari development)
- [ ] Buat unit tests untuk:
  - [ ] Authentication logic
  - [ ] Repository methods
  - [ ] Validation logic
- [ ] Buat integration tests untuk:
  - [ ] All API endpoints
  - [ ] Authentication flow
  - [ ] Thread isolation
  - [ ] Rate limiting
- [ ] Setup test fixtures
- [ ] Test coverage >80%
- [ ] Run tests di CI pipeline

### 3.19 API Security Hardening
- [ ] Setup HTTPS only (redirect HTTP to HTTPS)
- [ ] Setup security headers:
  - [ ] X-Frame-Options: SAMEORIGIN
  - [ ] X-Content-Type-Options: nosniff
  - [ ] X-XSS-Protection: 1; mode=block
  - [ ] Strict-Transport-Security
- [ ] Setup CORS dengan specific origins (not wildcard)
- [ ] Disable unnecessary HTTP methods
- [ ] Setup request size limits
- [ ] Test security headers
- [ ] Run security scan (OWASP ZAP atau similar)

### 3.20 API Performance Optimization
- [ ] Setup response compression (gzip)
- [ ] Setup caching untuk read-heavy endpoints
- [ ] Optimize database queries (avoid N+1)
- [ ] Setup connection pooling
- [ ] Test API performance under load
- [ ] Setup performance monitoring

## Output Fase 3
- ✅ FastAPI backend berjalan dengan semua endpoint
- ✅ Authentication system berfungsi (magic link + JWT)
- ✅ Thread isolation enforced di semua endpoint
- ✅ Rate limiting aktif
- ✅ Error handling konsisten
- ✅ API documentation lengkap (OpenAPI/Swagger)
- ✅ Test coverage >80%
- ✅ Security hardening selesai

## Definition of Done
Semua API endpoints dapat diakses dari frontend, authentication berfungsi, thread isolation terjamin, dan API dapat handle 100 concurrent users tanpa error.

---

# FASE 4: AGENT ORCHESTRATOR (LANGGRAPH)

## Tujuan
Membangun state machine agen yang mengimplementasikan siklus 11 fase sesuai PRD Contract Baseline.

## Aturan Emas

**4.1 — State Machine is Law**
> Agen HARUS mengikuti siklus 11 fase secara berurutan. Tidak boleh ada fase yang dilewati. Conditional edges harus dievaluasi dengan benar.

**4.2 — State Persistence**
> State agen harus dapat di-persist ke database dan di-restore kapan saja. Jika server restart, agen harus bisa melanjutkan dari fase terakhir.

**4.3 — Deterministic Transitions**
> Transisi antar fase harus deterministik berdasarkan state. Tidak boleh ada transisi acak atau tidak terduga.

**4.4 — Error Recovery**
> Jika satu fase gagal, agen harus bisa recover dan melanjutkan dari fase tersebut, bukan dari awal.

**4.5 — Observability**
> Setiap transisi fase harus di-log dan di-trace. Harus bisa menjawab "di fase mana agen sekarang?" kapan saja.

## Ceklist Implementasi

### 4.1 LangGraph Setup
- [ ] Install LangGraph dan dependencies:
  ```bash
  pip install langgraph langchain langchain-anthropic langchain-openai
  ```
- [ ] Buat struktur folder `apps/api/app/agents/`
- [ ] Setup LangGraph state definition
- [ ] Test LangGraph basic workflow

### 4.2 State Schema Definition
- [ ] Definisikan `AgentState` dengan Pydantic:
  ```python
  class AgentState(BaseModel):
      thread_id: str
      user_id: str
      messages: List[Message]
      facts: List[Fact]
      documents: List[Document]
      user_goals: List[str]
      identified_issues: List[str]
      retrieved_laws: List[LegalRule]
      arguments: List[Argument]
      counter_arguments: List[Argument]
      traceability_map: Dict[str, TraceNode]
      current_phase: AgentPhase
      facts_complete: bool
      report_generated: bool
  ```
- [ ] Definisikan semua nested models:
  - [ ] `Message`
  - [ ] `Fact`
  - [ ] `Document`
  - [ ] `LegalRule`
  - [ ] `Argument`
  - [ ] `TraceNode`
- [ ] Setup state validation
- [ ] Test state serialization/deserialization

### 4.3 State Persistence
- [ ] Implementasi state save to database
  - [ ] Save state setelah setiap fase
  - [ ] Include all state fields
  - [ ] Use JSONB untuk complex structures
- [ ] Implementasi state load from database
  - [ ] Load state by thread_id
  - [ ] Handle missing state (new thread)
- [ ] Implementasi state update
  - [ ] Update specific fields
  - [ ] Maintain consistency
- [ ] Test state persistence:
  - [ ] Save state
  - [ ] Restart server
  - [ ] Load state
  - [ ] Verify state完整性

### 4.4 Graph Definition
- [ ] Buat LangGraph StateGraph:
  ```python
  workflow = StateGraph(AgentState)
  ```
- [ ] Add semua nodes:
  - [ ] `PAHAM`
  - [ ] `TANYA`
  - [ ] `KONFIRMASI`
  - [ ] `RUMUSKAN`
  - [ ] `TELITI`
  - [ ] `VERIFIKASI`
  - [ ] `NALAR`
  - [ ] `BANTAH`
  - [ ] `UJI`
  - [ ] `SIMPULKAN`
  - [ ] `TELUSURI`
- [ ] Add edges:
  - [ ] `PAHAM` → `TANYA`
  - [ ] `TANYA` → `KONFIRMASI`
  - [ ] `KONFIRMASI` → conditional → `TANYA` atau `RUMUSKAN`
  - [ ] `RUMUSKAN` → `TELITI`
  - [ ] `TELITI` → `VERIFIKASI`
  - [ ] `VERIFIKASI` → `NALAR`
  - [ ] `NALAR` → `BANTAH`
  - [ ] `BANTAH` → `UJI`
  - [ ] `UJI` → `SIMPULKAN`
  - [ ] `SIMPULKAN` → `TELUSURI`
  - [ ] `TELUSURI` → conditional → `TELITI` atau `END`
- [ ] Setup entry point: `PAHAM`
- [ ] Setup exit point: `END`
- [ ] Compile graph
- [ ] Test graph structure

### 4.5 Conditional Edges Implementation
- [ ] Implementasi conditional edge dari `KONFIRMASI`:
  ```python
  def should_continue(state: AgentState) -> str:
      if state.facts_complete:
          return "RUMUSKAN"
      return "TANYA"
  ```
- [ ] Implementasi conditional edge dari `TELUSURI`:
  ```python
  def should_retry_research(state: AgentState) -> str:
      if all_citations_valid(state.traceability_map):
          return "END"
      return "TELITI"
  ```
- [ ] Test conditional transitions:
  - [ ] Facts incomplete → kembali ke TANYA
  - [ ] Facts complete → lanjut ke RUMUSKAN
  - [ ] Invalid citations → kembali ke TELITI
  - [ ] Valid citations → END

### 4.6 Node Implementation - PAHAM
- [ ] Definisikan fungsi `node_paham(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Extract informasi dari first message
  - [ ] Identify parties involved
  - [ ] Identify object of dispute
  - [ ] Identify initial chronology
  - [ ] Identify unknown information
- [ ] Call LLM dengan prompt:
  ```
  Anda adalah PAUGERAN. Pahami masalah awal pengguna:
  {user_input}
  
  Identifikasi:
  1. Pihak yang terlibat
  2. Objek masalah
  3. Kronologi awal
  4. Informasi yang belum diketahui
  
  Output dalam JSON terstruktur.
  ```
- [ ] Parse LLM response
- [ ] Update state dengan initial facts
- [ ] Save state to database
- [ ] Send message to user dengan summary
- [ ] Test node PAHAM dengan various inputs

### 4.7 Node Implementation - TANYA
- [ ] Definisikan fungsi `node_tanya(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Analyze current facts
  - [ ] Identify missing information
  - [ ] Generate adaptive questions (1-3 questions)
  - [ ] Prioritize questions by impact
- [ ] Call LLM dengan prompt untuk generate questions
- [ ] Parse LLM response
- [ ] Update state dengan questions
- [ ] Send questions to user
- [ ] Test node TANYA:
  - [ ] Questions are relevant
  - [ ] Questions are adaptive (not static)
  - [ ] Questions prioritize material facts

### 4.8 Node Implementation - KONFIRMASI
- [ ] Definisikan fungsi `node_konfirmasi(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Compile all facts
  - [ ] Generate reconstruction summary
  - [ ] Format output sesuai PRD (Tujuan, Para Pihak, Kronologi, dll)
  - [ ] Ask for user confirmation
- [ ] Call LLM untuk generate summary
- [ ] Parse LLM response
- [ ] Send reconstruction to user
- [ ] Wait for user confirmation (confirm/revise)
- [ ] Update `facts_complete` based on user response
- [ ] Test node KONFIRMASI:
  - [ ] Summary is comprehensive
  - [ ] User can confirm or revise
  - [ ] Conditional edge works correctly

### 4.9 Node Implementation - RUMUSKAN
- [ ] Definisikan fungsi `node_rumuskan(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Identify legal issues
  - [ ] Classify problem (perdata, pidana, TUN, dll)
  - [ ] Identify jurisdiction
  - [ ] Identify relevant legal fields
- [ ] Call LLM dengan model berat (Claude Sonnet/Opus)
- [ ] Parse LLM response
- [ ] Update state dengan identified issues
- [ ] Send summary to user
- [ ] Test node RUMUSKAN dengan various cases

### 4.10 Node Implementation - TELITI
- [ ] Definisikan fungsi `node_teliti(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Search for relevant regulations (RAG)
  - [ ] Search for relevant court decisions
  - [ ] Search for legal doctrines
  - [ ] Use vector search untuk semantic matching
  - [ ] Use Playwright untuk external research (jika perlu)
- [ ] Call RAG service
- [ ] Parse search results
- [ ] Update state dengan retrieved laws
- [ ] Send research summary to user
- [ ] Test node TELITI:
  - [ ] Relevant sources ditemukan
  - [ ] Sources are from trusted databases
  - [ ] Playwright only accesses whitelisted domains

### 4.11 Node Implementation - VERIFIKASI
- [ ] Definisikan fungsi `node_verifikasi(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Verify each legal source
  - [ ] Check effective date
  - [ ] Check revocation status
  - [ ] Check amendments
  - [ ] Verify relevance to case timeline
- [ ] Call LLM untuk verification
- [ ] Parse verification results
- [ ] Update state dengan verified laws
- [ ] Mark invalid sources
- [ ] Test node VERIFIKASI:
  - [ ] Invalid sources are marked
  - [ ] Effective dates are checked
  - [ ] Revoked laws are excluded

### 4.12 Node Implementation - NALAR
- [ ] Definisikan fungsi `node_nalar(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Apply law to facts
  - [ ] Analyze legal elements
  - [ ] Build supporting arguments
  - [ ] Structure argumentation
- [ ] Call LLM dengan model berat
- [ ] Parse LLM response
- [ ] Update state dengan arguments
- [ ] Send analysis to user
- [ ] Test node NALAR:
  - [ ] Arguments are logical
  - [ ] Arguments are supported by facts
  - [ ] Legal elements are analyzed

### 4.13 Node Implementation - BANTAH
- [ ] Definisikan fungsi `node_bantah(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Analyze supporting arguments
  - [ ] Identify weaknesses
  - [ ] Search for counter-arguments
  - [ ] Search for opposing court decisions
  - [ ] Identify exceptions
- [ ] Call LLM dengan model berbeda (untuk avoid bias)
- [ ] Parse counter-arguments
- [ ] Update state dengan counter_arguments
- [ ] Send counter-arguments to user
- [ ] Test node BANTAH:
  - [ ] Counter-arguments are substantial
  - [ ] Not just weak objections
  - [ ] Different perspective dari NALAR

### 4.14 Node Implementation - UJI
- [ ] Definisikan fungsi `node_uji(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Evaluate supporting arguments
  - [ ] Evaluate counter-arguments
  - [ ] Assess strength of each side
  - [ ] Identify uncertainties
  - [ ] Determine certainty level
- [ ] Call LLM untuk evaluation
- [ ] Parse evaluation results
- [ ] Update state dengan assessment
- [ ] Send balanced assessment to user
- [ ] Test node UJI:
  - [ ] Both sides are evaluated
  - [ ] Uncertainties are identified
  - [ ] Certainty level is assigned

### 4.15 Node Implementation - SIMPULKAN
- [ ] Definisikan fungsi `node_simpulkan(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Synthesize all analysis
  - [ ] Formulate conclusion
  - [ ] Assign certainty category:
    - Sangat kuat
    - Kuat
    - Cukup kuat
    - Bersyarat
    - Belum dapat ditentukan
    - Lemah
    - Bertentangan
  - [ ] Explain reasoning
- [ ] Call LLM untuk conclusion
- [ ] Parse conclusion
- [ ] Update state dengan conclusion
- [ ] Send conclusion to user
- [ ] Test node SIMPULKAN:
  - [ ] Conclusion is clear
  - [ ] Certainty level is appropriate
  - [ ] Reasoning is explained

### 4.16 Node Implementation - TELUSURI
- [ ] Definisikan fungsi `node_telusuri(state: AgentState) -> AgentState`
- [ ] Implementasi logic:
  - [ ] Build traceability map
  - [ ] Link conclusions to reasons
  - [ ] Link reasons to legal rules
  - [ ] Link legal rules to sources
  - [ ] Link rules to facts
  - [ ] Link facts to evidence
  - [ ] Validate all citations
- [ ] Call LLM untuk traceability mapping
- [ ] Parse traceability map
- [ ] Validate citations:
  - [ ] Every conclusion has reasons
  - [ ] Every reason has legal rules
  - [ ] Every rule has valid source
  - [ ] Every rule is linked to facts
- [ ] Update state dengan traceability_map
- [ ] If validation fails → return to TELITI
- [ ] If validation passes → END
- [ ] Test node TELUSURI:
  - [ ] Traceability map is complete
  - [ ] All citations are valid
  - [ ] Conditional edge works correctly

### 4.17 Agent Execution
- [ ] Implementasi agent runner:
  ```python
  class PaugeranAgent:
      def __init__(self, thread_id: str, db: Session):
          self.thread_id = thread_id
          self.db = db
          self.graph = self._build_graph()
      
      async def run(self):
          state = await self._load_state()
          result = self.graph.invoke(state)
          await self._save_state(result)
          return result
  ```
- [ ] Implementasi state loading dari database
- [ ] Implementasi state saving ke database
- [ ] Implementasi event streaming ke frontend
- [ ] Test agent execution end-to-end

### 4.18 Error Handling & Recovery
- [ ] Implementasi error handling untuk setiap node
- [ ] Setup retry logic untuk transient errors
- [ ] Implementasi state rollback on error
- [ ] Setup error logging dengan context
- [ ] Test error scenarios:
  - [ ] LLM API failure
  - [ ] Database connection loss
  - [ ] Invalid LLM response
  - [ ] Timeout

### 4.19 Observability
- [ ] Setup LangSmith tracing:
  ```python
  os.environ["LANGCHAIN_TRACING_V2"] = "true"
  os.environ["LANGCHAIN_API_KEY"] = settings.LANGSMITH_API_KEY
  ```
- [ ] Trace every node execution
- [ ] Log input/output untuk setiap node
- [ ] Track token usage
- [ ] Measure latency per node
- [ ] Setup LangSmith dashboard
- [ ] Test tracing di LangSmith UI

### 4.20 Testing
- [ ] Buat unit tests untuk setiap node
- [ ] Buat integration tests untuk full workflow
- [ ] Test conditional edges
- [ ] Test state persistence
- [ ] Test error recovery
- [ ] Test with various case types:
  - [ ] Sengketa tanah
  - [ ] Wanprestasi kontrak
  - [ ] PHK
  - [ ] Perceraian
  - [ ] Warisan
- [ ] Test coverage >80%

## Output Fase 4
- ✅ LangGraph state machine terimplementasi dengan 11 fase
- ✅ Conditional edges berfungsi dengan benar
- ✅ State persistence ke database
- ✅ Error handling dan recovery
- ✅ Observability dengan LangSmith
- ✅ Test coverage >80%

## Definition of Done
Agen dapat menjalankan siklus lengkap dari PAHAM sampai TELUSURI, state dapat di-persist dan di-restore, dan semua fase menghasilkan output yang sesuai PRD.

---

# FASE 5: LLM INTEGRATION & RAG

## Tujuan
Mengintegrasikan model AI (Anthropic/OpenAI) dan membangun sistem RAG untuk penelitian hukum.

## Aturan Emas

**5.1 — No Hallucination Policy**
> Model AI TIDAK BOLEH mengarang pasal, putusan, atau sumber hukum. Semua klaim hukum HARUS didukung oleh sumber dari database.

**5.2 — Citation Enforcement**
> Setiap klaim hukum HARUS memiliki `citation_id` yang valid. Klaim tanpa citation harus ditolak.

**5.3 — Model Routing**
> Gunakan model yang tepat untuk tugas yang tepat. Jangan gunakan model berat untuk tugas ringan, dan jangan gunakan model ringan untuk penalaran kompleks.

**5.4 — Cost Control**
> Monitor token usage dan biaya API. Setup budget limits per analysis.

**5.5 — Fallback Strategy**
> Jika satu model API gagal, fallback ke model alternatif. Jangan biarkan user menunggu tanpa respons.

## Ceklist Implementasi

### 5.1 LiteLLM Setup
- [ ] Install LiteLLM:
  ```bash
  pip install litellm
  ```
- [ ] Buat LLM client wrapper di `apps/api/app/services/llm_client.py`
- [ ] Setup model routing:
  ```python
  class LLMClient:
      async def call(self, task: str, messages: List[Dict]) -> str:
          if task == "interactive":
              return await self._call_claude_haiku(messages)
          elif task == "reasoning":
              return await self._call_claude_sonnet(messages)
          elif task == "extraction":
              return await self._call_gpt4o(messages)
  ```
- [ ] Setup API keys dari environment
- [ ] Test LiteLLM routing

### 5.2 Model Configuration
- [ ] Konfigurasi Anthropic Claude models:
  - [ ] Claude 3.5 Haiku (interactive tasks)
  - [ ] Claude 3.5 Sonnet (reasoning tasks)
  - [ ] Claude 3.5 Opus (complex reasoning - optional)
- [ ] Konfigurasi OpenAI models:
  - [ ] GPT-4o (extraction tasks)
  - [ ] GPT-4o-mini (fallback - optional)
- [ ] Setup model parameters:
  - [ ] Temperature (0.1 untuk extraction, 0.3 untuk reasoning)
  - [ ] Max tokens
  - [ ] Top-p
- [ ] Test semua models

### 5.3 Structured Output
- [ ] Setup JSON mode untuk extraction tasks
- [ ] Buat Pydantic models untuk structured output:
  - [ ] `PahamResponse`
  - [ ] `TanyaResponse`
  - [ ] `RumuskanResponse`
  - [ ] `TelitiResponse`
  - [ ] `NalarResponse`
  - [ ] `BantahResponse`
  - [ ] `SimpulkanResponse`
  - [ ] `TelusuriResponse`
- [ ] Implementasi output validation:
  ```python
  def validate_output(response: str, model: Type[BaseModel]) -> BaseModel:
      try:
          parsed = json.loads(response)
          return model(**parsed)
      except (json.JSONDecodeError, ValidationError) as e:
          raise LLMOutputError(f"Invalid output: {e}")
  ```
- [ ] Test structured output untuk semua nodes

### 5.4 Prompt Engineering
- [ ] Buat prompt templates untuk setiap node:
  - [ ] `prompts/paham.txt`
  - [ ] `prompts/tanya.txt`
  - [ ] `prompts/konfirmasi.txt`
  - [ ] `prompts/rumuskan.txt`
  - [ ] `prompts/teliti.txt`
  - [ ] `prompts/verifikasi.txt`
  - [ ] `prompts/nalar.txt`
  - [ ] `prompts/bantah.txt`
  - [ ] `prompts/uji.txt`
  - [ ] `prompts/simpulkan.txt`
  - [ ] `prompts/telusuri.txt`
- [ ] Include context injection:
  - [ ] Inject facts ke prompt
  - [ ] Inject legal rules ke prompt
  - [ ] Inject user goals ke prompt
- [ ] Include output format instructions
- [ ] Include examples (few-shot learning)
- [ ] Test prompts dengan various inputs

### 5.5 RAG Pipeline Setup
- [ ] Install RAG dependencies:
  ```bash
  pip install llama-index sentence-transformers
  ```
- [ ] Buat RAG service di `apps/api/app/services/rag_service.py`
- [ ] Setup embedding model:
  - [ ] Gunakan OpenAI embeddings (text-embedding-3-small)
  - [ ] Atau gunakan local model (BGE-M3) untuk cost saving
- [ ] Setup vector store (pgvector)
- [ ] Test embedding generation

### 5.6 Legal Database Ingestion
- [ ] Kumpulkan database hukum Indonesia:
  - [ ] Undang-Undang (UU)
  - [ ] Peraturan Pemerintah (PP)
  - [ ] Peraturan Presiden (Perpres)
  - [ ] Peraturan Menteri (Permen)
  - [ ] Putusan Mahkamah Agung (selected)
  - [ ] Doktrin hukum (selected)
- [ ] Parse dokumen hukum:
  - [ ] Extract pasal-pasal
  - [ ] Extract metadata (nomor, tahun, tanggal)
  - [ ] Extract struktur (bab, bagian, pasal)
- [ ] Generate embeddings untuk setiap pasal
- [ ] Store embeddings di pgvector
- [ ] Setup metadata indexing
- [ ] Test ingestion pipeline

### 5.7 Retrieval Implementation
- [ ] Implementasi semantic search:
  ```python
  async def search_legal_rules(query: str, top_k: int = 10) -> List[LegalRule]:
      # Generate query embedding
      query_embedding = await generate_embedding(query)
      
      # Search vector database
      results = await vector_db.search(
          embedding=query_embedding,
          top_k=top_k,
          filter={"status": "active"}
      )
      
      return results
  ```
- [ ] Implementasi hybrid search (semantic + keyword)
- [ ] Setup relevance scoring
- [ ] Setup filtering by:
  - [ ] Legal type (UU, PP, Perpres, dll)
  - [ ] Effective date
  - [ ] Jurisdiction
- [ ] Test retrieval accuracy

### 5.8 Context Assembly
- [ ] Implementasi context assembly untuk LLM:
  ```python
  def assemble_context(legal_rules: List[LegalRule], facts: List[Fact]) -> str:
      context = "FAKTA:\n"
      for fact in facts:
          context += f"- {fact.content}\n"
      
      context += "\nSUMBER HUKUM:\n"
      for rule in legal_rules:
          context += f"- {rule.source} {rule.article}: {rule.content}\n"
      
      return context
  ```
- [ ] Setup context size limits (max 100k tokens)
- [ ] Setup context prioritization (most relevant first)
- [ ] Test context assembly

### 5.9 Citation Enforcement
- [ ] Implementasi citation extraction dari LLM output:
  ```python
  def extract_citations(text: str) -> List[str]:
      # Extract citation IDs dari text
      # Format: [citation:id]
      citations = re.findall(r'\[citation:([^\]]+)\]', text)
      return citations
  ```
- [ ] Implementasi citation validation:
  ```python
  def validate_citations(citations: List[str], legal_rules: List[LegalRule]) -> bool:
      valid_ids = {rule.id for rule in legal_rules}
      return all(cid in valid_ids for cid in citations)
  ```
- [ ] Reject output dengan invalid citations
- [ ] Force LLM to retry dengan valid citations
- [ ] Test citation enforcement

### 5.10 Browser Automation (Playwright)
- [ ] Install Playwright:
  ```bash
  pip install playwright
  playwright install chromium
  ```
- [ ] Buat browser service di `apps/api/app/services/browser_service.py`
- [ ] Setup whitelist domains:
  ```python
  WHITELISTED_DOMAINS = [
      "jdih.setkab.go.id",
      "mahkamahagung.go.id",
      "bpk.go.id",
      "ojk.go.id",
      "bphn.go.id",
  ]
  ```
- [ ] Implementasi domain validation:
  ```python
  def is_allowed_domain(url: str) -> bool:
      return any(domain in url for domain in WHITELISTED_DOMAINS)
  ```
- [ ] Implementasi web scraping:
  - [ ] Navigate to URL
  - [ ] Wait for content
  - [ ] Extract text
  - [ ] Close browser
- [ ] Setup timeout (30 seconds)
- [ ] Setup read-only mode (no form filling)
- [ ] Test browser automation

### 5.11 Document Parsing
- [ ] Install document parsing libraries:
  ```bash
  pip install PyPDF2 python-docx pdfplumber
  ```
- [ ] Buat document parser service
- [ ] Implementasi PDF parsing:
  - [ ] Extract text
  - [ ] Extract tables
  - [ ] Extract metadata
- [ ] Implementasi DOCX parsing:
  - [ ] Extract text
  - [ ] Extract structure
- [ ] Implementasi TXT parsing
- [ ] Setup OCR untuk scanned documents (optional)
- [ ] Test document parsing dengan various formats

### 5.12 PII Anonymization
- [ ] Install NER library:
  ```bash
  pip install spacy
  python -m spacy download id_core_news_sm
  ```
- [ ] Buat anonymizer service di `apps/api/app/utils/anonymizer.py`
- [ ] Implementasi PII detection:
  - [ ] Names
  - [ ] NIK (Nomor Induk Kependudukan)
  - [ ] Phone numbers
  - [ ] Email addresses
  - [ ] Bank account numbers
  - [ ] Addresses
- [ ] Implementasi PII redaction:
  ```python
  def anonymize_text(text: str) -> Tuple[str, Dict[str, str]]:
      # Detect PII
      # Replace with placeholders
      # Return anonymized text + mapping
      pass
  ```
- [ ] Implementasi de-anonymization:
  ```python
  def deanonymize_text(text: str, mapping: Dict[str, str]) -> str:
      # Replace placeholders with original PII
      pass
  ```
- [ ] Apply anonymization sebelum kirim ke LLM API
- [ ] Apply de-anonymization setelah terima respons dari LLM
- [ ] Test anonymization accuracy

### 5.13 Cost Monitoring
- [ ] Setup token usage tracking:
  ```python
  def track_token_usage(model: str, input_tokens: int, output_tokens: int):
      # Log token usage
      # Calculate cost
      # Update metrics
      pass
  ```
- [ ] Setup cost calculation:
  - [ ] Claude Haiku: $0.00025/1K input tokens, $0.00125/1K output tokens
  - [ ] Claude Sonnet: $0.003/1K input tokens, $0.015/1K output tokens
  - [ ] GPT-4o: $0.005/1K input tokens, $0.015/1K output tokens
- [ ] Setup budget limits per analysis (e.g., max $2)
- [ ] Setup alerts untuk high cost
- [ ] Test cost tracking

### 5.14 Fallback Strategy
- [ ] Implementasi fallback logic:
  ```python
  async def call_llm_with_fallback(task: str, messages: List[Dict]) -> str:
      try:
          return await call_primary_model(task, messages)
      except Exception as e:
          logger.warning(f"Primary model failed: {e}")
          return await call_fallback_model(task, messages)
  ```
- [ ] Setup fallback models:
  - [ ] Claude Sonnet → Claude Haiku
  - [ ] GPT-4o → GPT-4o-mini
  - [ ] Anthropic → OpenAI (cross-provider fallback)
- [ ] Test fallback scenarios

### 5.15 Prompt Optimization
- [ ] Monitor prompt performance:
  - [ ] Success rate
  - [ ] Token usage
  - [ ] Latency
- [ ] A/B test different prompts
- [ ] Optimize prompts untuk:
  - [ ] Accuracy
  - [ ] Cost efficiency
  - [ ] Speed
- [ ] Document prompt versions

### 5.16 Testing
- [ ] Test LLM integration:
  - [ ] All models respond correctly
  - [ ] Structured output is valid
  - [ ] Citations are enforced
- [ ] Test RAG pipeline:
  - [ ] Retrieval accuracy >80%
  - [ ] Context assembly is correct
  - [ ] Legal database is complete
- [ ] Test browser automation:
  - [ ] Whitelist enforcement
  - [ ] Timeout handling
  - [ ] Error handling
- [ ] Test PII anonymization:
  - [ ] All PII detected
  - [ ] De-anonymization works
- [ ] Test cost monitoring:
  - [ ] Token tracking accurate
  - [ ] Cost calculation correct
  - [ ] Budget limits enforced

## Output Fase 5
- ✅ LLM integration dengan Anthropic dan OpenAI
- ✅ RAG pipeline berfungsi dengan legal database
- ✅ Citation enforcement aktif
- ✅ Browser automation untuk external research
- ✅ Document parsing untuk berbagai format
- ✅ PII anonymization aktif
- ✅ Cost monitoring dan budget limits
- ✅ Fallback strategy implemented

## Definition of Done
LLM dapat dipanggil untuk semua node agen, RAG dapat retrieve sumber hukum yang relevan, semua klaim hukum memiliki citation yang valid, dan PII teranonymisasi sebelum dikirim ke API.

---

# FASE 6: FRONTEND DEVELOPMENT

## Tujuan
Membangun antarmuka pengguna yang modern, intuitif, dan chat-first sesuai spesifikasi PRD.

## Aturan Emas

**6.1 — Chat-First Experience**
> Antarmuka utama adalah chat. Semua fitur kompleks (peta keterlacakan, laporan) harus dapat diakses dari dalam chat tanpa meninggalkan konteks.

**6.2 — Real-Time Feedback**
> Pengguna harus melihat progress agen secara real-time. Tidak ada "loading..." tanpa progress indicator.

**6.3 — Thread Isolation UI**
> UI harus clearly menunjukkan thread mana yang sedang aktif. Switching thread harus instant tanpa data leakage.

**6.4 — Responsive Design**
> UI harus berfungsi dengan baik di desktop, tablet, dan mobile. Chat interface harus comfortable di semua ukuran layar.

**6.5 — Accessibility**
> UI harus accessible untuk pengguna dengan disabilitas. Keyboard navigation, screen reader support, dan color contrast harus memenuhi standar WCAG 2.1 AA.

## Ceklist Implementasi

### 6.1 Next.js Setup
- [ ] Create Next.js app di `apps/web/`:
  ```bash
  npx create-next-app@latest web --typescript --tailwind --app --src-dir
  ```
- [ ] Setup App Router structure
- [ ] Konfigurasi TypeScript strict mode
- [ ] Setup Tailwind CSS
- [ ] Install shadcn/ui components
- [ ] Test Next.js dev server

### 6.2 Project Structure
- [ ] Buat folder structure:
  ```
  apps/web/
  ├── app/
  │   ├── (auth)/
  │   │   ├── login/
  │   │   └── register/
  │   ├── (dashboard)/
  │   │   ├── chat/
  │   │   │   └── [threadId]/
  │   │   └── layout.tsx
  │   ├── api/
  │   ├── layout.tsx
  │   └── page.tsx
  ├── components/
  │   ├── chat/
  │   ├── canvas/
  │   ├── traceability/
  │   └── ui/
  ├── lib/
  │   ├── api.ts
  │   ├── auth.ts
  │   └── types.ts
  └── styles/
  ```
- [ ] Setup path aliases di `tsconfig.json`
- [ ] Test folder structure

### 6.3 API Client
- [ ] Buat API client di `lib/api.ts`:
  ```typescript
  class PaugeranAPI {
    private baseUrl: string;
    private token: string | null = null;
    
    constructor(baseUrl: string) {
      this.baseUrl = baseUrl;
    }
    
    setToken(token: string) {
      this.token = token;
    }
    
    private async request<T>(endpoint: string, options: RequestInit = {}): Promise<T> {
      const response = await fetch(`${this.baseUrl}${endpoint}`, {
        ...options,
        headers: {
          'Content-Type': 'application/json',
          ...(this.token && { Authorization: `Bearer ${this.token}` }),
          ...options.headers,
        },
      });
      
      if (!response.ok) {
        throw new Error(`API Error: ${response.statusText}`);
      }
      
      return response.json();
    }
    
    // Thread methods
    async getThreads(): Promise<Thread[]> { }
    async createThread(title?: string): Promise<Thread> { }
    async getThread(threadId: string): Promise<Thread> { }
    async deleteThread(threadId: string): Promise<void> { }
    
    // Message methods
    async sendMessage(threadId: string, content: string): Promise<Message> { }
    async streamMessages(threadId: string): Promise<ReadableStream> { }
    
    // Document methods
    async uploadDocument(threadId: string, file: File): Promise<Document> { }
    async getDocuments(threadId: string): Promise<Document[]> { }
    async deleteDocument(threadId: string, documentId: string): Promise<void> { }
    
    // Analysis methods
    async confirmAnalysis(threadId: string, action: 'confirm' | 'revise'): Promise<void> { }
    async startReasoning(threadId: string): Promise<void> { }
    async getReport(threadId: string): Promise<Report> { }
    async exportReport(threadId: string, format: 'pdf' | 'docx'): Promise<string> { }
    
    // Traceability methods
    async getTraceability(threadId: string): Promise<TraceabilityGraph> { }
  }
  
  export const api = new PaugeranAPI(process.env.NEXT_PUBLIC_API_URL!);
  ```
- [ ] Setup error handling
- [ ] Setup request/response types
- [ ] Test API client

### 6.4 Authentication UI
- [ ] Buat halaman login di `app/(auth)/login/page.tsx`:
  - [ ] Email input
  - [ ] "Send Magic Link" button
  - [ ] Loading state
  - [ ] Success message
- [ ] Buat halaman register di `app/(auth)/register/page.tsx`:
  - [ ] Email input
  - [ ] Name input
  - [ ] Profession input
  - [ ] Submit button
- [ ] Buat halaman verify magic link di `app/(auth)/verify/page.tsx`:
  - [ ] Verify token dari URL
  - [ ] Store JWT tokens
  - [ ] Redirect to dashboard
- [ ] Implementasi auth state management:
  - [ ] Store tokens di localStorage
  - [ ] Auto-refresh tokens
  - [ ] Logout functionality
- [ ] Test authentication flow

### 6.5 Dashboard Layout
- [ ] Buat layout di `app/(dashboard)/layout.tsx`:
  - [ ] Sidebar untuk thread list
  - [ ] Main area untuk chat
  - [ ] Header untuk user info
- [ ] Implementasi responsive layout:
  - [ ] Desktop: sidebar + main area
  - [ ] Mobile: sidebar hidden, toggle button
- [ ] Setup navigation
- [ ] Test layout responsiveness

### 6.6 Thread Sidebar
- [ ] Buat komponen `ThreadSidebar` di `components/chat/ThreadSidebar.tsx`:
  - [ ] "New Thread" button
  - [ ] Thread list grouped by date:
    - [ ] Hari Ini
    - [ ] Minggu Lalu
    - [ ] Bulan Lalu
    - [ ] Older
  - [ ] Thread item dengan:
    - [ ] Title
    - [ ] Last updated time
    - [ ] Hover actions (rename, delete)
- [ ] Implementasi thread switching
- [ ] Implementasi thread creation
- [ ] Implementasi thread deletion
- [ ] Test thread sidebar

### 6.7 Chat Interface
- [ ] Buat komponen `ChatWindow` di `components/chat/ChatWindow.tsx`:
  - [ ] Message list
  - [ ] Auto-scroll to bottom
  - [ ] Message grouping by date
- [ ] Buat komponen `MessageBubble` di `components/chat/MessageBubble.tsx`:
  - [ ] User message (right aligned, blue background)
  - [ ] Assistant message (left aligned, gray background)
  - [ ] Phase indicator di header
  - [ ] Timestamp
  - [ ] Action buttons (contextual)
- [ ] Buat komponen `InputArea` di `components/chat/InputArea.tsx`:
  - [ ] Multi-line textarea
  - [ ] Document upload button
  - [ ] Send button
  - [ ] Keyboard shortcuts (Enter to send, Shift+Enter for new line)
- [ ] Implementasi message sending
- [ ] Implementasi message receiving
- [ ] Test chat interface

### 6.8 Streaming Responses
- [ ] Implementasi SSE (Server-Sent Events) client:
  ```typescript
  async function streamMessages(threadId: string) {
    const response = await fetch(
      `${API_URL}/threads/${threadId}/messages/stream`,
      {
        headers: { Authorization: `Bearer ${token}` }
      }
    );
    
    const reader = response.body!.getReader();
    const decoder = new TextDecoder();
    
    while (true) {
      const { done, value } = await reader.read();
      if (done) break;
      
      const text = decoder.decode(value);
      const lines = text.split('\n');
      
      for (const line of lines) {
        if (line.startsWith('data: ')) {
          const data = JSON.parse(line.slice(6));
          handleStreamEvent(data);
        }
      }
    }
  }
  ```
- [ ] Handle stream events:
  - [ ] Phase updates
  - [ ] Token streaming
  - [ ] Completion
  - [ ] Errors
- [ ] Display streaming text dengan typing effect
- [ ] Test streaming responses

### 6.9 Document Upload
- [ ] Buat komponen `DocumentUpload` di `components/chat/DocumentUpload.tsx`:
  - [ ] File picker dialog
  - [ ] File type validation (PDF, DOCX, TXT)
  - [ ] File size validation (max 10MB)
  - [ ] Upload progress indicator
  - [ ] Success/error messages
- [ ] Implementasi file upload:
  ```typescript
  async function uploadDocument(threadId: string, file: File) {
    const formData = new FormData();
    formData.append('file', file);
    
    const response = await fetch(
      `${API_URL}/threads/${threadId}/documents`,
      {
        method: 'POST',
        headers: { Authorization: `Bearer ${token}` },
        body: formData,
      }
    );
    
    return response.json();
  }
  ```
- [ ] Display uploaded documents di chat
- [ ] Test document upload

### 6.10 Phase Indicators
- [ ] Buat komponen `PhaseIndicator` untuk menampilkan fase agen:
  - [ ] PAHAM: "Memahami masalah..."
  - [ ] TANYA: "Mengajukan pertanyaan..."
  - [ ] KONFIRMASI: "Menunggu konfirmasi..."
  - [ ] RUMUSKAN: "Merumuskan isu hukum..."
  - [ ] TELITI: "Meneliti sumber hukum..."
  - [ ] VERIFIKASI: "Memverifikasi sumber..."
  - [ ] NALAR: "Menganalisis hukum..."
  - [ ] BANTAH: "Mencari kontraargumentasi..."
  - [ ] UJI: "Menguji argumen..."
  - [ ] SIMPULKAN: "Menyusun kesimpulan..."
  - [ ] TELUSURI: "Memvalidasi keterlacakan..."
- [ ] Display phase indicator di message header
- [ ] Animate phase transitions
- [ ] Test phase indicators

### 6.11 Confirmation UI
- [ ] Buat komponen `ConfirmationCard` untuk fase KONFIRMASI:
  - [ ] Display reconstruction summary
  - [ ] "Setuju" button
  - [ ] "Revisi" button
  - [ ] "Mulai Penalaran" button (jika sudah lengkap)
- [ ] Implementasi confirmation actions:
  ```typescript
  async function confirmAnalysis(threadId: string, action: 'confirm' | 'revise') {
    await api.confirmAnalysis(threadId, action);
  }
  ```
- [ ] Test confirmation flow

### 6.12 Traceability Panel
- [ ] Install React Flow:
  ```bash
  npm install @xyflow/react
  ```
- [ ] Buat komponen `TraceGraph` di `components/traceability/TraceGraph.tsx`:
  - [ ] Node types:
    - [ ] Conclusion node (red)
    - [ ] Reason node (orange)
    - [ ] Legal rule node (blue)
    - [ ] Fact node (green)
    - [ ] Source node (gray)
  - [ ] Edge types:
    - [ ] Solid lines untuk strong connections
    - [ ] Dashed lines untuk weak connections
  - [ ] Interactive features:
    - [ ] Zoom in/out
    - [ ] Pan
    - [ ] Click node for details
    - [ ] Hover to highlight path
- [ ] Implementasi traceability data fetching:
  ```typescript
  async function loadTraceability(threadId: string) {
    const data = await api.getTraceability(threadId);
    // Convert to React Flow format
    return convertToFlowFormat(data);
  }
  ```
- [ ] Setup panel drawer (slide from right)
- [ ] Test traceability panel

### 6.13 Report Panel
- [ ] Buat komponen `ReportPanel` di `components/canvas/ReportPanel.tsx`:
  - [ ] Display 24-point report
  - [ ] Collapsible sections
  - [ ] Rich text formatting
  - [ ] Citation links
- [ ] Implementasi report fetching:
  ```typescript
  async function loadReport(threadId: string) {
    const report = await api.getReport(threadId);
    return report;
  }
  ```
- [ ] Setup panel drawer
- [ ] Test report panel

### 6.14 Export Functionality
- [ ] Buat komponen `ExportButton` dengan dropdown:
  - [ ] Export to PDF
  - [ ] Export to DOCX
  - [ ] Export traceability as PNG
  - [ ] Export traceability as SVG
- [ ] Implementasi export:
  ```typescript
  async function exportReport(threadId: string, format: 'pdf' | 'docx') {
    const url = await api.exportReport(threadId, format);
    // Trigger download
    window.open(url, '_blank');
  }
  ```
- [ ] Test export functionality

### 6.15 State Management
- [ ] Install Zustand:
  ```bash
  npm install zustand
  ```
- [ ] Buat store di `lib/store.ts`:
  ```typescript
  interface PaugeranStore {
    // Auth
    user: User | null;
    token: string | null;
    setUser: (user: User | null) => void;
    setToken: (token: string | null) => void;
    
    // Threads
    threads: Thread[];
    currentThreadId: string | null;
    setThreads: (threads: Thread[]) => void;
    setCurrentThread: (threadId: string | null) => void;
    
    // Messages
    messages: Record<string, Message[]>;
    addMessage: (threadId: string, message: Message) => void;
    
    // Agent state
    agentPhase: Record<string, AgentPhase>;
    setAgentPhase: (threadId: string, phase: AgentPhase) => void;
  }
  
  export const useStore = create<PaugeranStore>((set) => ({
    // Implementation
  }));
  ```
- [ ] Setup persistence (localStorage)
- [ ] Test state management

### 6.16 Error Handling
- [ ] Buat error boundary components
- [ ] Implementasi global error handler
- [ ] Display user-friendly error messages
- [ ] Setup error reporting (Sentry integration)
- [ ] Test error scenarios

### 6.17 Loading States
- [ ] Buat loading components:
  - [ ] Skeleton loaders untuk lists
  - [ ] Spinners untuk buttons
  - [ ] Progress bars untuk uploads
- [ ] Display loading states untuk:
  - [ ] Thread list loading
  - [ ] Message loading
  - [ ] Document upload
  - [ ] Report generation
- [ ] Test loading states

### 6.18 Responsive Design
- [ ] Test di berbagai screen sizes:
  - [ ] Desktop (1920x1080)
  - [ ] Laptop (1366x768)
  - [ ] Tablet (768x1024)
  - [ ] Mobile (375x667)
- [ ] Optimize layout untuk mobile:
  - [ ] Sidebar collapsible
  - [ ] Chat input fixed at bottom
  - [ ] Touch-friendly buttons
- [ ] Test responsive design

### 6.19 Accessibility
- [ ] Setup keyboard navigation:
  - [ ] Tab order
  - [ ] Enter/Space for buttons
  - [ ] Escape to close modals
- [ ] Setup ARIA labels:
  - [ ] Buttons
  - [ ] Inputs
  - [ ] Panels
- [ ] Test dengan screen reader
- [ ] Check color contrast (WCAG 2.1 AA)
- [ ] Test accessibility

### 6.20 Performance Optimization
- [ ] Setup code splitting
- [ ] Setup lazy loading untuk components
- [ ] Optimize images
- [ ] Setup caching strategy
- [ ] Test performance:
  - [ ] Lighthouse score >90
  - [ ] First Contentful Paint <1.5s
  - [ ] Time to Interactive <3s

### 6.21 Testing
- [ ] Setup testing framework (Jest + React Testing Library)
- [ ] Buat unit tests untuk components
- [ ] Buat integration tests untuk user flows
- [ ] Test coverage >70%
- [ ] Test di berbagai browsers:
  - [ ] Chrome
  - [ ] Firefox
  - [ ] Safari
  - [ ] Edge

## Output Fase 6
- ✅ Next.js frontend dengan chat-first interface
- ✅ Authentication UI berfungsi
- ✅ Thread management UI
- ✅ Chat interface dengan streaming
- ✅ Document upload UI
- ✅ Traceability panel dengan React Flow
- ✅ Report panel
- ✅ Export functionality
- ✅ Responsive design
- ✅ Accessibility compliant

## Definition of Done
Frontend dapat berkomunikasi dengan backend, user dapat login, membuat thread, mengirim pesan, upload dokumen, melihat traceability, dan export laporan. UI responsive dan accessible.

---

# FASE 7: INTEGRATION & TESTING

## Tujuan
Mengintegrasikan semua komponen dan melakukan testing komprehensif untuk memastikan produk siap deploy.

## Aturan Emas

**7.1 — End-to-End Testing is Mandatory**
> Setiap user flow harus di-test dari awal sampai akhir. Tidak ada fitur yang boleh lolos tanpa E2E test.

**7.2 — Thread Isolation Must Be Proven**
> Thread isolation harus dibuktikan dengan penetration testing. Tidak boleh ada data leakage antar thread.

**7.3 — No Hallucination in Production**
> Sistem harus di-test dengan 100+ kasus untuk memastikan tidak ada halusinasi pasal/putusan.

**7.4 — Performance Under Load**
> Sistem harus dapat handle 100 concurrent users tanpa degradation.

**7.5 — Security Audit**
> Semua endpoint harus di-audit untuk vulnerabilities (OWASP Top 10).

## Ceklist Implementasi

### 7.1 Integration Testing
- [ ] Test frontend-backend integration:
  - [ ] API client dapat connect ke backend
  - [ ] Authentication flow works end-to-end
  - [ ] Thread CRUD operations work
  - [ ] Message sending/receiving works
  - [ ] Document upload/download works
  - [ ] Streaming responses work
- [ ] Test agent integration:
  - [ ] Agent dapat dipanggil dari API
  - [ ] Agent state persists correctly
  - [ ] Agent transitions between phases correctly
  - [ ] Agent generates correct output
- [ ] Test LLM integration:
  - [ ] LLM calls work untuk semua nodes
  - [ ] Structured output is valid
  - [ ] Citations are enforced
- [ ] Test RAG integration:
  - [ ] Legal rules can be retrieved
  - [ ] Embeddings are generated correctly
  - [ ] Vector search works
- [ ] Test database integration:
  - [ ] All queries work
  - [ ] Thread isolation enforced
  - [ ] Data integrity maintained

### 7.2 End-to-End Testing
- [ ] Test complete user flows:
  - [ ] **Flow 1: New User Registration**
    - [ ] User registers dengan email
    - [ ] User receives magic link
    - [ ] User clicks link dan verified
    - [ ] User redirected to dashboard
  - [ ] **Flow 2: Create Thread & Chat**
    - [ ] User creates new thread
    - [ ] User sends first message
    - [ ] PAUGERAN responds dengan PAHAM phase
    - [ ] PAUGERAN asks questions (TANYA phase)
    - [ ] User answers questions
    - [ ] PAUGERAN confirms understanding (KONFIRMASI phase)
    - [ ] User confirms
    - [ ] PAUGERAN starts reasoning (RUMUSKAN → TELUSURI)
    - [ ] PAUGERAN generates report
  - [ ] **Flow 3: Document Upload**
    - [ ] User uploads PDF document
    - [ ] Document is parsed
    - [ ] Facts extracted dari document
    - [ ] Facts displayed di chat
  - [ ] **Flow 4: View Traceability**
    - [ ] User clicks "Peta Keterlacakan"
    - [ ] Traceability graph displayed
    - [ ] User can click nodes
    - [ ] User can zoom/pan
  - [ ] **Flow 5: Export Report**
    - [ ] User clicks "Export PDF"
    - [ ] PDF generated
    - [ ] PDF downloaded
    - [ ] PDF contains all 24 points
  - [ ] **Flow 6: Switch Threads**
    - [ ] User has multiple threads
    - [ ] User switches between threads
    - [ ] Data isolated correctly
    - [ ] No data leakage
- [ ] Automate E2E tests dengan Playwright atau Cypress
- [ ] Test coverage >80% untuk critical flows

### 7.3 Thread Isolation Testing
- [ ] Test thread isolation:
  - [ ] User A creates thread 1
  - [ ] User B creates thread 2
  - [ ] User A tries to access thread 2 → 403 Forbidden
  - [ ] User B tries to access thread 1 → 403 Forbidden
- [ ] Test database queries:
  - [ ] All queries include thread_id filter
  - [ ] No query can access data from other threads
- [ ] Test API endpoints:
  - [ ] All endpoints verify thread ownership
  - [ ] Unauthorized access returns 403
- [ ] Penetration testing:
  - [ ] Try to access other user's threads via API
  - [ ] Try to manipulate thread_id in requests
  - [ ] Try SQL injection untuk bypass isolation
- [ ] Document thread isolation test results

### 7.4 Hallucination Testing
- [ ] Prepare test dataset:
  - [ ] 50 sengketa tanah cases
  - [ ] 50 wanprestasi kontrak cases
  - [ ] 50 PHK cases
  - [ ] Total: 150 cases
- [ ] Run all cases through PAUGERAN
- [ ] Verify output:
  - [ ] No fabricated pasal numbers
  - [ ] No fabricated putusan numbers
  - [ ] No fabricated sources
  - [ ] All citations are valid
  - [ ] All legal rules exist in database
- [ ] Calculate hallucination rate:
  - [ ] Target: 0% hallucination
  - [ ] If >0%, identify root cause dan fix
- [ ] Document hallucination test results

### 7.5 Performance Testing
- [ ] Setup load testing tool (k6 atau Locust)
- [ ] Test scenarios:
  - [ ] **Scenario 1: 100 concurrent users sending messages**
    - [ ] Measure response time
    - [ ] Measure error rate
    - [ ] Target: <2s response time, <1% error rate
  - [ ] **Scenario 2: 50 concurrent analyses running**
    - [ ] Measure analysis completion time
    - [ ] Measure resource usage (CPU, RAM)
    - [ ] Target: <5min per analysis, <80% resource usage
  - [ ] **Scenario 3: 1000 documents uploaded simultaneously**
    - [ ] Measure upload time
    - [ ] Measure storage usage
    - [ ] Target: <10s per upload
- [ ] Monitor performance metrics:
  - [ ] API response time
  - [ ] Database query time
  - [ ] LLM API latency
  - [ ] Memory usage
  - [ ] CPU usage
- [ ] Identify bottlenecks dan optimize
- [ ] Document performance test results

### 7.6 Security Testing
- [ ] Run OWASP Top 10 security scan:
  - [ ] A01: Broken Access Control
    - [ ] Test authentication bypass
    - [ ] Test authorization bypass
    - [ ] Test thread isolation
  - [ ] A02: Cryptographic Failures
    - [ ] Test data encryption at rest
    - [ ] Test data encryption in transit
    - [ ] Test password hashing
  - [ ] A03: Injection
    - [ ] Test SQL injection
    - [ ] Test command injection
    - [ ] Test XSS
  - [ ] A04: Insecure Design
    - [ ] Review architecture
    - [ ] Review threat model
  - [ ] A05: Security Misconfiguration
    - [ ] Check security headers
    - [ ] Check CORS configuration
    - [ ] Check error messages
  - [ ] A06: Vulnerable and Outdated Components
    - [ ] Scan dependencies for vulnerabilities
    - [ ] Update vulnerable packages
  - [ ] A07: Identification and Authentication Failures
    - [ ] Test brute force protection
    - [ ] Test session management
  - [ ] A08: Software and Data Integrity Failures
    - [ ] Test input validation
    - [ ] Test output encoding
  - [ ] A09: Security Logging and Monitoring Failures
    - [ ] Check audit logging
    - [ ] Check monitoring
  - [ ] A10: Server-Side Request Forgery (SSRF)
    - [ ] Test browser automation whitelist
- [ ] Fix all critical dan high vulnerabilities
- [ ] Document security test results

### 7.7 User Acceptance Testing (UAT)
- [ ] Recruit 10-20 beta testers:
  - [ ] 5 advokat
  - [ ] 5 legal in-house
  - [ ] 5 akademisi hukum
  - [ ] 5 orang awam
- [ ] Prepare UAT guide:
  - [ ] Test scenarios
  - [ ] Expected outcomes
  - [ ] Feedback form
- [ ] Conduct UAT sessions:
  - [ ] Observe users menggunakan PAUGERAN
  - [ ] Collect feedback
  - [ ] Identify usability issues
- [ ] Analyze feedback:
  - [ ] Common pain points
  - [ ] Feature requests
  - [ ] Bug reports
- [ ] Fix critical issues
- [ ] Document UAT results

### 7.8 Bug Fixing
- [ ] Create bug tracking system (GitHub Issues)
- [ ] Categorize bugs:
  - [ ] Critical (blocks usage)
  - [ ] High (major feature broken)
  - [ ] Medium (minor feature broken)
  - [ ] Low (cosmetic issue)
- [ ] Fix all critical dan high bugs
- [ ] Test fixes
- [ ] Document bug fixes

### 7.9 Documentation
- [ ] Update user documentation:
  - [ ] Getting started guide
  - [ ] Feature documentation
  - [ ] FAQ
- [ ] Update API documentation:
  - [ ] OpenAPI spec up-to-date
  - [ ] Examples for all endpoints
- [ ] Update developer documentation:
  - [ ] Setup guide
  - [ ] Architecture overview
  - [ ] Deployment guide
- [ ] Create video tutorials (optional)

### 7.10 Final Checklist
- [ ] All features implemented sesuai PRD
- [ ] All tests passing
- [ ] No critical bugs
- [ ] Performance meets targets
- [ ] Security audit passed
- [ ] Documentation complete
- [ ] UAT completed dengan positive feedback

## Output Fase 7
- ✅ Semua komponen terintegrasi dan berfungsi
- ✅ E2E tests passing untuk semua user flows
- ✅ Thread isolation proven dengan penetration testing
- ✅ Hallucination rate 0% dalam 150 test cases
- ✅ Performance meets targets (100 concurrent users)
- ✅ Security audit passed (OWASP Top 10)
- ✅ UAT completed dengan positive feedback
- ✅ Documentation complete

## Definition of Done
Produk siap untuk deployment dengan confidence. Semua critical tests passing, no critical bugs, performance acceptable, dan security verified.

---

# FASE 8: DEPLOYMENT & SECURITY

## Tujuan
Deploy produk ke production environment dengan security hardening dan monitoring.

## Aturan Emas

**8.1 — Zero Downtime Deployment**
> Deployment tidak boleh menyebabkan downtime. Gunakan rolling updates atau blue-green deployment.

**8.2 — Secrets Management**
> Tidak ada secrets yang hardcoded atau committed ke repository. Semua secrets managed melalui environment variables atau secret manager.

**8.3 — HTTPS Only**
> Semua traffic harus melalui HTTPS. HTTP traffic harus redirect ke HTTPS.

**8.4 — Backup Before Deploy**
> Selalu backup database sebelum deployment. Test restore procedure.

**8.5 — Rollback Plan**
> Setiap deployment harus memiliki rollback plan. Jika deployment gagal, bisa rollback ke versi sebelumnya dalam <5 menit.

## Ceklist Implementasi

### 8.1 Production Environment Setup
- [ ] Provision production VPS:
  - [ ] 8 vCPU, 16GB RAM, 200GB SSD (recommended)
  - [ ] Ubuntu 22.04 LTS
  - [ ] Jakarta region
- [ ] Setup production domain:
  - [ ] app.paugeran.com → Vercel
  - [ ] api.paugeran.com → VPS
- [ ] Setup SSL certificates:
  - [ ] Install Certbot
  - [ ] Generate Let's Encrypt certificates
  - [ ] Setup auto-renewal
- [ ] Setup firewall (UFW):
  - [ ] Allow 22 (SSH)
  - [ ] Allow 80 (HTTP - redirect to HTTPS)
  - [ ] Allow 443 (HTTPS)
  - [ ] Deny all other ports

### 8.2 Backend Deployment
- [ ] Clone repository ke VPS:
  ```bash
  cd /opt
  git clone https://github.com/your-org/paugeran.git
  cd paugeran
  ```
- [ ] Setup environment variables:
  ```bash
  cp .env.example .env
  nano .env
  # Fill in production values
  ```
- [ ] Build Docker images:
  ```bash
  docker-compose -f infra/docker/docker-compose.prod.yml build
  ```
- [ ] Start services:
  ```bash
  docker-compose -f infra/docker/docker-compose.prod.yml up -d
  ```
- [ ] Run database migrations:
  ```bash
  docker-compose exec backend alembic upgrade head
  ```
- [ ] Verify services running:
  ```bash
  docker-compose ps
  curl http://localhost:8000/health
  ```

### 8.3 Frontend Deployment (Vercel)
- [ ] Connect GitHub repository ke Vercel
- [ ] Setup environment variables di Vercel:
  - [ ] NEXT_PUBLIC_API_URL=https://api.paugeran.com
  - [ ] NEXT_PUBLIC_WS_URL=wss://api.paugeran.com/ws
- [ ] Configure build settings:
  - [ ] Framework: Next.js
  - [ ] Build command: `pnpm build`
  - [ ] Output directory: `.next`
- [ ] Setup custom domain:
  - [ ] app.paugeran.com
  - [ ] Configure DNS
- [ ] Deploy:
  ```bash
  vercel --prod
  ```
- [ ] Verify deployment:
  - [ ] Visit https://app.paugeran.com
  - [ ] Test login
  - [ ] Test basic functionality

### 8.4 Nginx Configuration
- [ ] Setup Nginx sebagai reverse proxy:
  ```nginx
  server {
      listen 80;
      server_name api.paugeran.com;
      return 301 https://$server_name$request_uri;
  }
  
  server {
      listen 443 ssl http2;
      server_name api.paugeran.com;
      
      ssl_certificate /etc/lets
      ssl_certificate /etc/letsencrypt/live/api.paugeran.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/api.paugeran.com/privkey.pem;
      ssl_protocols TLSv1.2 TLSv1.3;
      ssl_ciphers HIGH:!aNULL:!MD5;
      ssl_prefer_server_ciphers on;
      ssl_session_cache shared:SSL:10m;
      ssl_session_timeout 10m;
      
      # Security headers
      add_header X-Frame-Options "SAMEORIGIN" always;
      add_header X-Content-Type-Options "nosniff" always;
      add_header X-XSS-Protection "1; mode=block" always;
      add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
      add_header Referrer-Policy "strict-origin-when-cross-origin" always;
      add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';" always;
      
      # Rate limiting
      limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
      limit_req_zone $binary_remote_addr zone=auth:10m rate=5r/m;
      
      # API routes
      location /api/ {
          limit_req zone=api burst=20 nodelay;
          
          proxy_pass http://backend:8000;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
          
          # WebSocket support
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
          
          # Timeouts
          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 300s;  # Longer for agent processing
          
          # Request size limit
          client_max_body_size 50M;
      }
      
      # Auth endpoints - stricter rate limit
      location /api/v1/auth/ {
          limit_req zone=auth burst=5 nodelay;
          
          proxy_pass http://backend:8000;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
      
      # Health check - no rate limit
      location /health {
          proxy_pass http://backend:8000/health;
          access_log off;
      }
      
      # Block sensitive paths
      location ~ /\.(git|env|gitignore) {
          deny all;
          return 404;
      }
  }
  ```
- [ ] Test Nginx configuration:
  ```bash
  nginx -t
  systemctl reload nginx
  ```
- [ ] Verify HTTPS working
- [ ] Verify security headers present
- [ ] Verify rate limiting working

### 8.5 CI/CD Pipeline
- [ ] Setup GitHub Actions workflows:

**`.github/workflows/ci.yml`:**
```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: pnpm/action-setup@v2
        with:
          version: 8
      
      - uses: actions/setup-node@v3
        with:
          node-version: 20
          cache: 'pnpm'
      
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Install dependencies
        run: pnpm install
      
      - name: Lint
        run: pnpm lint
      
      - name: Type check
        run: pnpm type-check
      
      - name: Test backend
        run: |
          cd apps/api
          pip install -r requirements.txt
          pytest tests/ -v --cov=app
      
      - name: Test frontend
        run: pnpm test
      
      - name: Build
        run: pnpm build
```

**`.github/workflows/deploy-backend.yml`:**
```yaml
name: Deploy Backend

on:
  push:
    branches: [main]
    paths:
      - 'apps/api/**'
      - 'packages/database/**'
      - 'infra/docker/**'

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v3
      
      - name: Backup database before deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            /opt/paugeran/tools/scripts/backup.sh
      
      - name: Deploy to VPS
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /opt/paugeran
            git pull origin main
            docker-compose -f infra/docker/docker-compose.prod.yml down
            docker-compose -f infra/docker/docker-compose.prod.yml up -d --build
            docker-compose exec -T backend alembic upgrade head
            docker system prune -af
      
      - name: Health check
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            sleep 10
            curl -f https://api.paugeran.com/health || exit 1
      
      - name: Rollback on failure
        if: failure()
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            /opt/paugeran/tools/scripts/rollback.sh
```

**`.github/workflows/deploy-frontend.yml`:**
```yaml
name: Deploy Frontend

on:
  push:
    branches: [main]
    paths:
      - 'apps/web/**'
      - 'packages/shared/**'
      - 'packages/ui/**'

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: ./apps/web
          vercel-args: '--prod'
```

- [ ] Setup GitHub secrets:
  - [ ] `VPS_HOST`
  - [ ] `VPS_USER`
  - [ ] `VPS_SSH_KEY`
  - [ ] `VERCEL_TOKEN`
  - [ ] `VERCEL_ORG_ID`
  - [ ] `VERCEL_PROJECT_ID`
- [ ] Test CI pipeline on PR
- [ ] Test deployment pipeline on push to main
- [ ] Setup branch protection rules:
  - [ ] Require PR reviews
  - [ ] Require CI passing
  - [ ] Require up-to-date branch

### 8.6 Security Hardening
- [ ] Setup fail2ban:
  ```bash
  apt install fail2ban
  ```
  ```ini
  # /etc/fail2ban/jail.local
  [sshd]
  enabled = true
  port = ssh
  filter = sshd
  logpath = /var/log/auth.log
  maxretry = 3
  bantime = 3600
  
  [nginx-http-auth]
  enabled = true
  port = http,https
  filter = nginx-http-auth
  logpath = /var/log/nginx/error.log
  maxretry = 5
  ```
- [ ] Setup automatic security updates:
  ```bash
  apt install unattended-upgrades
  dpkg-reconfigure -plow unattended-upgrades
  ```
- [ ] Setup intrusion detection (AIDE):
  ```bash
  apt install aide
  aideinit
  cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db
  ```
- [ ] Setup log rotation:
  ```bash
  # /etc/logrotate.d/paugeran
  /opt/paugeran/data/logs/*.log {
      daily
      rotate 30
      compress
      delaycompress
      missingok
      notifempty
      create 0640 root root
  }
  ```
- [ ] Disable root SSH login:
  ```bash
  # /etc/ssh/sshd_config
  PermitRootLogin no
  PasswordAuthentication no
  ```
- [ ] Setup SSH key-only authentication
- [ ] Change SSH port (optional, security through obscurity)
- [ ] Setup regular security scans:
  ```bash
  # Weekly scan
  0 2 * * 0 /opt/paugeran/tools/scripts/security-scan.sh
  ```
- [ ] Test all security measures

### 8.7 Backup & Recovery Setup
- [ ] Create backup script `tools/scripts/backup.sh`:
  ```bash
  #!/bin/bash
  set -e
  
  DATE=$(date +%Y%m%d_%H%M%S)
  BACKUP_DIR="/opt/paugeran/backups"
  RETENTION_DAYS=30
  
  mkdir -p $BACKUP_DIR
  
  echo "Starting backup at $(date)"
  
  # Database backup
  echo "Backing up database..."
  docker exec paugeran-postgres pg_dump -U paugeran paugeran | gzip > $BACKUP_DIR/db_$DATE.sql.gz
  
  # Documents backup
  echo "Backing up documents..."
  tar -czf $BACKUP_DIR/documents_$DATE.tar.gz -C /opt/paugeran/data documents
  
  # Config backup
  echo "Backing up config..."
  tar -czf $BACKUP_DIR/config_$DATE.tar.gz -C /opt/paugeran .env infra/
  
  # Cleanup old backups
  echo "Cleaning up old backups..."
  find $BACKUP_DIR -name "*.gz" -mtime +$RETENTION_DAYS -delete
  find $BACKUP_DIR -name "*.tar.gz" -mtime +$RETENTION_DAYS -delete
  
  # Verify backup
  echo "Verifying backup..."
  gunzip -t $BACKUP_DIR/db_$DATE.sql.gz
  tar -tzf $BACKUP_DIR/documents_$DATE.tar.gz > /dev/null
  
  echo "Backup completed at $(date)"
  echo "Backup size: $(du -sh $BACKUP_DIR | cut -f1)"
  ```
- [ ] Create restore script `tools/scripts/restore.sh`:
  ```bash
  #!/bin/bash
  set -e
  
  if [ -z "$1" ]; then
      echo "Usage: $0 <backup_date>"
      echo "Example: $0 20260826_020000"
      exit 1
  fi
  
  BACKUP_DATE=$1
  BACKUP_DIR="/opt/paugeran/backups"
  
  echo "Starting restore from $BACKUP_DATE"
  
  # Stop services
  echo "Stopping services..."
  docker-compose -f infra/docker/docker-compose.prod.yml down
  
  # Restore database
  echo "Restoring database..."
  gunzip -c $BACKUP_DIR/db_$BACKUP_DATE.sql.gz | docker exec -i paugeran-postgres psql -U paugeran paugeran
  
  # Restore documents
  echo "Restoring documents..."
  tar -xzf $BACKUP_DIR/documents_$BACKUP_DATE.tar.gz -C /opt/paugeran/data
  
  # Start services
  echo "Starting services..."
  docker-compose -f infra/docker/docker-compose.prod.yml up -d
  
  # Verify
  echo "Verifying restore..."
  sleep 10
  curl -f http://localhost:8000/health
  
  echo "Restore completed"
  ```
- [ ] Create rollback script `tools/scripts/rollback.sh`:
  ```bash
  #!/bin/bash
  set -e
  
  echo "Rolling back to previous version..."
  
  cd /opt/paugeran
  
  # Get previous commit
  PREV_COMMIT=$(git rev-parse HEAD~1)
  
  echo "Rolling back to commit: $PREV_COMMIT"
  
  # Checkout previous version
  git checkout $PREV_COMMIT
  
  # Rebuild and restart
  docker-compose -f infra/docker/docker-compose.prod.yml down
  docker-compose -f infra/docker/docker-compose.prod.yml up -d --build
  
  # Verify
  sleep 10
  curl -f http://localhost:8000/health
  
  echo "Rollback completed"
  ```
- [ ] Setup automated backup schedule:
  ```bash
  # Crontab
  0 2 * * * /opt/paugeran/tools/scripts/backup.sh >> /var/log/paugeran-backup.log 2>&1
  ```
- [ ] Test backup and restore:
  - [ ] Run backup manually
  - [ ] Verify backup files created
  - [ ] Test restore on staging
  - [ ] Verify data integrity after restore
- [ ] Setup off-site backup (optional):
  - [ ] Upload to S3-compatible storage
  - [ ] Or rsync to secondary server

### 8.8 Environment Variables Management
- [ ] Create production `.env` file:
  ```bash
  # Database
  DB_PASSWORD=<strong-random-password>
  DATABASE_URL=postgresql://paugeran:${DB_PASSWORD}@postgres:5432/paugeran
  
  # Redis
  REDIS_URL=redis://redis:6379
  
  # JWT
  JWT_SECRET=<strong-random-secret-256-bit>
  
  # API Keys
  ANTHROPIC_API_KEY=sk-ant-...
  OPENAI_API_KEY=sk-...
  LANGSMITH_API_KEY=ls-...
  
  # CORS
  CORS_ORIGINS=https://app.paugeran.com
  
  # File storage
  DOCUMENT_STORAGE_PATH=/opt/paugeran/data/documents
  
  # Sentry (optional)
  SENTRY_DSN=https://...@sentry.io/...
  ```
- [ ] Set proper file permissions:
  ```bash
  chmod 600 .env
  chown root:root .env
  ```
- [ ] Verify no secrets in git history:
  ```bash
  git log -p | grep -i "password\|secret\|api_key"
  ```
- [ ] Setup secret rotation plan (quarterly)

### 8.9 Deployment Verification
- [ ] Verify all services running:
  ```bash
  docker-compose ps
  ```
- [ ] Verify health endpoints:
  ```bash
  curl https://api.paugeran.com/health
  curl https://app.paugeran.com
  ```
- [ ] Verify SSL certificates:
  ```bash
  openssl s_client -connect api.paugeran.com:443
  ```
- [ ] Verify security headers:
  ```bash
  curl -I https://api.paugeran.com
  ```
- [ ] Verify rate limiting:
  ```bash
  for i in {1..20}; do curl -s https://api.paugeran.com/api/v1/auth/login; done
  ```
- [ ] Verify database connectivity
- [ ] Verify Redis connectivity
- [ ] Verify file storage accessible
- [ ] Verify LLM API calls working
- [ ] Verify email service working (magic link)

### 8.10 Monitoring Setup
- [ ] Setup Prometheus:
  ```yaml
  # infra/monitoring/prometheus.yml
  global:
    scrape_interval: 15s
    
  scrape_configs:
    - job_name: 'backend'
      static_configs:
        - targets: ['backend:8000']
      metrics_path: '/metrics'
    
    - job_name: 'postgres'
      static_configs:
        - targets: ['postgres-exporter:9187']
    
    - job_name: 'redis'
      static_configs:
        - targets: ['redis-exporter:9121']
    
    - job_name: 'nginx'
      static_configs:
        - targets: ['nginx-exporter:9113']
  ```
- [ ] Setup Grafana:
  - [ ] Import dashboards:
    - [ ] System metrics (CPU, RAM, Disk)
    - [ ] API metrics (requests, latency, errors)
    - [ ] Database metrics (queries, connections)
    - [ ] Agent metrics (phases, duration, tokens)
    - [ ] Business metrics (users, threads, analyses)
  - [ ] Setup alerts
- [ ] Setup Uptime monitoring:
  - [ ] UptimeRobot atau Pingdom
  - [ ] Monitor endpoints:
    - [ ] https://app.paugeran.com
    - [ ] https://api.paugeran.com/health
    - [ ] https://api.paugeran.com/api/v1/auth/login
  - [ ] Setup alert contacts (email, Slack, SMS)

### 8.11 Production Checklist
- [ ] All services running and healthy
- [ ] SSL certificates valid
- [ ] Security headers present
- [ ] Rate limiting active
- [ ] Backup schedule configured
- [ ] Monitoring active
- [ ] Alerts configured
- [ ] CI/CD pipeline working
- [ ] Rollback plan tested
- [ ] Documentation updated
- [ ] Team trained on operations

## Output Fase 8
- ✅ Backend deployed ke VPS dengan Docker Compose
- ✅ Frontend deployed ke Vercel dengan custom domain
- ✅ Nginx reverse proxy dengan SSL dan security headers
- ✅ CI/CD pipeline otomatis dari GitHub
- ✅ Security hardening selesai (fail2ban, auto-updates, AIDE)
- ✅ Backup & recovery teruji
- ✅ Monitoring aktif (Prometheus + Grafana + UptimeRobot)
- ✅ Rollback plan terdokumentasi dan teruji

## Definition of Done
Produk live di production dengan zero downtime deployment capability, security hardening selesai, backup automated, monitoring aktif, dan rollback plan teruji. Tim ops dapat merespon insiden dalam <5 menit.

---

# FASE 9: MONITORING & LAUNCH READINESS

## Tujuan
Memastikan produk siap diluncurkan ke publik dengan monitoring komprehensif, dokumentasi operasional, dan prosedur go-live yang terstruktur.

## Aturan Emas

**9.1 — Observability is Non-Negotiable**
> Tidak ada deployment tanpa monitoring. Setiap metrik kritis harus terukur dan teralert.

**9.2 — Runbook for Every Alert**
> Setiap alert harus memiliki runbook yang jelas. Tim harus tahu apa yang harus dilakukan dalam <5 menit.

**9.3 — Launch is a Process, Not an Event**
> Go-live bukan sekadar menekan tombol deploy. Ada checklist pra-launch, prosedur launch, dan monitoring pasca-launch.

**9.4 — User Feedback Loop**
> Mekanisme untuk menerima, mengkategorisasi, dan merespons feedback pengguna harus siap sebelum launch.

**9.5 — Legal & Compliance Ready**
> Semua aspek legal (Terms of Service, Privacy Policy, DPA) harus siap sebelum launch publik.

## Ceklist Implementasi

### 9.1 Comprehensive Monitoring

#### 9.1.1 Application Performance Monitoring (APM)
- [ ] Setup LangSmith untuk agent tracing:
  - [ ] Trace setiap node execution
  - [ ] Monitor token usage per analysis
  - [ ] Track cost per analysis
  - [ ] Monitor latency per phase
  - [ ] Setup dashboard untuk:
    - [ ] Average analysis duration
    - [ ] Token usage trends
    - [ ] Cost per user
    - [ ] Error rates per node
    - [ ] Citation validation success rate
- [ ] Setup Sentry untuk error tracking:
  - [ ] Install Sentry SDK di backend
  - [ ] Install Sentry SDK di frontend
  - [ ] Configure error boundaries
  - [ ] Setup alert rules:
    - [ ] Critical errors: immediate alert
    - [ ] Error rate spike: alert if >1% in 5 minutes
    - [ ] New error types: alert
  - [ ] Test error reporting
- [ ] Setup custom metrics di backend:
  ```python
  # apps/api/app/core/metrics.py
  from prometheus_client import Counter, Histogram, Gauge
  
  # Counters
  REQUEST_COUNT = Counter('paugeran_requests_total', 'Total requests', ['method', 'endpoint', 'status'])
  AGENT_PHASE_COUNT = Counter('paugeran_agent_phase_total', 'Agent phase executions', ['phase'])
  LLM_CALL_COUNT = Counter('paugeran_llm_calls_total', 'LLM API calls', ['model', 'task'])
  ANALYSIS_COUNT = Counter('paugeran_analyses_total', 'Completed analyses', ['status'])
  
  # Histograms
  REQUEST_DURATION = Histogram('paugeran_request_duration_seconds', 'Request duration', ['endpoint'])
  AGENT_PHASE_DURATION = Histogram('paugeran_agent_phase_duration_seconds', 'Agent phase duration', ['phase'])
  LLM_TOKEN_USAGE = Histogram('paugeran_llm_tokens', 'LLM token usage', ['model', 'type'])
  
  # Gauges
  ACTIVE_THREADS = Gauge('paugeran_active_threads', 'Currently active threads')
  ACTIVE_USERS = Gauge('paugeran_active_users', 'Currently active users')
  DB_CONNECTIONS = Gauge('paugeran_db_connections', 'Database connections')
  ```
- [ ] Expose metrics endpoint:
  ```python
  # apps/api/app/main.py
  from prometheus_client import make_asgi_app
  
  metrics_app = make_asgi_app()
  app.mount("/metrics", metrics_app)
  ```

#### 9.1.2 Infrastructure Monitoring
- [ ] Setup node_exporter untuk system metrics:
  ```yaml
  # Tambahkan ke docker-compose.prod.yml
  node-exporter:
    image: prom/node-exporter
    container_name: paugeran-node-exporter
    restart: always
    pid: host
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.sysfs=/host/sys'
      - '--path.rootfs=/rootfs'
  ```
- [ ] Setup postgres_exporter:
  ```yaml
  postgres-exporter:
    image: prometheuscommunity/postgres-exporter
    container_name: paugeran-postgres-exporter
    restart: always
    environment:
      - DATA_SOURCE_NAME=postgresql://paugeran:${DB_PASSWORD}@postgres:5432/paugeran?sslmode=disable
  ```
- [ ] Setup redis_exporter:
  ```yaml
  redis-exporter:
    image: oliver006/redis_exporter
    container_name: paugeran-redis-exporter
    restart: always
    environment:
      - REDIS_ADDR=redis://redis:6379
  ```
- [ ] Setup nginx-exporter:
  ```yaml
  nginx-exporter:
    image: nginx/nginx-prometheus-exporter
    container_name: paugeran-nginx-exporter
    restart: always
    command:
      - '-nginx.scrape-uri=http://nginx/stub_status'
  ```
- [ ] Verify all exporters working:
  ```bash
  curl http://localhost:9100/metrics  # node-exporter
  curl http://localhost:9187/metrics  # postgres-exporter
  curl http://localhost:9121/metrics  # redis-exporter
  curl http://localhost:9113/metrics  # nginx-exporter
  ```

#### 9.1.3 Grafana Dashboards
- [ ] Import/create dashboards:

**Dashboard 1: System Overview**
- [ ] CPU usage per core
- [ ] Memory usage
- [ ] Disk usage
- [ ] Network I/O
- [ ] Load average

**Dashboard 2: API Performance**
- [ ] Request rate per endpoint
- [ ] Response time percentiles (p50, p95, p99)
- [ ] Error rate per endpoint
- [ ] Active connections
- [ ] Rate limit violations

**Dashboard 3: Agent Metrics**
- [ ] Analyses started/completed per hour
- [ ] Average duration per phase
- [ ] Phase transition success rate
- [ ] Token usage per analysis
- [ ] Cost per analysis
- [ ] Citation validation rate

**Dashboard 4: Database Performance**
- [ ] Query duration
- [ ] Active connections
- [ ] Cache hit ratio
- [ ] Replication lag (jika ada replica)
- [ ] Deadlocks

**Dashboard 5: Business Metrics**
- [ ] Active users (DAU/WAU/MAU)
- [ ] New registrations
- [ ] Threads created per day
- [ ] Documents uploaded
- [ ] Reports exported
- [ ] User retention

- [ ] Setup dashboard variables untuk filtering
- [ ] Setup dashboard sharing (read-only links)
- [ ] Test all dashboards

#### 9.1.4 Alerting
- [ ] Setup Alertmanager:
  ```yaml
  # infra/monitoring/alertmanager.yml
  global:
    resolve_timeout: 5m
    
  route:
    group_by: ['alertname']
    group_wait: 10s
    group_interval: 10s
    repeat_interval: 1h
    receiver: 'web.hook'
    routes:
      - match:
          severity: critical
        receiver: 'critical-alerts'
      - match:
          severity: warning
        receiver: 'warning-alerts'
  
  receivers:
    - name: 'critical-alerts'
      slack_configs:
        - api_url: 'https://hooks.slack.com/services/...'
          channel: '#paugeran-critical'
          title: '🚨 Critical Alert'
          text: '{{ .CommonAnnotations.description }}'
      email_configs:
        - to: 'ops@paugeran.com'
          subject: '🚨 Critical: {{ .GroupLabels.alertname }}'
    
    - name: 'warning-alerts'
      slack_configs:
        - api_url: 'https://hooks.slack.com/services/...'
          channel: '#paugeran-warnings'
          title: '⚠️ Warning'
          text: '{{ .CommonAnnotations.description }}'
  ```
- [ ] Define alert rules:
  ```yaml
  # infra/monitoring/alert-rules.yml
  groups:
    - name: paugeran-alerts
      rules:
        # Infrastructure
        - alert: HighCPUUsage
          expr: 100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: "High CPU usage on {{ $labels.instance }}"
            description: "CPU usage is above 80% for 5 minutes"
        
        - alert: HighMemoryUsage
          expr: (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 90
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: "High memory usage on {{ $labels.instance }}"
        
        - alert: DiskSpaceLow
          expr: (node_filesystem_avail_bytes / node_filesystem_size_bytes) * 100 < 10
          for: 5m
          labels:
            severity: critical
          annotations:
            summary: "Disk space low on {{ $labels.instance }}"
        
        # Application
        - alert: HighErrorRate
          expr: rate(paugeran_requests_total{status=~"5.."}[5m]) / rate(paugeran_requests_total[5m]) > 0.01
          for: 5m
          labels:
            severity: critical
          annotations:
            summary: "High error rate"
            description: "Error rate is above 1% for 5 minutes"
        
        - alert: HighLatency
          expr: histogram_quantile(0.95, rate(paugeran_request_duration_seconds_bucket[5m])) > 5
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: "High API latency"
            description: "95th percentile latency is above 5 seconds"
        
        - alert: AgentPhaseStuck
          expr: rate(paugeran_agent_phase_duration_seconds_sum[30m]) / rate(paugeran_agent_phase_duration_seconds_count[30m]) > 300
          for: 10m
          labels:
            severity: warning
          annotations:
            summary: "Agent phase taking too long"
        
        - alert: HighLLMCost
          expr: sum(rate(paugeran_llm_tokens_total[1h])) * 0.01 > 10
          for: 1h
          labels:
            severity: warning
          annotations:
            summary: "High LLM API cost"
            description: "LLM API cost is above $10/hour"
        
        # Database
        - alert: DatabaseConnectionPoolExhausted
          expr: paugeran_db_connections > 80
          for: 5m
          labels:
            severity: critical
          annotations:
            summary: "Database connection pool exhausted"
        
        - alert: SlowQueries
          expr: rate(paugeran_db_query_duration_seconds_sum[5m]) / rate(paugeran_db_query_duration_seconds_count[5m]) > 1
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: "Slow database queries"
        
        # Service Health
        - alert: ServiceDown
          expr: up == 0
          for: 1m
          labels:
            severity: critical
          annotations:
            summary: "Service {{ $labels.job }} is down"
  ```
- [ ] Test alerts dengan simulated failures
- [ ] Setup on-call rotation (jika tim >1 orang)
- [ ] Document escalation procedures

### 9.2 Logging & Audit

#### 9.2.1 Centralized Logging
- [ ] Setup Loki untuk log aggregation:
  ```yaml
  # Tambahkan ke docker-compose.prod.yml
  loki:
    image: grafana/loki:2.9.0
    container_name: paugeran-loki
    restart: always
    volumes:
      - loki_data:/loki
      - ./infra/monitoring/loki-config.yml:/etc/loki/local-config.yaml
  
  promtail:
    image: grafana/promtail:2.9.0
    container_name: paugeran-promtail
    restart: always
    volumes:
      - /var/log:/var/log
      - ./infra/monitoring/promtail-config.yml:/etc/promtail/config.yml
  ```
- [ ] Setup log format standar:
  ```python
  # apps/api/app/core/logging.py
  import logging
  import json
  from pythonjsonlogger import jsonlogger
  
  class CustomJsonFormatter(jsonlogger.JsonFormatter):
      def add_fields(self, log_record, record, message_dict):
          super().add_fields(log_record, record, message_dict)
          log_record['timestamp'] = datetime.utcnow().isoformat()
          log_record['level'] = record.levelname
          log_record['logger'] = record.name
          if hasattr(record, 'user_id'):
              log_record['user_id'] = record.user_id
          if hasattr(record, 'thread_id'):
              log_record['thread_id'] = record.thread_id
          if hasattr(record, 'request_id'):
              log_record['request_id'] = record.request_id
  
  logger = logging.getLogger(__name__)
  handler = logging.StreamHandler()
  handler.setFormatter(CustomJsonFormatter())
  logger.addHandler(handler)
  logger.setLevel(logging.INFO)
  ```
- [ ] Setup log levels:
  - [ ] DEBUG: Development only
  - [ ] INFO: Normal operations
  - [ ] WARNING: Potential issues
  - [ ] ERROR: Errors that need attention
  - [ ] CRITICAL: System down
- [ ] Setup log retention:
  - [ ] Info logs: 30 days
  - [ ] Error logs: 1 year
  - [ ] Audit logs: 2 years
- [ ] Setup log rotation
- [ ] Verify logs di Grafana

#### 9.2.2 Audit Trail
- [ ] Implementasi audit logging untuk semua aksi kritis:
  ```python
  # apps/api/app/core/audit.py
  async def log_audit(
      db: Session,
      user_id: str,
      action: str,
      resource_type: str,
      resource_id: str,
      details: dict,
      ip_address: str
  ):
      audit_log = AuditLog(
          user_id=user_id,
          action=action,
          resource_type=resource_type,
          resource_id=resource_id,
          details=details,
          ip_address=ip_address,
          created_at=datetime.utcnow()
      )
      db.add(audit_log)
      await db.commit()
  
  # Usage examples
  await log_audit(db, user_id, "thread_created", "thread", thread_id, {"title": title}, ip)
  await log_audit(db, user_id, "document_uploaded", "document", doc_id, {"filename": filename}, ip)
  await log_audit(db, user_id, "analysis_started", "thread", thread_id, {}, ip)
  await log_audit(db, user_id, "report_exported", "thread", thread_id, {"format": "pdf"}, ip)
  ```
- [ ] Audit events yang harus di-log:
  - [ ] User registration
  - [ ] User login/logout
  - [ ] Thread created/updated/deleted
  - [ ] Message sent
  - [ ] Document uploaded/deleted
  - [ ] Analysis started/completed
  - [ ] Report exported
  - [ ] Profile updated
  - [ ] Account deleted
  - [ ] Failed login attempts
  - [ ] Authorization failures
- [ ] Setup audit log query interface (admin panel)
- [ ] Setup audit log export functionality
- [ ] Test audit logging

### 9.3 Legal & Compliance

#### 9.3.1 Terms of Service
- [ ] Draft Terms of Service (ToS):
  - [ ] Acceptance of terms
  - [ ] Description of service
  - [ ] User accounts
  - [ ] Acceptable use policy
  - [ ] Intellectual property
  - [ ] Payment terms (jika ada)
  - [ ] Disclaimer of warranties
  - [ ] Limitation of liability
  - [ ] Indemnification
  - [ ] Termination
  - [ ] Governing law (Indonesia)
  - [ ] Dispute resolution
- [ ] Review oleh lawyer
- [ ] Publish di website: `https://app.paugeran.com/terms`
- [ ] Require user acceptance saat registration

#### 9.3.2 Privacy Policy
- [ ] Draft Privacy Policy:
  - [ ] Information collected
  - [ ] How information is used
  - [ ] Data storage and security
  - [ ] Third-party services (Anthropic, OpenAI, Vercel)
  - [ ] Data retention
  - [ ] User rights (access, correction, deletion)
  - [ ] Children's privacy
  - [ ] Changes to policy
  - [ ] Contact information
- [ ] Ensure compliance dengan UU PDP (Indonesia)
- [ ] Review oleh lawyer
- [ ] Publish di website: `https://app.paugeran.com/privacy`
- [ ] Link di footer dan registration

#### 9.3.3 Data Processing Agreement (DPA)
- [ ] Draft DPA untuk enterprise customers:
  - [ ] Scope of processing
  - [ ] Data protection measures
  - [ ] Sub-processors (Anthropic, OpenAI, Vercel)
  - [ ] Data breach notification
  - [ ] Audit rights
  - [ ] Data transfer mechanisms
- [ ] Template siap untuk customisasi per klien
- [ ] Review oleh lawyer

#### 9.3.4 Cookie Policy
- [ ] Draft Cookie Policy:
  - [ ] Types of cookies used
  - [ ] Purpose of each cookie
  - [ ] How to manage cookies
- [ ] Implementasi cookie consent banner
- [ ] Publish di website

#### 9.3.5 Disclaimer
- [ ] Draft Legal Disclaimer:
  - [ ] PAUGERAN adalah alat bantu, bukan pengganti advokat
  - [ ] Output tidak构成 nasihat hukum formal
  - [ ] Pengguna harus konsultasi dengan advokat untuk tindakan hukum
  - [ ] Tidak ada jaminan akurasi 100%
  - [ ] Pengguna bertanggung jawab atas penggunaan output
- [ ] Display di:
  - [ ] Footer website
  - [ ] Setiap laporan yang di-generate
  - [ ] Onboarding flow
- [ ] Require user acknowledgement

### 9.4 User Support & Feedback

#### 9.4.1 Support Channels
- [ ] Setup support email: `support@paugeran.com`
- [ ] Setup in-app help center:
  - [ ] FAQ
  - [ ] User guides
  - [ ] Video tutorials (optional)
  - [ ] Search functionality
- [ ] Setup knowledge base (Notion/Confluence/GitBook)
- [ ] Setup ticketing system (Zendesk/Freshdesk/Intercom)
- [ ] Setup live chat (optional, untuk premium users)

#### 9.4.2 Feedback Collection
- [ ] Implementasi in-app feedback widget:
  ```typescript
  // components/FeedbackWidget.tsx
  export function FeedbackWidget() {
    const [open, setOpen] = useState(false);
    const [rating, setRating] = useState(0);
    const [comment, setComment] = useState('');
    
    const handleSubmit = async () => {
      await api.submitFeedback({ rating, comment });
      setOpen(false);
    };
    
    return (
      <div className="fixed bottom-4 right-4">
        <button onClick={() => setOpen(!open)}>
          💬 Feedback
        </button>
        {open && (
          <div className="bg-white p-4 rounded-lg shadow-lg">
            <h3>Berikan Feedback</h3>
            <div>
              {[1,2,3,4,5].map(i => (
                <button key={i} onClick={() => setRating(i)}>
                  {i <= rating ? '⭐' : '☆'}
                </button>
              ))}
            </div>
            <textarea
              value={comment}
              onChange={(e) => setComment(e.target.value)}
              placeholder="Komentar..."
            />
            <button onClick={handleSubmit}>Kirim</button>
          </div>
        )}
      </div>
    );
  }
  ```
- [ ] Setup feedback database table:
  ```sql
  CREATE TABLE feedback (
      id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
      user_id UUID REFERENCES users(id),
      rating INTEGER CHECK (rating >= 1 AND rating <= 5),
      comment TEXT,
      context JSONB,  -- thread_id, phase, etc
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```
- [ ] Setup feedback dashboard di Grafana
- [ ] Setup weekly feedback review meeting

#### 9.4.3 Bug Reporting
- [ ] Setup bug reporting flow:
  - [ ] In-app "Report Bug" button
  - [ ] Auto-collect context (user ID, thread ID, phase, error)
  - [ ] Submit ke ticketing system
- [ ] Setup bug triage process:
  - [ ] Categorize: Critical, High, Medium, Low
  - [ ] Assign to developer
  - [ ] Track status
  - [ ] Notify user when fixed

### 9.5 Launch Readiness Checklist

#### 9.5.1 Technical Readiness
- [ ] All features implemented sesuai PRD
- [ ] All tests passing (unit, integration, E2E)
- [ ] Performance tests passed (100 concurrent users)
- [ ] Security audit passed (OWASP Top 10)
- [ ] Thread isolation verified
- [ ] Hallucination rate 0% in 150+ test cases
- [ ] Backup & restore tested
- [ ] Rollback plan tested
- [ ] Monitoring active dengan alerts
- [ ] Logging comprehensive
- [ ] Error tracking active (Sentry)
- [ ] API documentation up-to-date
- [ ] Mobile responsive verified
- [ ] Cross-browser tested (Chrome, Firefox, Safari, Edge)
- [ ] Accessibility verified (WCAG 2.1 AA)

#### 9.5.2 Operational Readiness
- [ ] Runbook documented untuk semua alerts
- [ ] On-call rotation setup (jika applicable)
- [ ] Incident response plan documented
- [ ] Disaster recovery plan tested
- [ ] Team trained on operations
- [ ] Escalation procedures defined
- [ ] Communication templates prepared (incident notifications)
- [ ] Status page setup (opsional: status.paugeran.com)

#### 9.5.3 Business Readiness
- [ ] Pricing model finalized (free/freemium/subscription)
- [ ] Payment integration (jika berbayar)
- [ ] Billing system ready
- [ ] Invoice generation working
- [ ] Refund policy defined
- [ ] Customer support team trained
- [ ] FAQ and help center populated
- [ ] Marketing materials ready
- [ ] Launch announcement prepared
- [ ] Social media accounts setup

#### 9.5.4 Legal Readiness
- [ ] Terms of Service published
- [ ] Privacy Policy published
- [ ] Cookie Policy published
- [ ] Legal disclaimer displayed
- [ ] DPA template ready (untuk enterprise)
- [ ] GDPR/UU PDP compliance verified
- [ ] Data processing agreements dengan sub-processors
- [ ] Legal review completed

#### 9.5.5 Documentation Readiness
- [ ] User documentation complete
- [ ] API documentation complete
- [ ] Developer documentation complete
- [ ] Operations manual complete
- [ ] Troubleshooting guide complete
- [ ] Video tutorials (optional)
- [ ] Onboarding flow tested

### 9.6 Go-Live Procedure

#### 9.6.1 Pre-Launch (H-7)
- [ ] Final code freeze
- [ ] Final round of testing
- [ ] Backup production database
- [ ] Verify all monitoring active
- [ ] Verify all alerts working
- [ ] Prepare launch announcement
- [ ] Brief support team
- [ ] Prepare rollback plan
- [ ] Schedule launch window (low traffic time)

#### 9.6.2 Launch Day
- [ ] Morning (H-0, 08:00):
  - [ ] Final health check semua services
  - [ ] Verify monitoring dashboards
  - [ ] Verify alerts working
  - [ ] Team standup untuk launch
- [ ] Pre-Launch (H-0, 10:00):
  - [ ] Announce maintenance window (jika perlu)
  - [ ] Final backup
  - [ ] Deploy final version
  - [ ] Verify deployment successful
- [ ] Launch (H-0, 12:00):
  - [ ] Remove maintenance page
  - [ ] Open access to public
  - [ ] Monitor closely untuk 1 jam pertama
  - [ ] Send launch announcement
  - [ ] Post di social media
- [ ] Post-Launch (H-0, 13:00-18:00):
  - [ ] Monitor metrics setiap 15 menit
  - [ ] Watch for errors di Sentry
  - [ ] Monitor user feedback
  - [ ] Respond to support tickets cepat
  - [ ] Team on standby untuk issues

#### 9.6.3 Post-Launch (H+1 sampai H+7)
- [ ] H+1:
  - [ ] Review metrics dari launch day
  - [ ] Identify any issues
  - [ ] Respond to user feedback
  - [ ] Fix critical bugs segera
- [ ] H+2 sampai H+3:
  - [ ] Monitor trends
  - [ ] Optimize performance jika perlu
  - [ ] Continue user support
- [ ] H+4 sampai H+7:
  - [ ] Weekly review meeting
  - [ ] Analyze user behavior
  - [ ] Plan improvements berdasarkan feedback
  - [ ] Prepare weekly report untuk stakeholders

### 9.7 Post-Launch Monitoring

#### 9.7.1 Key Metrics to Track
- [ ] **User Metrics:**
  - [ ] Daily Active Users (DAU)
  - [ ] Weekly Active Users (WAU)
  - [ ] Monthly Active Users (MAU)
  - [ ] New registrations per day
  - [ ] User retention rate (D1, D7, D30)
  - [ ] Churn rate

- [ ] **Usage Metrics:**
  - [ ] Threads created per day
  - [ ] Messages sent per day
  - [ ] Documents uploaded per day
  - [ ] Reports exported per day
  - [ ] Average analysis duration
  - [ ] Completion rate (analyses started vs completed)

- [ ] **Performance Metrics:**
  - [ ] API response time (p50, p95, p99)
  - [ ] Error rate
  - [ ] Uptime percentage
  - [ ] Agent phase duration
  - [ ] LLM API latency

- [ ] **Business Metrics:**
  - [ ] Revenue (jika berbayar)
  - [ ] Customer Acquisition Cost (CAC)
  - [ ] Lifetime Value (LTV)
  - [ ] Net Promoter Score (NPS)
  - [ ] Customer Satisfaction (CSAT)

- [ ] **Cost Metrics:**
  - [ ] LLM API cost per analysis
  - [ ] Infrastructure cost per user
  - [ ] Total monthly cost
  - [ ] Cost per revenue (jika berbayar)

#### 9.7.2 Weekly Review
- [ ] Setup weekly review meeting:
  - [ ] Review metrics dari minggu sebelumnya
  - [ ] Identify trends
  - [ ] Discuss user feedback
  - [ ] Review incidents
  - [ ] Plan improvements
  - [ ] Set goals untuk minggu berikutnya
- [ ] Prepare weekly report:
  ```markdown
  # PAUGERAN Weekly Report - Week X
  
  ## Key Metrics
  - DAU: X (+Y% vs last week)
  - New registrations: X
  - Analyses completed: X
  - Uptime: X%
  - Error rate: X%
  
  ## Highlights
  - ...
  
  ## Issues
  - ...
  
  ## User Feedback
  - ...
  
  ## Action Items
  - ...
  ```

#### 9.7.3 Monthly Review
- [ ] Setup monthly review meeting dengan stakeholders
- [ ] Prepare monthly report:
  - [ ] Comprehensive metrics analysis
  - [ ] User growth trends
  - [ ] Revenue analysis (jika applicable)
  - [ ] Cost analysis
  - [ ] Product improvements
  - [ ] Roadmap untuk bulan berikutnya
- [ ] Present to stakeholders
- [ ] Adjust strategy berdasarkan data

### 9.8 Continuous Improvement

#### 9.8.1 Feedback Loop
- [ ] Collect feedback continuously:
  - [ ] In-app feedback widget
  - [ ] Support tickets
  - [ ] User interviews (monthly)
  - [ ] Surveys (quarterly)
- [ ] Categorize feedback:
  - [ ] Bugs
  - [ ] Feature requests
  - [ ] UX improvements
  - [ ] Performance issues
- [ ] Prioritize berdasarkan:
  - [ ] Impact (how many users affected)
  - [ ] Urgency (severity)
  - [ ] Effort (development time)
  - [ ] Strategic alignment

#### 9.8.2 Iteration Cycle
- [ ] Setup 2-week iteration cycle:
  - [ ] Week 1: Development
  - [ ] Week 2: Testing & deployment
- [ ] Each iteration:
  - [ ] Select items dari backlog
  - [ ] Develop
  - [ ] Test
  - [ ] Deploy
  - [ ] Monitor
  - [ ] Collect feedback
  - [ ] Repeat

#### 9.8.3 A/B Testing
- [ ] Setup A/B testing framework (optional):
  - [ ] Test different prompts
  - [ ] Test different UI layouts
  - [ ] Test different features
- [ ] Measure impact on:
  - [ ] User satisfaction
  - [ ] Task completion rate
  - [ ] Time to insight
  - [ ] Error rate

### 9.9 Launch Readiness Sign-Off

#### 9.9.1 Final Checklist
- [ ] **Technical:**
  - [ ] All tests passing ✅
  - [ ] Performance targets met ✅
  - [ ] Security audit passed ✅
  - [ ] Monitoring active ✅
  - [ ] Backup tested ✅

- [ ] **Operational:**
  - [ ] Runbooks documented ✅
  - [ ] Team trained ✅
  - [ ] Support channels ready ✅
  - [ ] Incident response plan ✅

- [ ] **Business:**
  - [ ] Pricing finalized ✅
  - [ ] Payment system ready ✅
  - [ ] Marketing materials ready ✅
  - [ ] Launch announcement prepared ✅

- [ ] **Legal:**
  - [ ] ToS published ✅
  - [ ] Privacy Policy published ✅
  - [ ] Legal disclaimer displayed ✅
  - [ ] Compliance verified ✅

#### 9.9.2 Go/No-Go Decision
- [ ] Conduct go/no-go meeting dengan semua stakeholders
- [ ] Review semua checklist items
- [ ] Address any blockers
- [ ] Make final decision:
  - [ ] **GO**: Launch sesuai jadwal
  - [ ] **NO-GO**: Delay launch, address issues
  - [ ] **CONDITIONAL GO**: Launch dengan catatan (certain features disabled)
- [ ] Document decision dan rationale

#### 9.9.3 Launch Authorization
- [ ] Get final approval dari:
  - [ ] Product owner
  - [ ] Technical lead
  - [ ] Operations lead
  - [ ] Legal counsel
  - [ ] Business owner
- [ ] Document approvals
- [ ] Schedule launch time
- [ ] Communicate launch plan ke seluruh tim

## Output Fase 9
- ✅ Monitoring komprehensif (APM, infrastructure, business metrics)
- ✅ Alerting aktif dengan runbooks
- ✅ Logging dan audit trail lengkap
- ✅ Legal documents published (ToS, Privacy Policy, Disclaimer)
- ✅ User support channels ready
- ✅ Feedback collection system active
- ✅ Launch readiness checklist completed
- ✅ Go-live procedure documented
- ✅ Post-launch monitoring plan ready
- ✅ Continuous improvement process established

## Definition of Done
Produk siap launch ke publik dengan monitoring komprehensif, legal compliance, user support ready, dan prosedur go-live yang terstruktur. Tim dapat merespon insiden dalam <5 menit, dan mekanisme feedback loop aktif untuk continuous improvement.

---

# RANGKUMAN IMPLEMENTASI GUIDE

## Total Ceklist: 9 Fase

| Fase | Fokus Utama | Jumlah Ceklist |
|------|-------------|----------------|
| **Fase 1** | Setup Infrastruktur & Environment | ~50 items |
| **Fase 2** | Database & Data Layer | ~80 items |
| **Fase 3** | Backend API Core | ~100 items |
| **Fase 4** | Agent Orchestrator (LangGraph) | ~90 items |
| **Fase 5** | LLM Integration & RAG | ~80 items |
| **Fase 6** | Frontend Development | ~90 items |
| **Fase 7** | Integration & Testing | ~70 items |
| **Fase 8** | Deployment & Security | ~70 items |
| **Fase 9** | Monitoring & Launch Readiness | ~100 items |
| **TOTAL** | | **~730 items** |

## Aturan Emas per Fase

| Fase | Aturan Emas Utama |
|------|-------------------|
| **Fase 1** | Infrastructure as Code, Environment Parity, Zero Trust Network |
| **Fase 2** | Thread Isolation is Sacred, Schema First, Encryption at Rest |
| **Fase 3** | API First Design, Security by Default, Rate Limiting Mandatory |
| **Fase 4** | State Machine is Law, State Persistence, Deterministic Transitions |
| **Fase 5** | No Hallucination Policy, Citation Enforcement, Cost Control |
| **Fase 6** | Chat-First Experience, Real-Time Feedback, Thread Isolation UI |
| **Fase 7** | E2E Testing Mandatory, Thread Isolation Proven, No Hallucination |
| **Fase 8** | Zero Downtime Deployment, Secrets Management, HTTPS Only |
| **Fase 9** | Observability Non-Negotiable, Runbook for Every Alert, Launch is Process |

## Kriteria Produk Siap Launch

PAUGERAN dinyatakan **SIAP LAUNCH** apabila:

### ✅ Kriteria Teknis
- [ ] Semua 730+ checklist items completed
- [ ] Semua tests passing (unit, integration, E2E)
- [ ] Performance targets met (100 concurrent users, <2s response time)
- [ ] Security audit passed (OWASP Top 10)
- [ ] Thread isolation verified dengan penetration testing
- [ ] Hallucination rate 0% dalam 150+ test cases
- [ ] Uptime >99.5% dalam staging

### ✅ Kriteria Operasional
- [ ] Monitoring active dengan alerts
- [ ] Runbooks documented untuk semua alerts
- [ ] Backup & restore tested
- [ ] Rollback plan tested
- [ ] Team trained on operations
- [ ] Incident response plan ready

### ✅ Kriteria Bisnis
- [ ] Legal documents published (ToS, Privacy Policy, Disclaimer)
- [ ] User support channels ready
- [ ] Feedback collection system active
- [ ] Pricing model finalized
- [ ] Marketing materials ready

### ✅ Kriteria Kepatuhan PRD
- [ ] Siklus 11 fase berjalan sempurna
- [ ] Peta keterlacakan lengkap untuk setiap kesimpulan
- [ ] Kontraargumentasi selalu ditampilkan
- [ ] Ketidakpastian dinyatakan eksplisit
- [ ] Bahasa sesuai standar profesional
- [ ] 5 pertanyaan keberhasilan dapat dijawab dari output

## Penutup

Implementasi Guide ini adalah **peta jalan komprehensif** untuk membangun PAUGERAN dari nol sampai siap launch. Dengan mengikuti 9 fase ini secara berurutan dan menyelesaikan semua checklist, tim pengembangan dapat memastikan bahwa:

1. **Produk dibangun dengan fondasi yang kuat** — infrastruktur sebagai kode, database yang robust, API yang secure
2. **Kualitas terjamin** — testing komprehensif, security audit, performance testing
3. **Operasional siap** — monitoring, alerting, backup, rollback
4. **Legal compliant** — ToS, Privacy Policy, UU PDP compliance
5. **User-ready** — support channels, feedback loop, documentation

**PAUGERAN bukan sekadar AI chatbot.** PAUGERAN adalah **agen penalaran hukum yang dapat dipertanggungjawabkan**, dengan:
- Pemahaman adaptif sebelum penalaran
- Penelitian hukum berbasis sumber terverifikasi
- Argumentasi berimbang dengan kontraargumentasi
- Keterlacakan penuh dari kesimpulan ke fakta dan sumber
- Privasi data dengan isolasi thread yang ketat
- Transparansi ketidakpastian dan risiko

Dengan Implementasi Guide ini, tim memiliki **blueprint lengkap** untuk mewujudkan visi tersebut menjadi produk yang siap digunakan oleh advokat, legal in-house, dan profesional hukum di Indonesia.

---

**Dokumen Implementasi Guide — Selesai**

**Versi:** 1.0  
**Referensi:** PRD Contract Baseline v1.0  
**Status:** Siap Dieksekusi  
**Total Fase:** 9  
**Total Checklist Items:** ~730  

**Selamat membangun PAUGERAN!** 🚀
