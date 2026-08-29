# Dokumen Turunan PAUGERAN CONTRACT BASELINE

Berdasarkan PAUGERAN Contract Baseline (CB), berikut adalah daftar lengkap dokumen turunan yang harus dibuat. Dokumen-dokumen ini diturunkan dari CB dan **wajib konsisten** dengan CB. Jika terjadi pertentangan, CB yang berlaku.

---

## Kategori 1: Dokumen Arsitektur & Teknis Inti

### 1.1. **SPEC-ARCH — Spesifikasi Arsitektur Teknis**
- **Referensi CB:** [CB §6], [CB §7]
- **Format:** Markdown + diagram (Mermaid/PlantUML)
- **Tujuan:** Menjelaskan arsitektur sistem secara detail untuk developer
- **Isi Utama:**
  - Diagram arsitektur keseluruhan (C4 Model: Context, Container, Component)
  - Diagram alur data untuk setiap fitur utama
  - Spesifikasi lapisan (HTTP Server, Graph Engine, LLM Router, Web Researcher, Knowledge Base, Data Layer)
  - Diagram sequence untuk alur kritis (chat streaming, license validation, web research)
  - Keputusan arsitektur (ADRs — Architecture Decision Records)
  - Trade-off analysis untuk setiap pilihan teknologi
- **Target:** Developer backend & frontend
- **Prioritas:** 🔴 **WAJIB** (dibuat pertama)

### 1.2. **SPEC-API — OpenAPI Specification**
- **Referensi CB:** [CB §13]
- **Format:** YAML (OpenAPI 3.1)
- **Tujuan:** Kontrak API yang dapat di-generate dan divalidasi otomatis
- **Isi Utama:**
  - Semua endpoint dengan request/response schema
  - Authentication schemes (JWT, conditional auth)
  - Error codes dan format
  - Rate limiting policies
  - Contoh request/response untuk setiap endpoint
  - WebSocket/SSE event definitions
- **Target:** Developer frontend & backend, QA, integrator eksternal
- **Prioritas:** 🔴 **WAJIB**

### 1.3. **SPEC-DB — Spesifikasi Database & Migrasi**
- **Referensi CB:** [CB §10]
- **Format:** SQL migration files + Markdown
- **Tujuan:** Skema database yang dapat di-migrate secara deterministik
- **Isi Utama:**
  - Migration files berurutan (0001_initial.sql, 0002_add_knowledge_base.sql, dst)
  - Entity Relationship Diagram (ERD)
  - Index strategy dan rationale
  - Query performance notes
  - Data retention policies
  - Backup & restore procedures untuk database
  - Rollback procedures untuk setiap migration
- **Target:** Developer backend, DBA
- **Prioritas:** 🔴 **WAJIB**

### 1.4. **SPEC-REPO — Spesifikasi Struktur Repositori**
- **Referensi CB:** [CB §37]
- **Format:** Markdown + tree structure
- **Tujuan:** Panduan struktur kode untuk konsistensi tim
- **Isi Utama:**
  - Struktur direktori lengkap dengan penjelasan
  - Konvensi penamaan file dan folder
  - Konvensi penamaan fungsi, struct, dan module di Rust
  - Konvensi penamaan komponen di SolidJS
  - Cargo workspace configuration
  - pnpm workspace configuration
  - Code ownership (CODEOWNERS file)
- **Target:** Semua developer
- **Prioritas:** 🔴 **WAJIB**

---

## Kategori 2: Dokumen Spesifikasi Fitur

### 2.1. **SPEC-GRAPH — Spesifikasi Custom Graph Engine**
- **Referensi CB:** [CB §8], [CB §14]
- **Format:** Markdown + diagram state machine
- **Tujuan:** Detail implementasi state machine 11 fase
- **Isi Utama:**
  - State diagram lengkap dengan semua transisi
  - Spesifikasi setiap node (input, proses, output, durasi target)
  - Spesifikasi conditional edges
  - Error handling dan recovery strategy
  - Event streaming protocol
  - Cancellation mechanism
  - State persistence format
  - Test scenarios untuk setiap node
- **Target:** Developer backend
- **Prioritas:** 🔴 **WAJIB**

### 2.2. **SPEC-LLM — Spesifikasi Multi-Provider LLM**
- **Referensi CB:** [CB §9]
- **Format:** Markdown + interface definitions
- **Tujuan:** Detail implementasi router LLM multi-provider
- **Isi Utama:**
  - Trait `LlmProvider` dan implementasi untuk setiap provider
  - Adapter untuk: Anthropic, OpenAI, OpenAI-compatible, Ollama
  - Model routing strategy (task → provider → model)
  - Fallback mechanism
  - Cost tracking dan budget limits
  - Token usage monitoring
  - Rate limiting per provider
  - Testing strategy untuk setiap provider
  - Cara menambah provider baru (extension guide)
- **Target:** Developer backend
- **Prioritas:** 🔴 **WAJIB**

### 2.3. **SPEC-KB — Spesifikasi Legal Knowledge Base**
- **Referensi CB:** [CB §11]
- **Format:** Markdown + algoritma
- **Tujuan:** Detail implementasi basis pengetahuan hukum
- **Isi Utama:**
  - Skema penyimpanan (metadata, konten, pasal, embeddings)
  - Algoritma embedding generation
  - Algoritma pencarian (semantic, keyword, hybrid)
  - Strategi chunking dokumen hukum
  - Metadata extraction dari berbagai format (UU, PP, putusan)
  - Update dan versioning strategy
  - Conflict resolution (peraturan bertentangan)
  - Bulk import format dan procedures
  - Performance benchmarks
- **Target:** Developer backend
- **Prioritas:** 🔴 **WAJIB**

### 2.4. **SPEC-LICENSE — Spesifikasi Sistem Lisensi**
- **Referensi CB:** [CB §12]
- **Format:** Markdown + protocol definitions
- **Tujuan:** Detail implementasi sistem lisensi
- **Isi Utama:**
  - Protokol validasi (request/response format)
  - Algoritma installation_id generation
  - Grace period logic
  - Lisensi offline format dan validasi
  - Cryptographic signing mechanism
  - Server-side license management (jika ada)
  - Feature gating berdasarkan tipe lisensi
  - Migration strategy untuk lisensi
  - Privacy considerations (data yang dikirim ke server lisensi)
- **Target:** Developer backend, security team
- **Prioritas:** 🔴 **WAJIB**

### 2.5. **SPEC-WEB — Spesifikasi Penelitian Web**
- **Referensi CB:** [CB §17]
- **Format:** Markdown + whitelist config
- **Tujuan:** Detail implementasi web research module
- **Isi Utama:**
  - Daftar whitelist domain lengkap dengan justifikasi
  - HTTP client configuration (timeout, retry, user-agent)
  - HTML parsing dan content extraction strategy
  - Robots.txt compliance
  - Rate limiting per domain
  - Error handling untuk berbagai skenario
  - Metadata extraction dari halaman hukum
  - Integrasi dengan Knowledge Base
  - Legal considerations (copyright, fair use)
  - Cara menambah domain ke whitelist
- **Target:** Developer backend, legal advisor
- **Prioritas:** 🟡 **PENTING**

### 2.6. **SPEC-CITATION — Spesifikasi Inline Citation**
- **Referensi CB:** [CB §18], [CB §20]
- **Format:** Markdown + format definitions
- **Tujuan:** Standarisasi format dan visualisasi citation
- **Isi Utama:**
  - Format citation untuk berbagai jenis sumber (UU, PP, putusan, web, dokumen)
  - Markdown syntax untuk inline citation
  - Frontend rendering rules
  - Tooltip dan panel detail specification
  - Konsistensi citation dalam satu dokumen
  - Export format untuk citation (PDF, DOCX)
  - Validation rules untuk citation
- **Target:** Developer frontend & backend
- **Prioritas:** 🟡 **PENTING**

### 2.7. **SPEC-EXPORT — Spesifikasi Export Dokumen Profesional**
- **Referensi CB:** [CB §19]
- **Format:** Markdown + template files
- **Tujuan:** Detail implementasi export PDF/DOCX
- **Isi Utama:**
  - Template PDF (layout, tipografi, margin, header/footer)
  - Template DOCX (styles, structure)
  - Template variants (Standar, Formal, Memorandum, Opinion Letter)
  - Metadata embedding (PDF properties, DOCX properties)
  - Bookmark dan hyperlink generation
  - Image handling dan compression
  - Font embedding strategy
  - Performance considerations untuk dokumen besar
  - Custom template upload mechanism
- **Target:** Developer backend, designer
- **Prioritas:** 🟡 **PENTING**

### 2.8. **SPEC-A11Y — Spesifikasi Aksesibilitas**
- **Referensi CB:** [CB §28]
- **Format:** Markdown + WCAG checklist
- **Tujuan:** Panduan implementasi aksesibilitas
- **Isi Utama:**
  - WCAG 2.1 AA compliance checklist
  - Keyboard navigation map
  - ARIA labels dan roles untuk setiap komponen
  - Screen reader testing scenarios
  - Color contrast requirements
  - Focus management strategy
  - Mode aksesibilitas (High Contrast, Reduced Motion, Large Text)
  - Testing tools dan procedures
  - Common accessibility pitfalls dan solusinya
- **Target:** Developer frontend, QA, accessibility specialist
- **Prioritas:** 🟡 **PENTING**

### 2.9. **SPEC-AUTH — Spesifikasi Autentikasi & Otorisasi**
- **Referensi CB:** [CB §25]
- **Format:** Markdown + flow diagrams
- **Tujuan:** Detail implementasi sistem auth
- **Isi Utama:**
  - JWT token structure dan lifecycle
  - Password hashing strategy (Argon2 parameters)
  - Role-based access control matrix
  - Invitation system flow
  - Admin privileges dan batasan
  - Session management
  - Security considerations
  - Testing scenarios untuk auth bypass
- **Target:** Developer backend, security team
- **Prioritas:** 🟡 **PENTING**

### 2.10. **SPEC-UI — Spesifikasi Antarmuka Pengguna**
- **Referensi CB:** [CB §27], [CB §29]
- **Format:** Markdown + wireframes + design tokens
- **Tujuan:** Panduan desain UI yang konsisten
- **Isi Utama:**
  - Design tokens (colors, typography, spacing, shadows)
  - Component library specification
  - Layout rules untuk desktop, tablet, mobile
  - Responsive breakpoints
  - State visualizations (loading, error, empty, success)
  - Animation guidelines
  - Icon library dan usage
  - Dark mode dan theme variants
- **Target:** Designer, developer frontend
- **Prioritas:** 🟡 **PENTING**

---

## Kategori 3: Dokumen Pengujian & Kualitas

### 3.1. **TEST-PLAN — Rencana Pengujian Keseluruhan**
- **Referensi CB:** [CB §35], [CB §36]
- **Format:** Markdown + test matrix
- **Tujuan:** Strategi pengujian end-to-end
- **Isi Utama:**
  - Testing pyramid (unit, integration, E2E, performance)
  - Test coverage targets
  - Test environment setup
  - Test data strategy
  - Regression testing strategy
  - Acceptance testing criteria
  - Release testing checklist
- **Target:** QA team, developer
- **Prioritas:** 🔴 **WAJIB**

### 3.2. **TEST-CASES — Kumpulan Test Cases**
- **Referensi CB:** [CB §34], [CB §36]
- **Format:** Markdown atau test management tool
- **Tujuan:** Test cases terstruktur untuk setiap fitur
- **Isi Utama:**
  - Test cases untuk setiap endpoint API
  - Test cases untuk setiap fase agen (11 fase)
  - Test cases untuk isolasi sesi
  - Test cases untuk lisensi (valid, expired, grace period, offline)
  - Test cases untuk multi-provider LLM
  - Test cases untuk Knowledge Base
  - Test cases untuk web research
  - Test cases untuk export
  - Test cases untuk aksesibilitas
  - Negative test cases (error scenarios)
  - Security test cases (OWASP Top 10)
- **Target:** QA team
- **Prioritas:** 🔴 **WAJIB**

### 3.3. **TEST-HALLUCINATION — Protokol Uji Anti-Halusinasi**
- **Referensi CB:** [CB §34.1-34.3], [CB §36.3.1]
- **Format:** Markdown + dataset
- **Tujuan:** Memastikan PAUGERAN tidak berhalusinasi
- **Isi Utama:**
  - Dataset 100+ kasus hukum untuk pengujian
  - Kriteria penilaian (pasal valid, putusan valid, sumber valid)
  - Prosedur pengujian
  - Threshold acceptance (0% halusinasi)
  - Root cause analysis template
  - Regression test untuk setiap bug halusinasi yang ditemukan
- **Target:** QA team, legal advisor
- **Prioritas:** 🔴 **WAJIB**

### 3.4. **TEST-PERFORMANCE — Protokol Uji Performa**
- **Referensi CB:** [CB §35.3], [CB §36.2]
- **Format:** Markdown + benchmark scripts
- **Tujuan:** Memastikan performa sesuai target
- **Isi Utama:**
  - Load testing scenarios (100 concurrent users)
  - Stress testing scenarios
  - Endurance testing (24+ jam)
  - Performance benchmarks:
    - Response time per endpoint
    - LLM API latency
    - Knowledge Base search time
    - Export generation time
    - Startup time
    - Memory usage
  - Performance regression detection
  - Profiling tools dan procedures
- **Target:** QA team, DevOps
- **Prioritas:** 🟡 **PENTING**

### 3.5. **TEST-SECURITY — Protokol Uji Keamanan**
- **Referensi CB:** [CB §30], [CB §36.6]
- **Format:** Markdown + security checklist
- **Tujuan:** Memastikan keamanan sistem
- **Isi Utama:**
  - OWASP Top 10 testing procedures
  - Penetration testing scenarios
  - API key isolation testing
  - License validation bypass attempts
  - Session isolation testing
  - SQL injection testing
  - XSS testing
  - CSRF testing
  - File upload security testing
  - Web research whitelist bypass testing
  - Security audit checklist
- **Target:** Security team, QA
- **Prioritas:** 🔴 **WAJIB**

---

## Kategori 4: Dokumen Deployment & Operasional

### 4.1. **DEPLOY-GUIDE — Panduan Deployment**
- **Referensi CB:** [CB §23], [CB §31]
- **Format:** Markdown + scripts
- **Tujuan:** Panduan deployment untuk setiap skenario
- **Isi Utama:**
  - Deployment ke laptop pribadi (binary standalone)
  - Deployment ke Docker lokal
  - Deployment ke Railway (one-click)
  - Deployment ke VPS dengan Caddy/Nginx
  - Deployment ke homelab (Tailscale, Cloudflare Tunnel)
  - Deployment Tauri desktop
  - Deployment air-gapped dengan lisensi offline
  - Troubleshooting untuk setiap skenario
  - Verification checklists
- **Target:** DevOps, sysadmin, pengguna advanced
- **Prioritas:** 🔴 **WAJIB**

### 4.2. **OPS-RUNBOOK — Runbook Operasional**
- **Referensi CB:** [CB §32], [CB §33]
- **Format:** Markdown + decision trees
- **Tujuan:** Panduan operasional harian dan insiden
- **Isi Utama:**
  - Monitoring dashboard setup
  - Alert definitions dan escalation paths
  - Common incidents dan prosedur penanganan:
    - Server down
    - Database corruption
    - LLM API failure
    - License server unreachable
    - Disk space full
    - Memory leak
    - High CPU usage
  - Log analysis procedures
  - Performance tuning guide
  - Capacity planning
- **Target:** DevOps, sysadmin
- **Prioritas:** 🟡 **PENTING**

### 4.3. **OPS-BACKUP — Prosedur Backup & Recovery**
- **Referensi CB:** [CB §33]
- **Format:** Markdown + scripts
- **Tujuan:** Prosedur backup dan recovery yang teruji
- **Isi Utama:**
  - Backup strategy (full, incremental)
  - Backup schedule
  - Backup verification procedures
  - Recovery procedures untuk berbagai skenario
  - Disaster recovery plan
  - Migration procedures antar versi
  - Data retention policies
  - Backup storage recommendations
  - RTO (Recovery Time Objective) dan RPO (Recovery Point Objective)
- **Target:** DevOps, sysadmin
- **Prioritas:** 🔴 **WAJIB**

### 4.4. **OPS-CICD — Pipeline CI/CD**
- **Referensi CB:** [CB §37]
- **Format:** YAML (GitHub Actions) + Markdown
- **Tujuan:** Otomatisasi build, test, dan deployment
- **Isi Utama:**
  - Pipeline definition (GitHub Actions workflows)
  - Build stages (lint, test, build, package)
  - Test stages (unit, integration, E2E)
  - Release automation
  - Binary distribution (GitHub Releases)
  - Docker image build dan push
  - Tauri installer build
  - Code signing procedures
  - Version bumping strategy
- **Target:** DevOps, developer
- **Prioritas:** 🟡 **PENTING**

---

## Kategori 5: Dokumen Pengguna & Desain

### 5.1. **USER-GUIDE — Panduan Pengguna**
- **Referensi CB:** [CB §29], [CB §28], [CB §27]
- **Format:** Markdown atau documentation site
- **Tujuan:** Panduan lengkap untuk pengguna akhir
- **Isi Utama:**
  - Quick start guide (5 menit pertama)
  - Instalasi untuk setiap platform
  - Aktivasi lisensi
  - Konfigurasi LLM provider
  - Membuat sesi obrolan pertama
  - Menggunakan fitur-fitur utama
  - Mengelola Knowledge Base
  - Export laporan
  - Kustomisasi UI
  - Keyboard shortcuts reference
  - Command palette guide
  - FAQ
  - Troubleshooting umum
- **Target:** Pengguna akhir
- **Prioritas:** 🔴 **WAJIB**

### 5.2. **ADMIN-GUIDE — Panduan Administrator**
- **Referensi CB:** [CB §25], [CB §26]
- **Format:** Markdown
- **Tujuan:** Panduan untuk admin tim
- **Isi Utama:**
  - Setup awal sebagai admin
  - Mengelola anggota tim
  - Mengundang pengguna baru
  - Mengonfigurasi LLM providers global
  - Mengelola Legal Knowledge Base global
  - Mengonfigurasi whitelist domain
  - Monitoring penggunaan tim
  - Managing lisensi tim
  - Backup dan restore
  - Troubleshooting untuk admin
- **Target:** Administrator tim
- **Prioritas:** 🟡 **PENTING** (jika AUTH_ENABLED=true)

### 5.3. **DESIGN-SYSTEM — Design System**
- **Referensi CB:** [CB §27], [CB §29]
- **Format:** Storybook + Markdown
- **Tujuan:** Sistem desain yang konsisten
- **Isi Utama:**
  - Color palette (light, dark, sepia, high contrast)
  - Typography scale
  - Spacing system
  - Component library (Button, Input, Modal, dll)
  - Icon library
  - Layout patterns
  - Motion & animation guidelines
  - Accessibility guidelines
  - Brand guidelines (logo, warna, tipografi)
- **Target:** Designer, developer frontend
- **Prioritas:** 🟡 **PENTING**

### 5.4. **COPY-STANDARDS — Standar Penulisan Konten**
- **Referensi CB:** [CB §21], [CB §22]
- **Format:** Markdown
- **Tujuan:** Standarisasi bahasa dan nada komunikasi
- **Isi Utama:**
  - Tone of voice
  - Bahasa hukum baku
  - Istilah yang harus digunakan dan dihindari
  - Format penulisan peraturan dan putusan
  - Format tanggal, angka, mata uang
  - Error message guidelines
  - Empty state messages
  - Success messages
  - Localization guidelines (ID/EN)
- **Target:** Content writer, developer, designer
- **Prioritas:** 🟢 **PENDUKUNG**

---

## Kategori 6: Dokumen Bisnis & Legal

### 6.1. **LEGAL-TOS — Terms of Service**
- **Referensi CB:** [CB §1], [CB §4]
- **Format:** Dokumen hukum
- **Tujuan:** Ketentuan layanan untuk pengguna
- **Isi Utama:**
  - Acceptance of terms
  - Description of service
  - User responsibilities
  - API key dan lisensi responsibilities
  - Data privacy
  - Limitation of liability
  - Disclaimer (PAUGERAN bukan pengganti advokat)
  - Termination
  - Governing law (Indonesia)
- **Target:** Pengguna akhir
- **Prioritas:** 🔴 **WAJIB** (sebelum launch publik)

### 6.2. **LEGAL-PRIVACY — Privacy Policy**
- **Referensi CB:** [CB §4.11], [CB §4.16], [CB §4.24]
- **Format:** Dokumen hukum
- **Tujuan:** Kebijakan privasi sesuai UU PDP
- **Isi Utama:**
  - Data yang dikumpulkan
  - Tujuan pengumpulan data
  - Data yang TIDAK dikumpulkan (API key, konten sesi)
  - Data yang dikirim ke pihak ketiga (hanya ke LLM provider dan license server)
  - User rights (akses, koreksi, hapus)
  - Data retention
  - Security measures
  - Contact information
- **Target:** Pengguna akhir
- **Prioritas:** 🔴 **WAJIB** (sebelum launch publik)

### 6.3. **LEGAL-LICENSE-AGREEMENT — Perjanjian Lisensi**
- **Referensi CB:** [CB §12]
- **Format:** Dokumen hukum
- **Tujuan:** Perjanjian lisensi software
- **Isi Utama:**
  - Tipe lisensi (Trial, Personal, Team, Enterprise)
  - Hak dan kewajiban licensee
  - Restrictions
  - Warranty disclaimer
  - Termination
  - Refund policy
- **Target:** Pembeli lisensi
- **Prioritas:** 🔴 **WAJIB** (jika ada model bisnis lisensi)

### 6.4. **BUSINESS-PRICING — Strategi Harga**
- **Referensi CB:** [CB §12.4]
- **Format:** Internal document
- **Tujuan:** Strategi pricing untuk lisensi
- **Isi Utama:**
  - Pricing tiers
  - Feature gating per tier
  - Discount strategies
  - Payment methods
  - Revenue projections
- **Target:** Management, sales
- **Prioritas:** 🟢 **PENDUKUNG** (internal)

---

## Kategori 7: Dokumen AI Agent

### 7.1. **agen.md — Kontrak Perilaku AI Agen Pengembangan**
- **Referensi CB:** Seluruh dokumen CB
- **Format:** Markdown
- **Tujuan:** Mengatur perilaku AI agent yang membantu pengembangan PAUGERAN
- **Isi Utama:**
  - Identitas dan misi agen
  - Hierarki kebenaran (CB > agen.md > dokumen turunan)
  - Aturan emas perilaku
  - Protokol implementasi
  - Protokol update ceklist
  - Larangan keras
  - Protokol komunikasi
  - Quality gates
  - Template update
  - Protokol escalation
- **Target:** AI agent (Claude, GPT, dll)
- **Prioritas:** 🔴 **WAJIB** (untuk development)

### 7.2. **IMPLEMENTASI-STATUS.md — Status Implementasi**
- **Referensi CB:** Seluruh dokumen CB
- **Format:** Markdown (living document)
- **Tujuan:** Tracking progress implementasi
- **Isi Utama:**
  - Status per fase
  - Status per checklist item
  - Update log
  - Blockers
  - Next actions
- **Target:** Developer, project manager, AI agent
- **Prioritas:** 🔴 **WAJIB** (living document)

---

## Ringkasan Prioritas

### 🔴 **WAJIB** (Harus ada sebelum development dimulai)
1. SPEC-ARCH — Arsitektur Teknis
2. SPEC-API — OpenAPI Specification
3. SPEC-DB — Database & Migrasi
4. SPEC-REPO — Struktur Repositori
5. SPEC-GRAPH — Custom Graph Engine
6. SPEC-LLM — Multi-Provider LLM
7. SPEC-KB — Legal Knowledge Base
8. SPEC-LICENSE — Sistem Lisensi
9. TEST-PLAN — Rencana Pengujian
10. TEST-CASES — Test Cases
11. TEST-HALLUCINATION — Uji Anti-Halusinasi
12. DEPLOY-GUIDE — Panduan Deployment
13. OPS-BACKUP — Backup & Recovery
14. USER-GUIDE — Panduan Pengguna
15. agen.md — Kontrak AI Agent
16. IMPLEMENTASI-STATUS.md — Status Implementasi
17. LEGAL-TOS — Terms of Service
18. LEGAL-PRIVACY — Privacy Policy

### 🟡 **PENTING** (Harus ada sebelum launch)
1. SPEC-WEB — Penelitian Web
2. SPEC-CITATION — Inline Citation
3. SPEC-EXPORT — Export Dokumen
4. SPEC-A11Y — Aksesibilitas
5. SPEC-AUTH — Autentikasi
6. SPEC-UI — Antarmuka Pengguna
7. TEST-PERFORMANCE — Uji Performa
8. TEST-SECURITY — Uji Keamanan
9. OPS-RUNBOOK — Runbook Operasional
10. OPS-CICD — Pipeline CI/CD
11. ADMIN-GUIDE — Panduan Admin
12. DESIGN-SYSTEM — Design System
13. LEGAL-LICENSE-AGREEMENT — Perjanjian Lisensi

### 🟢 **PENDUKUNG** (Opsional, bisa ditambahkan nanti)
1. COPY-STANDARDS — Standar Penulisan
2. BUSINESS-PRICING — Strategi Harga

---

## Rekomendasi Urutan Pembuatan

**Fase 1 — Fondasi (sebelum coding):**
1. SPEC-REPO
2. SPEC-ARCH
3. SPEC-DB
4. SPEC-API
5. agen.md
6. IMPLEMENTASI-STATUS.md

**Fase 2 — Fitur Inti (saat development):**
7. SPEC-GRAPH
8. SPEC-LLM
9. SPEC-KB
10. SPEC-LICENSE
11. SPEC-AUTH
12. SPEC-UI
13. DESIGN-SYSTEM

**Fase 3 — Fitur Lanjutan (saat development):**
14. SPEC-WEB
15. SPEC-CITATION
16. SPEC-EXPORT
17. SPEC-A11Y

**Fase 4 — Pengujian (saat development):**
18. TEST-PLAN
19. TEST-CASES
20. TEST-HALLUCINATION
21. TEST-PERFORMANCE
22. TEST-SECURITY

**Fase 5 — Deployment (sebelum launch):**
23. DEPLOY-GUIDE
24. OPS-BACKUP
25. OPS-RUNBOOK
26. OPS-CICD

**Fase 6 — Dokumentasi Pengguna (sebelum launch):**
27. USER-GUIDE
28. ADMIN-GUIDE
29. LEGAL-TOS
30. LEGAL-PRIVACY
31. LEGAL-LICENSE-AGREEMENT

**Fase 7 — Pendukung (setelah launch):**
32. COPY-STANDARDS
33. BUSINESS-PRICING

---

## Catatan Penting

1. **Setiap dokumen turunan harus mencantumkan referensi ke pasal CB** yang menjadi dasarnya menggunakan format `[CB §X.Y]`.

2. **Dokumen turunan tidak boleh bertentangan dengan CB.** Jika ditemukan pertentangan, CB yang berlaku dan dokumen turunan harus direvisi.

3. **Dokumen turunan harus dipelihara.** Setiap perubahan pada CB harus diikuti dengan update dokumen turunan yang relevan.

4. **Versi dokumen turunan harus sinkron dengan versi CB.** Gunakan skema versi yang konsisten (misalnya: `SPEC-ARCH v1.0` mengikuti `CB v1.0`).

5. **Dokumen turunan harus self-contained.** Pembaca tidak harus membuka CB untuk memahami dokumen turunan, tetapi referensi ke CB harus tetap dicantumkan untuk traceability.

6. **Format dokumen harus konsisten.** Gunakan Markdown untuk semua dokumen kecuali yang memang memerlukan format khusus (OpenAPI YAML, SQL, dll).

7. **Setiap dokumen harus memiliki metadata:**
   ```markdown
   ---
   title: [Nama Dokumen]
   document_id: [SPEC-ARCH, SPEC-API, dll]
   version: 1.0
   cb_reference: [CB §X.Y]
   status: DRAFT | FINAL
   owner: [Tim/individu yang bertanggung jawab]
   last_updated: [YYYY-MM-DD]
   ---
   ```

Dengan daftar dokumen turunan ini, tim pengembangan PAUGERAN memiliki peta lengkap untuk membangun produk secara terstruktur, terukur, dan konsisten dengan Contract Baseline.
