# PRD — CONTRACT BASELINE PAUGERAN

**Nama Produk:** PAUGERAN
**Jenis Dokumen:** Product Requirements Document — Contract Baseline
**Status:** PRODUK READY (Siap Dikembangkan)
**Versi:** 1.0
**Tanggal Efektif:** 26 Agustus 2026
**Sifat:** Kontrak perilaku, kemampuan, batasan, arsitektur, dan keluaran produk yang mengikat
**Bukan:** Roadmap, rencana sprint, backlog, fase implementasi, atau dokumen evolusi produk

---

## DAFTAR ISI

1. Definisi Produk
2. Tujuan Produk
3. Masalah yang Diselesaikan
4. Prinsip Produk
5. Aktor
6. Arsitektur Produk
7. Spesifikasi Antarmuka
8. Spesifikasi Chat Thread
9. Spesifikasi Agen
10. Spesifikasi Data
11. Spesifikasi API
12. Spesifikasi Keamanan
13. Spesifikasi Deployment
14. Siklus Agen
15. Mode Pemahaman
16. Mode Penalaran
17. Spesifikasi Output
18. Standar Keterlacakan
19. Standar Bahasa
20. Larangan Produk
21. Kontrak Perilaku
22. Kriteria Keberhasilan
23. Kriteria Penerimaan
24. Spesifikasi Repositori
25. Monitoring & Observabilitas
26. Backup & Recovery
27. Penutup

---

## 1. DEFINISI PRODUK

PAUGERAN adalah agen kecerdasan buatan untuk pemahaman masalah, penelitian, dan penalaran hukum Indonesia yang dioperasikan sebagai layanan web dengan antarmuka chat-first dan backend yang dikelola sendiri.

PAUGERAN menerima uraian masalah dari pengguna melalui antarmuka chat, kemudian secara bertahap membangun pemahaman terhadap persoalan tersebut melalui dialog adaptif dalam satu thread obrolan yang terisolasi, mengidentifikasi fakta dan kekurangan informasi, merumuskan masalah hukum, melakukan penelitian terhadap sumber hukum yang relevan, menguji berbagai argumentasi dan penafsiran, kemudian menghasilkan analisis hukum yang memiliki keterlacakan penuh antara kesimpulan, fakta, kaidah hukum, sumber hukum, dan penalarannya.

PAUGERAN bukan sekadar mesin pencari pasal dan bukan mesin pemberi jawaban "benar/salah".

Produk harus mampu menjelaskan:
> apa kesimpulannya, mengapa kesimpulan tersebut muncul, dasar apa yang digunakan, fakta apa yang mendukungnya, apa kelemahannya, dan apa yang dapat membuat kesimpulan tersebut berubah.

PAUGERAN dioperasikan sebagai produk siap pakai (*product-ready*) dengan arsitektur monorepo terpadu, antarmuka web modern yang di-hosting di Vercel, backend yang berjalan di VPS yang dikelola sendiri, dan setiap obrolan (*chat thread*) merupakan entitas terisolasi yang menjamin privasi dan keamanan data kasus pengguna.

---

## 2. TUJUAN PRODUK

PAUGERAN harus memungkinkan pengguna untuk:

1. menjelaskan masalah menggunakan bahasa natural dalam antarmuka chat;
2. mendapatkan pertanyaan klarifikasi yang relevan dan adaptif;
3. membangun pemahaman masalah secara bertahap dalam satu thread obrolan;
4. mengoreksi pemahaman PAUGERAN melalui dialog;
5. menentukan kapan proses pemahaman dianggap cukup;
6. meminta PAUGERAN melakukan penalaran hukum;
7. memperoleh penelitian hukum dari sumber yang relevan;
8. mengetahui dasar hukum yang digunakan;
9. memahami penerapan hukum terhadap fakta;
10. melihat argumentasi yang mendukung dan berlawanan;
11. mengetahui ketidakpastian dan kelemahan analisis;
12. melihat alternatif penafsiran;
13. menelusuri setiap kesimpulan menuju dasar dan sumbernya melalui peta keterlacakan;
14. memperoleh laporan hukum dalam Bahasa Indonesia yang profesional dan mudah dipahami;
15. mengakses produk melalui peramban web tanpa instalasi;
16. menyimpan seluruh data kasus secara privat dalam thread yang terisolasi;
17. mengekspor hasil analisis dalam format PDF dan DOCX;
18. mengelola banyak thread obrolan untuk kasus yang berbeda;
19. melihat riwayat obrolan dan melanjutkan pekerjaan yang tertunda;
20. mengunggah dokumen pendukung (PDF, DOCX, TXT) ke dalam thread obrolan.

---

## 3. MASALAH YANG DISELESAIKAN

Sistem hukum memiliki karakteristik yang membuat jawaban langsung sering tidak memadai:

- fakta pengguna sering tidak lengkap;
- istilah yang digunakan pengguna belum tentu merupakan istilah hukum yang tepat;
- satu fakta dapat memiliki beberapa konsekuensi hukum;
- satu masalah dapat melibatkan beberapa bidang hukum;
- aturan dapat berubah;
- aturan dapat memiliki pengecualian;
- terdapat kemungkinan konflik norma;
- putusan pengadilan dapat memiliki fakta yang berbeda;
- suatu norma dapat memiliki beberapa interpretasi;
- kekuatan suatu kesimpulan bergantung pada fakta dan bukti;
- informasi hukum di internet memiliki tingkat keandalan yang berbeda;
- data hukum bersifat sensitif dan harus dijaga kerahasiaannya;
- advokat membutuhkan alat yang dapat dipertanggungjawabkan secara profesional;
- pengguna membutuhkan isolasi data antar kasus yang berbeda.

Karena itu, PAUGERAN harus memahami sebelum menalar, menalar sebelum menyimpulkan, selalu dapat ditelusuri, dan menjaga isolasi data setiap thread obrolan.

---

## 4. PRINSIP PRODUK

**P-01 — Pemahaman sebelum kesimpulan**
PAUGERAN tidak boleh langsung memberikan kesimpulan hukum mendalam apabila informasi material belum memadai.

**P-02 — Dialog adaptif**
Pertanyaan lanjutan harus dihasilkan berdasarkan informasi yang telah diperoleh dalam thread obrolan. PAUGERAN tidak boleh menggunakan daftar pertanyaan statis sebagai satu-satunya mekanisme wawancara.

**P-03 — Pengguna dapat mengoreksi**
PAUGERAN harus menyajikan pemahaman sementara dan memberikan kesempatan kepada pengguna untuk memperbaikinya dalam thread yang sama.

**P-04 — Fakta bukan asumsi**
Pernyataan pengguna harus dibedakan dari fakta yang telah diverifikasi melalui dokumen atau sumber lain.

**P-05 — Sumber adalah bagian dari penalaran**
Sumber hukum bukan sekadar daftar referensi di bagian akhir. Setiap sumber harus mempunyai hubungan dengan klaim atau bagian analisis yang menggunakannya.

**P-06 — Kesimpulan harus dapat ditelusuri**
Setiap kesimpulan material harus dapat ditelusuri melalui:
Kesimpulan → alasan → kaidah → sumber hukum → fakta → bukti/sumber fakta.

**P-07 — Penalaran harus berimbang**
PAUGERAN wajib mencari argumentasi yang dapat melemahkan kesimpulannya sendiri.

**P-08 — Ketidakpastian harus terlihat**
PAUGERAN tidak boleh menyembunyikan:
- informasi yang kurang;
- fakta yang belum diverifikasi;
- konflik norma;
- konflik putusan;
- ketidakpastian interpretasi;
- keterbatasan sumber.

**P-09 — Tidak memalsukan kepastian**
Jika kesimpulan tidak dapat ditentukan secara pasti, PAUGERAN harus menyatakan kondisi tersebut.

**P-10 — Bahasa profesional dan mudah dipahami**
Bahasa keluaran harus memenuhi standar komunikasi hukum profesional Indonesia tanpa sengaja dibuat rumit.

**P-11 — Privasi dan isolasi thread**
Data dalam satu thread obrolan tidak boleh bocor ke thread lain. Setiap thread adalah entitas terisolasi. Data pengguna tidak boleh bocor ke pihak ketiga.

**P-12 — Produk harus siap pakai**
PAUGERAN harus dapat digunakan segera setelah deployment tanpa konfigurasi tambahan yang rumit oleh pengguna akhir.

**P-13 — Chat-first experience**
Antarmuka utama adalah chat yang intuitif. Fitur kompleks (peta keterlacakan, laporan) harus dapat diakses dari dalam chat tanpa meninggalkan konteks obrolan.

---

## 5. AKTOR

**Pengguna**
Orang yang menjelaskan masalah, memberikan fakta, dokumen, tujuan, dan instruksi kepada PAUGERAN melalui antarmuka chat. Pengguna mengakses produk melalui peramban web modern dan harus terautentikasi.

**PAUGERAN**
Agen AI yang melakukan:
- wawancara adaptif dalam thread obrolan;
- pemodelan masalah;
- penelitian hukum;
- penalaran;
- pengujian;
- penyusunan laporan.

**Chat Thread**
Entitas yang menaungi satu sesi analisis kasus. Setiap thread terisolasi dari thread lain dan memiliki:
- ID unik
- Daftar pesan
- Fakta yang diekstrak
- Dokumen yang diunggah
- Peta keterlacakan
- Laporan hukum

**Sumber Hukum**
Sumber eksternal yang digunakan sebagai dasar analisis, termasuk peraturan perundang-undangan, putusan pengadilan, doktrin, dan dokumen resmi lembaga.

**Dokumen Pengguna**
Dokumen yang diberikan pengguna sebagai sumber fakta atau bukti, disimpan secara aman dan terisolasi dalam thread obrolan.

**Administrator Sistem**
Pihak yang mengelola infrastruktur VPS, pembaruan basis data hukum, dan pemantauan operasional.

---

## 6. ARSITEKTUR PRODUK

PAUGERAN dioperasikan sebagai produk web dengan arsitektur tiga lapis yang terpisah secara tegas:

```
┌─────────────────────────────────────────────────────────┐
│  LAPIS 1: ANTARMUKA PENGGUNA (VERCEL)                   │
│  Next.js 14+ + TypeScript + Tailwind CSS                │
│  Di-hosting di Vercel dengan CDN global                 │
────────────────────┬────────────────────────────────────┘
                     │ HTTPS + WebSocket (WSS)
                     │ JWT Authentication
                     ▼
┌─────────────────────────────────────────────────────────┐
│  LAPIS 2: MESIN PRODUK (VPS SELF-HOSTED)                │
│  FastAPI + LangGraph + PostgreSQL + Redis               │
│  Berjalan di VPS dengan Docker Compose                  │
│  Data residensi di server yang dikelola sendiri         │
────────────────────┬────────────────────────────────────┘
                     │ HTTPS (API calls)
                     ▼
┌─────────────────────────────────────────────────────────┐
│  LAPIS 3: LAYANAN EKSTERNAL                             │
│  ├── Anthropic API / OpenAI API (Model AI)              │
│  ├── LangSmith (Observabilitas)                         │
│  └── Situs Hukum Resmi (JDIH, MA, dll - via Playwright) │
└─────────────────────────────────────────────────────────┘
```

### 6.1 Lapis Antarmuka (Vercel)

**Teknologi:**
- Framework: Next.js 14+ (App Router)
- Bahasa: TypeScript
- Styling: Tailwind CSS + shadcn/ui
- State Management: Zustand + React Query
- Streaming: Server-Sent Events (SSE)
- Deployment: Vercel (auto-scaling, edge CDN)

**Tanggung Jawab:**
- Menampilkan antarmuka chat-first dengan sidebar riwayat thread
- Mengelola state sesi pengguna dan thread aktif
- Melakukan streaming respons agen secara real-time
- Menampilkan panel peta keterlacakan dan laporan dalam chat
- Menangani unggahan dokumen
- Menyediakan pengalaman pengguna yang responsif

### 6.2 Lapis Mesin Produk (VPS)

**Teknologi:**
- Framework API: FastAPI (Python 3.11+)
- Orkestrasi Agen: LangGraph
- Database Utama: PostgreSQL 15
- Cache: Redis 7
- Penyimpanan Dokumen: File system lokal dengan enkripsi
- Reverse Proxy: Nginx
- Containerization: Docker + Docker Compose
- SSL: Let's Encrypt

**Tanggung Jawab:**
- Menjalankan siklus agen (11 fase) per thread
- Menyimpan dan mengelola data pengguna dengan isolasi ketat per thread
- Melakukan penelitian hukum (RAG)
- Mengeksekusi browser automation untuk riset
- Menyediakan API yang aman dan terotentikasi
- Menjamin privasi dan kerahasiaan data

### 6.3 Lapis Layanan Eksternal

**Model AI:**
- Anthropic Claude 3.5 Sonnet/Haiku (untuk penalaran berat dan interaktif)
- OpenAI GPT-4o (untuk ekstraksi terstruktur)
- Dikelola melalui LiteLLM untuk routing dan fallback

**Observabilitas:**
- LangSmith untuk pelacakan agen, evaluasi, dan monitoring biaya

**Sumber Hukum Eksternal:**
- Situs JDIH, Mahkamah Agung, BPK, OJK diakses via Playwright
- Hanya domain yang masuk daftar putih yang boleh diakses

---

## 7. SPESIFIKASI ANTARMUKA

### 7.1 Layout Utama

```
┌─────────────────────────────────────────────────────────┐
│  PAUGERAN                          [👤 User]  [⚙️ Settings]│
│  ─────────────────────────────────────────────────────  │
│                                                         │
│  ┌──────────┐  ─────────────────────────────────────┐ │
│  │ Riwayat  │  │  Chat Interface                     │ │
│  │ Thread   │  │                                     │ │
│  │          │  │  ┌─────────────────────────────┐   │ │
│  │ [+ Baru] │  │  │  Header Thread              │   │ │
│  │          │  │  │  Judul: Sengketa Tanah...   │   │ │
│  │ ▼ Hari   │  │  │  [📊 Peta] [📄 Laporan]     │   │ │
│  │  Ini     │  │  └─────────────────────────────┘   │ │
│  │  • Sengketa│ │                                     │ │
│  │    Tanah │  │  ┌─────────────────────────────┐   │ │
│  │  • PHK   │  │  │  🧑 User                    │   │ │
│  │          │  │  │  Saya membeli tanah, sudah  │   │ │
│  │ ▼ Ming-  │  │  │  bayar lunas, tapi penjual  │   │ │
│  │  gu Lalu │  │  │  tidak mau menyerahkan      │   │ │
│  │  • Kontrak│ │  │  sertifikat.                │   │ │
│  │    Kerja │  │  └─────────────────────────────┘   │ │
│  │          │  │                                     │ │
│  │          │  │  ┌─────────────────────────────┐   │ │
│  │          │  │  │   PAUGERAN (Fase: PAHAM)  │   │ │
│  │          │  │  │  Saya memahami masalah      │   │ │
│  │          │  │  │  Anda. Untuk membantu       │   │ │
│  │          │  │  │  analisis yang akurat:      │   │ │
│  │          │  │  │  1. Apakah ada PPJB?        │   │ │
│  │          │  │  │  2. Kapan transaksi?        │   │ │
│  │          │  │  │  [📎 Upload Dokumen]        │   │ │
│  │          │  │  └─────────────────────────────┘   │ │
│  │          │  │                                     │ │
│  │          │  │  ┌─────────────────────────────┐   │ │
│  │          │  │  │  [Ketik pesan...]  [Kirim] │   │ │
│  │          │  │  └─────────────────────────────┘   │ │
│  └──────────┘  └─────────────────────────────────────┘ │
─────────────────────────────────────────────────────────┘
```

### 7.2 Komponen Antarmuka

**7.2.1 Sidebar Riwayat Thread**
- Tombol "Obrolan Baru" untuk membuat thread baru
- Daftar thread dikelompokkan berdasarkan waktu (Hari Ini, Minggu Lalu, Bulan Lalu)
- Setiap item menampilkan judul otomatis (dihasilkan dari pesan pertama)
- Klik thread untuk beralih konteks
- Hover untuk opsi: rename, delete, archive

**7.2.2 Header Thread**
- Judul thread (editable)
- Indikator fase agen saat ini (PAHAM, TANYA, NALAR, dll)
- Tombol akses cepat:
  -  Peta Keterlacakan (buka panel samping)
  - 📄 Laporan (buka panel laporan)
  - ⚙️ Pengaturan thread
  - 📥 Ekspor (PDF/DOCX)

**7.2.3 Area Chat**
- Pesan pengguna: aligned right, background biru muda
- Pesan PAUGERAN: aligned left, background abu-abu terang
- Indikator fase ditampilkan di header pesan PAUGERAN
- Streaming text dengan efek typing
- Tombol aksi kontekstual pada pesan PAUGERAN:
  - Fase PAHAM/TANYA: [Koreksi Fakta] [Lanjut]
  - Fase KONFIRMASI: [Setuju] [Revisi] [Mulai Penalaran]
  - Fase NALAR/BANTAH: [Lihat Detail] [Tambah Argumen]
  - Fase SIMPULKAN: [Lihat Peta] [Ekspor Laporan]

**7.2.4 Input Area**
- Textarea multi-line untuk input pesan
- Tombol  Upload Dokumen (PDF, DOCX, TXT, max 10MB)
- Tombol Kirim (Enter untuk kirim, Shift+Enter untuk baris baru)
- Indikator status: "PAUGERAN sedang mengetik..." saat agen memproses

**7.2.5 Panel Samping (Drawer)**
- Dapat dibuka/tutup dari header thread
- Mode Peta Keterlacakan:
  - Visualisasi graf interaktif (React Flow)
  - Node: Kesimpulan, Kaidah, Fakta, Sumber
  - Zoom, pan, click untuk detail
  - Tombol ekspor PNG/SVG
- Mode Laporan:
  - Tampilan 24 poin laporan
  - Collapsible sections
  - Editable rich text
  - Tombol ekspor PDF/DOCX

### 7.3 Autentikasi

**7.3.1 Login**
- Email + Magic Link (tanpa password)
- Atau Google/Microsoft SSO
- Redirect ke thread terakhir atau dashboard

**7.3.2 Registrasi**
- Input email
- Verifikasi email via link
- Setup profil dasar (nama, profesi)

**7.3.3 Session Management**
- JWT token dengan refresh token
- Session timeout: 24 jam
- Multi-device support
- Logout dari semua perangkat

---

## 8. SPESIFIKASI CHAT THREAD

### 8.1 Entitas Thread

Setiap thread adalah entitas terisolasi dengan struktur:

```typescript
interface ChatThread {
  id: string;                    // UUID
  userId: string;                // Owner
  title: string;                 // Auto-generated dari pesan pertama
  createdAt: Date;
  updatedAt: Date;
  status: 'active' | 'archived' | 'deleted';
  currentPhase: AgentPhase;
  factsComplete: boolean;
}

interface Message {
  id: string;
  threadId: string;              // Foreign key ke thread
  role: 'user' | 'assistant';
  content: string;
  phase: AgentPhase;             // Fase saat pesan dikirim
  metadata: {
    facts?: Fact[];
    documents?: string[];        // Document IDs
    citations?: Citation[];
  };
  createdAt: Date;
}

interface Fact {
  id: string;
  threadId: string;              // Isolasi ketat
  content: string;
  source: 'user_statement' | 'document' | 'verified';
  status: 'stated' | 'supported' | 'verified' | 'disputed';
  relevance: string;
  certainty: 'high' | 'medium' | 'low';
  createdAt: Date;
}

interface Document {
  id: string;
  threadId: string;              // Isolasi ketat
  filename: string;
  fileType: 'pdf' | 'docx' | 'txt';
  fileSize: number;
  filePath: string;              // Encrypted storage
  uploadedAt: Date;
}

interface TraceabilityEdge {
  id: string;
  threadId: string;              // Isolasi ketat
  conclusionId: string;
  reason: string;
  ruleId: string;
  factId: string;
  evidenceSource: string;
}
```

### 8.2 Aturan Isolasi

**Aturan Emas:**
> Data dari Thread A TIDAK BOLEH diakses oleh Thread B, bahkan oleh pengguna yang sama.

**Implementasi:**
- Setiap query database WAJIB memfilter berdasarkan `threadId`
- API endpoint WAJIB memverifikasi ownership: `thread.userId == currentUser.id`
- File storage menggunakan path terenkripsi per thread
- Cache Redis menggunakan prefix `thread:{threadId}:*`

**Pengecualian (Data Global):**
- Database hukum (peraturan, putusan) bersifat global dan dapat diakses semua thread
- Template laporan bersifat global
- Model AI bersifat global

### 8.3 Siklus Hidup Thread

1. **Created**: Thread dibuat saat pengguna klik "Obrolan Baru" atau kirim pesan pertama
2. **Active**: Thread sedang digunakan, fase agen berjalan
3. **Completed**: Analisis selesai, laporan dihasilkan
4. **Archived**: Thread tidak aktif tapi masih dapat diakses
5. **Deleted**: Thread dihapus (soft delete 30 hari, hard delete setelahnya)

---

## 9. SPESIFIKASI AGEN

### 9.1 State Machine (LangGraph)

Siklus agen diimplementasikan sebagai state machine dengan 11 node:

```
PAHAM → TANYA → KONFIRMASI → (kondisional) → RUMUSKAN → TELITI → 
VERIFIKASI → NALAR → BANTAH → UJI → SIMPULKAN → TELUSURI → END
```

**Kondisi Percabangan:**
- Dari KONFIRMASI: 
  - Jika `factsComplete == false` → kembali ke TANYA
  - Jika `factsComplete == true` → lanjut ke RUMUSKAN
- Dari TELUSURI: 
  - Jika ada kesimpulan tanpa `citation_id` → kembali ke TELITI
  - Jika semua valid → END

### 9.2 State Schema

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

### 9.3 Node Implementasi

Setiap node diimplementasikan sebagai fungsi async yang:
1. Menerima state saat ini
2. Memanggil LLM yang sesuai (berdasarkan routing)
3. Memvalidasi output dengan Pydantic
4. Memperbarui state
5. Menyimpan perubahan ke database
6. Mengirim event ke frontend via SSE

**Contoh Node PAHAM:**
```python
async def node_paham(state: AgentState) -> AgentState:
    # Ekstrak informasi awal dari pesan pertama
    prompt = f"""
    Anda adalah PAUGERAN. Pahami masalah awal pengguna:
    
    {state.messages[0].content}
    
    Identifikasi:
    1. Pihak yang terlibat
    2. Objek masalah
    3. Kronologi awal
    4. Informasi yang belum diketahui
    
    Output dalam JSON terstruktur.
    """
    
    response = await llm_call("interactive", prompt)
    parsed = parse_paham_response(response)
    
    # Update state dengan fakta awal
    state.facts.extend(parsed.facts)
    state.user_goals = parsed.goals
    
    # Simpan ke database
    await db.save_facts(state.thread_id, state.facts)
    
    # Kirim pesan ke user
    await send_message(state.thread_id, {
        "role": "assistant",
        "phase": "PAHAM",
        "content": parsed.summary
    })
    
    return state
```

### 9.4 Model Routing

- **Node interaktif** (PAHAM, TANYA, KONFIRMASI): 
  - Model: Claude 3.5 Haiku / GPT-4o-mini
  - Tujuan: Respons cepat, biaya rendah
  
- **Node penalaran** (NALAR, BANTAH, UJI, SIMPULKAN): 
  - Model: Claude 3.5 Sonnet / GPT-4o
  - Tujuan: Penalaran mendalam, akurasi tinggi
  
- **Node ekstraksi** (TELITI, VERIFIKASI): 
  - Model: GPT-4o (JSON mode)
  - Tujuan: Ekstraksi terstruktur, validasi ketat

---

## 10. SPESIFIKASI DATA

### 10.1 Database PostgreSQL

**Tabel Utama:**

```sql
-- Users
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255),
    profession VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login TIMESTAMP
);

-- Chat Threads
CREATE TABLE chat_threads (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(255),
    status VARCHAR(20) DEFAULT 'active',
    current_phase VARCHAR(50) DEFAULT 'PAHAM',
    facts_complete BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Messages
CREATE TABLE messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    thread_id UUID REFERENCES chat_threads(id) ON DELETE CASCADE,
    role VARCHAR(20) NOT NULL,
    content TEXT NOT NULL,
    phase VARCHAR(50),
    metadata JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Facts
CREATE TABLE facts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    thread_id UUID REFERENCES chat_threads(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    source VARCHAR(50) NOT NULL,
    status VARCHAR(50) NOT NULL,
    relevance TEXT,
    certainty VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Documents
CREATE TABLE documents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    thread_id UUID REFERENCES chat_threads(id) ON DELETE CASCADE,
    filename VARCHAR(255) NOT NULL,
    file_type VARCHAR(20) NOT NULL,
    file_size INTEGER NOT NULL,
    file_path TEXT NOT NULL,
    uploaded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Legal Rules (Global)
CREATE TABLE legal_rules (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    source VARCHAR(255) NOT NULL,
    article VARCHAR(100) NOT NULL,
    content TEXT NOT NULL,
    effective_date DATE,
    revoked_date DATE,
    metadata JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Traceability Edges
CREATE TABLE traceability_edges (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    thread_id UUID REFERENCES chat_threads(id) ON DELETE CASCADE,
    conclusion_id UUID,
    reason TEXT,
    rule_id UUID REFERENCES legal_rules(id),
    fact_id UUID REFERENCES facts(id),
    evidence_source TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Audit Logs
CREATE TABLE audit_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id),
    thread_id UUID REFERENCES chat_threads(id),
    action VARCHAR(100) NOT NULL,
    details JSONB,
    ip_address INET,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Indexes untuk performa
CREATE INDEX idx_messages_thread_id ON messages(thread_id);
CREATE INDEX idx_facts_thread_id ON facts(thread_id);
CREATE INDEX idx_documents_thread_id ON documents(thread_id);
CREATE INDEX idx_traceability_thread_id ON traceability_edges(thread_id);
CREATE INDEX idx_chat_threads_user_id ON chat_threads(user_id);
CREATE INDEX idx_chat_threads_status ON chat_threads(status);
```

### 10.2 Redis Cache

**Struktur Key:**
```
thread:{thread_id}:state          # Agent state sementara
thread:{thread_id}:messages       # Cache pesan terakhir
user:{user_id}:session            # Session data
api:rate_limit:{user_id}          # Rate limiting
rag:embedding:{rule_id}           # Cache embedding hukum
```

**TTL (Time to Live):**
- Agent state: 24 jam
- Session: 24 jam
- Rate limit: 1 jam
- Embedding: 7 hari

### 10.3 Vector Database (pgvector)

```sql
-- Enable pgvector extension
CREATE EXTENSION IF NOT EXISTS vector;

-- Embedding table untuk RAG
CREATE TABLE legal_embeddings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    rule_id UUID REFERENCES legal_rules(id),
    content TEXT NOT NULL,
    embedding vector(1536),  # OpenAI embedding dimension
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Index untuk pencarian semantik
CREATE INDEX ON legal_embeddings USING ivfflat (embedding vector_cosine_ops);
```

### 10.4 Penyimpanan Dokumen

**Lokasi:** `/opt/paugeran/data/documents/{user_id}/{thread_id}/`

**Struktur File:**
```
documents/
├── {user_id}/
│   ├── {thread_id}/
│   │   ├── original/
│   │   │   ── {document_id}_filename.pdf
│   │   ├── processed/
│   │   │   └── {document_id}_extracted.txt
│   │   └── metadata.json
```

**Keamanan:**
- Enkripsi at rest menggunakan LUKS
- File permissions: 600 (hanya owner bisa baca/tulis)
- Path menggunakan UUID, bukan nama asli file

---

## 11. SPESIFIKASI API

### 11.1 Base URL

- Development: `http://localhost:8000/api/v1`
- Production: `https://api.paugeran.com/api/v1`

### 11.2 Autentikasi

**Header:**
```
Authorization: Bearer {jwt_token}
Content-Type: application/json
```

### 11.3 Endpoint

#### Autentikasi

**POST /auth/register**
```json
Request:
{
  "email": "user@example.com",
  "name": "John Doe",
  "profession": "Advokat"
}

Response:
{
  "message": "Verification email sent",
  "user_id": "uuid"
}
```

**POST /auth/login**
```json
Request:
{
  "email": "user@example.com"
}

Response:
{
  "message": "Magic link sent to email"
}
```

**POST /auth/verify**
```json
Request:
{
  "token": "verification_token"
}

Response:
{
  "access_token": "jwt",
  "refresh_token": "jwt",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

#### Chat Threads

**GET /threads**
```json
Response:
{
  "threads": [
    {
      "id": "uuid",
      "title": "Sengketa Tanah",
      "status": "active",
      "current_phase": "NALAR",
      "created_at": "2026-08-26T10:00:00Z",
      "updated_at": "2026-08-26T11:30:00Z"
    }
  ],
  "total": 1
}
```

**POST /threads**
```json
Request:
{
  "title": "Sengketa Kontrak"  // Optional, auto-generated jika kosong
}

Response:
{
  "id": "uuid",
  "title": "Sengketa Kontrak",
  "status": "active",
  "current_phase": "PAHAM",
  "created_at": "2026-08-26T12:00:00Z"
}
```

**GET /threads/{thread_id}**
```json
Response:
{
  "id": "uuid",
  "title": "Sengketa Tanah",
  "status": "active",
  "current_phase": "NALAR",
  "facts_complete": true,
  "messages": [...],
  "facts": [...],
  "documents": [...]
}
```

**DELETE /threads/{thread_id}**
```json
Response:
{
  "message": "Thread deleted"
}
```

#### Messages

**POST /threads/{thread_id}/messages**
```json
Request:
{
  "content": "Saya membeli tanah pada Januari 2024"
}

Response:
{
  "message_id": "uuid",
  "thread_id": "uuid",
  "status": "processing",
  "phase": "TANYA"
}
```

**GET /threads/{thread_id}/messages/stream**
```
Response: Server-Sent Events (SSE)

data: {"type": "phase", "phase": "PAHAM"}

data: {"type": "token", "content": "Saya"}

data: {"type": "token", "content": " memahami"}

data: {"type": "complete", "message_id": "uuid"}
```

#### Documents

**POST /threads/{thread_id}/documents**
```
Request: multipart/form-data
- file: PDF/DOCX/TXT

Response:
{
  "document_id": "uuid",
  "filename": "kontrak.pdf",
  "file_size": 1024000,
  "status": "processing"
}
```

**GET /threads/{thread_id}/documents/{document_id}**
```json
Response:
{
  "id": "uuid",
  "filename": "kontrak.pdf",
  "file_type": "pdf",
  "file_size": 1024000,
  "uploaded_at": "2026-08-26T10:00:00Z"
}
```

**DELETE /threads/{thread_id}/documents/{document_id}**
```json
Response:
{
  "message": "Document deleted"
}
```

#### Analysis

**POST /threads/{thread_id}/analysis/confirm**
```json
Request:
{
  "action": "confirm"  // atau "revise"
}

Response:
{
  "status": "proceeding",
  "next_phase": "RUMUSKAN"
}
```

**POST /threads/{thread_id}/analysis/start-reasoning**
```json
Response:
{
  "status": "started",
  "phase": "TELITI",
  "estimated_time": 180  // seconds
}
```

**GET /threads/{thread_id}/analysis/report**
```json
Response:
{
  "report": {
    "executive_summary": "...",
    "facts": [...],
    "legal_issues": [...],
    "analysis": [...],
    "conclusion": "...",
    "traceability_map": [...]
  },
  "generated_at": "2026-08-26T12:00:00Z"
}
```

**POST /threads/{thread_id}/analysis/export**
```json
Request:
{
  "format": "pdf",  // atau "docx"
  "sections": ["all"]  // atau specific sections
}

Response:
{
  "download_url": "https://api.paugeran.com/downloads/...",
  "expires_at": "2026-08-26T13:00:00Z"
}
```

#### Traceability

**GET /threads/{thread_id}/traceability**
```json
Response:
{
  "nodes": [
    {
      "id": "conclusion-1",
      "type": "conclusion",
      "content": "Pembeli dapat menuntut penjual",
      "position": {"x": 0, "y": 0}
    },
    {
      "id": "rule-1",
      "type": "rule",
      "content": "Pasal 1338 KUHPerdata",
      "position": {"x": -100, "y": 100}
    }
  ],
  "edges": [
    {
      "id": "edge-1",
      "source": "conclusion-1",
      "target": "rule-1",
      "label": "berdasarkan"
    }
  ]
}
```

#### Users

**GET /users/me**
```json
Response:
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "John Doe",
  "profession": "Advokat",
  "created_at": "2026-01-01T00:00:00Z"
}
```

**PUT /users/me**
```json
Request:
{
  "name": "John Doe Jr.",
  "profession": "Legal Consultant"
}

Response:
{
  "id": "uuid",
  "name": "John Doe Jr.",
  "profession": "Legal Consultant"
}
```

**DELETE /users/me**
```json
Response:
{
  "message": "Account deletion scheduled",
  "deletion_date": "2026-09-25T00:00:00Z"
}
```

### 11.4 Error Handling

**Format Error:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ],
    "timestamp": "2026-08-26T10:00:00Z",
    "request_id": "req-uuid"
  }
}
```

**HTTP Status Codes:**
- 200: Success
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 409: Conflict
- 429: Too Many Requests
- 500: Internal Server Error

### 11.5 Rate Limiting

**Limits:**
- Login: 5 requests per minute per IP
- Create thread: 20 per hour per user
- Send message: 60 per minute per thread
- Upload document: 10 per hour per thread
- Export report: 30 per hour per user
- API calls: 1000 per hour per user

**Headers:**
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 950
X-RateLimit-Reset: 1629964800
```

---

## 12. SPESIFIKASI KEAMANAN

### 12.1 Autentikasi & Otorisasi

**JWT Token:**
- Algorithm: HS256
- Access token expiry: 15 menit
- Refresh token expiry: 24 jam
- Secret key: minimum 256-bit

**Password (jika digunakan):**
- Hash: bcrypt dengan cost factor 12
- Minimum length: 12 karakter
- Complexity: huruf besar, huruf kecil, angka, simbol

**Session Management:**
- Session timeout: 24 jam inactive
- Concurrent sessions: unlimited
- Force logout: tersedia dari dashboard

### 12.2 Perlindungan Data

**Enkripsi:**
- At rest: AES-256 untuk database dan file
- In transit: TLS 1.3 untuk semua komunikasi
- API keys: stored in environment variables, never in database

**PII Redaction:**
```python
# Sebelum dikirim ke API model
def redact_pii(text: str) -> Tuple[str, Dict[str, str]]:
    # Detect dan replace PII
    # Return redacted text + mapping untuk de-anonymize
    pass
```

**Data Residency:**
- Semua data disimpan di VPS Jakarta
- Tidak ada replikasi ke region lain tanpa persetujuan
- Backup disimpan di storage terpisah dalam region yang sama

### 12.3 Perlindungan Infrastruktur

**Firewall:**
```bash
# Hanya port yang diperlukan
ALLOW: 80 (HTTP - redirect to HTTPS)
ALLOW: 443 (HTTPS)
ALLOW: 22 (SSH - key only, fail2ban)
DENY: All other ports
```

**Security Measures:**
- Fail2ban untuk pencegahan brute force
- Automatic security updates (unattended-upgrades)
- Regular vulnerability scanning (Trivy)
- Intrusion Detection System (AIDE)
- DDoS protection (Cloudflare di depan Vercel)

### 12.4 Audit Trail

**Logging:**
```python
# Semua aksi kritis dicatat
audit_log(
    user_id=current_user.id,
    thread_id=thread_id,
    action="document_upload",
    details={"document_id": doc_id, "filename": filename},
    ip_address=request.client.host
)
```

**Log Retention:**
- Audit logs: 2 tahun
- Access logs: 90 hari
- Error logs: 1 tahun

### 12.5 Kepatuhan

**Regulasi:**
- UU PDP (Perlindungan Data Pribadi) Indonesia
- Kerahasiaan profesi hukum (untuk advokat)
- ISO 27001 best practices

**Data Processing Agreement:**
- Data tidak digunakan untuk melatih model AI
- Zero data retention dari penyedia API model
- Pengguna memiliki hak akses, koreksi, dan penghapusan data

---

## 13. SPESIFIKASI DEPLOYMENT

### 13.1 Frontend (Vercel)

**Configuration:**
```json
// vercel.json
{
  "buildCommand": "pnpm build",
  "outputDirectory": ".next",
  "devCommand": "pnpm dev",
  "installCommand": "pnpm install",
  "framework": "nextjs",
  "regions": ["sin1"],
  "env": {
    "NEXT_PUBLIC_API_URL": "https://api.paugeran.com",
    "NEXT_PUBLIC_WS_URL": "wss://api.paugeran.com/ws"
  }
}
```

**Deployment:**
- Auto-deploy dari branch `main`
- Preview deployment untuk setiap PR
- Custom domain: `app.paugeran.com`
- CDN: Vercel Edge Network

### 13.2 Backend (VPS)

**Spesifikasi Minimum:**
- CPU: 4 cores
- RAM: 8 GB
- Storage: 100 GB SSD
- OS: Ubuntu 22.04 LTS
- Provider: DigitalOcean/Vultr/Contabo (Jakarta region)

**Docker Compose:**
```yaml
# infra/docker/docker-compose.yml
version: '3.8'

services:
  backend:
    build: ../apps/api
    container_name: paugeran-backend
    restart: always
    environment:
      - DATABASE_URL=postgresql://paugeran:${DB_PASSWORD}@postgres:5432/paugeran
      - REDIS_URL=redis://redis:6379
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - LANGCHAIN_API_KEY=${LANGSMITH_API_KEY}
      - CORS_ORIGINS=https://app.paugeran.com
      - JWT_SECRET=${JWT_SECRET}
    ports:
      - "8000:8000"
    volumes:
      - ./data/documents:/app/documents
      - ./data/logs:/app/logs
    depends_on:
      - postgres
      - redis
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  postgres:
    image: postgres:15-alpine
    container_name: paugeran-postgres
    restart: always
    environment:
      - POSTGRES_DB=paugeran
      - POSTGRES_USER=paugeran
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U paugeran"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    container_name: paugeran-redis
    restart: always
    command: redis-server --appendonly yes
    volumes:
      - redis_data:/data
    ports:
      - "6379:6379"
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  nginx:
    image: nginx:alpine
    container_name: paugeran-nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - backend

volumes:
  postgres_data:
  redis_data:
```

**Nginx Configuration:**
```nginx
# infra/nginx/nginx.conf
events {
    worker_connections 1024;
}

http {
    upstream backend {
        server backend:8000;
    }

    # Rate limiting
    limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;

    server {
        listen 80;
        server_name api.paugeran.com;
        return 301 https://$server_name$request_uri;
    }

    server {
        listen 443 ssl http2;
        server_name api.paugeran.com;

        ssl_certificate /etc/nginx/ssl/fullchain.pem;
        ssl_certificate_key /etc/nginx/ssl/privkey.pem;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers HIGH:!aNULL:!MD5;

        # Security headers
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;

        # API routes
        location /api/ {
            limit_req zone=api burst=20 nodelay;
            
            proxy_pass http://backend;
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
            proxy_read_timeout 60s;
        }

        # Health check
        location /health {
            proxy_pass http://backend/health;
            access_log off;
        }
    }
}
```

### 13.3 Database Migration

**Alembic Configuration:**
```python
# apps/api/alembic/env.py
def run_migrations_online():
    with connectable.connect() as connection:
        context.configure(
            connection=connection,
            target_metadata=target_metadata,
            compare_type=True,
            compare_server_default=True
        )
        context.run_migrations()
```

**Migration Command:**
```bash
docker-compose exec backend alembic upgrade head
```

### 13.4 CI/CD Pipeline

**GitHub Actions:**
```yaml
# .github/workflows/deploy-backend.yml
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
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to VPS
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /opt/paugeran
            git pull origin main
            docker-compose -f infra/docker/docker-compose.yml down
            docker-compose -f infra/docker/docker-compose.yml up -d --build
            docker-compose exec backend alembic upgrade head
            docker system prune -af
```

### 13.5 Monitoring

**Health Checks:**
```python
# apps/api/app/main.py
@app.get("/health")
async def health_check():
    # Check database
    try:
        await db.execute("SELECT 1")
        db_status = "healthy"
    except:
        db_status = "unhealthy"
    
    # Check Redis
    try:
        await redis.ping()
        redis_status = "healthy"
    except:
        redis_status = "unhealthy"
    
    status = "healthy" if db_status == "healthy" and redis_status == "healthy" else "unhealthy"
    
    return {
        "status": status,
        "version": "1.0.0",
        "database": db_status,
        "redis": redis_status,
        "timestamp": datetime.utcnow().isoformat()
    }
```

**Uptime Monitoring:**
- UptimeRobot atau Pingdom
- Check interval: 1 menit
- Alert via email/Slack jika down

---

## 14. SIKLUS AGEN

### 14.1 Diagram Siklus

```
                    ┌─────────────┐
                    │   START     │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │    PAHAM    │
                    │ (Pahami     │
                    │  masalah)   │
                    └──────┬──────
                           │
                           ▼
                    ─────────────┐
                    │    TANYA    │
                    │ (Gali fakta │
                    │  material)  │
                    └──────┬──────
                           │
                           ▼
                    ┌─────────────┐
              ┌─────│ KONFIRMASI  │◄─────┐
              │     │ (Validasi   │      │
              │     │  pemahaman) │      │
              │     └──────┬──────┘      │
              │            │             │
              │     ┌──────┴──────     │
              │     │Facts Complete?│   │
              │     └──────┬──────┘     │
              │            │             │
              │      No    │    Yes     │
              │     ┌──────┴──────┐     │
              │     │             │     │
              │     ▼             ▼     │
              │  (Kembali)    (Lanjut)  │
              │     │             │     │
              │     │             ▼     │
              │     │     ┌───────────┐ │
              │     │     │  RUMUSKAN │ │
              │     │     │ (Isu &    │ │
              │     │     │  norma)   │ │
              │     │     ─────┬─────┘ │
              │     │           │       │
              │     │           ▼       │
              │     │     ┌───────────┐ │
              │     │     │   TELITI  │ │
              │     │     │ (Cari     │ │
              │     │     │  sumber)  │ │
              │     │     └─────┬─────┘ │
              │     │           │       │
              │     │           ▼       │
              │     │     ┌───────────┐ │
              │     │     │VERIFIKASI │ │
              │     │     │ (Validasi │ │
              │     │     │  norma)   │ │
              │     │     └──────────┘ │
              │     │           │       │
              │     │           ▼       │
              │     │     ───────────┐ │
              │     │     │   NALAR   │ │
              │     │     │ (Terapkan │ │
              │     │     │  hukum)   │ │
              │     │     └─────┬─────┘ │
              │     │           │       │
              │     │           ▼       │
              │     │     ┌───────────┐ │
              │     │     │   BANTAH  │ │
              │     │     │ (Kontra   │ │
              │     │     │  argumen) │ │
              │     │     └──────────┘ │
              │     │           │       │
              │     │           ▼       │
              │     │     ───────────┐ │
              │     │     │    UJI    │ │
              │     │     │ (Validasi │ │
              │     │     │  argumen) │ │
              │     │     ─────┬─────┘ │
              │     │           │       │
              │     │           ▼       │
              │     │     ┌───────────┐ │
              │     │     │SIMPULKAN  │ │
              │     │     │ (Buat     │ │
              │     │     │  kesimpulan)│
              │     │     └─────┬───── │
              │     │           │       │
              │     │           ▼       │
              │     │     ┌───────────┐ │
              │     │     │  TELUSURI │ │
              │     │     │ (Validasi │ │
              │     │     │  citation)│ │
              │     │     └─────┬─────┘ │
              │     │           │       │
              │     │     ─────┴─────┐ │
              │     │     │All Valid? │ │
              │     │     ─────┬─────┘ │
              │     │           │       │
              │     │      No   │  Yes  │
              │     │     ┌──────────┐ │
              │     │     │           │ │
              │     │     ▼           ▼ │
              │     │  (Kembali)   (END)│
              │     │     │             │
              │     └─────┘             │
              │                         │
              └─────────────────────────┘
```

### 14.2 Detail Setiap Fase

**Fase 1: PAHAM**
- Input: Pesan pertama pengguna
- Proses: Ekstrak informasi dasar (pihak, objek, kronologi)
- Output: Fakta awal, tujuan pengguna
- LLM: Claude Haiku (cepat)
- Duration: < 5 detik

**Fase 2: TANYA**
- Input: Fakta awal
- Proses: Identifikasi informasi yang hilang, generate pertanyaan adaptif
- Output: Pertanyaan klarifikasi (1-3 pertanyaan)
- LLM: Claude Haiku
- Duration: < 5 detik

**Fase 3: KONFIRMASI**
- Input: Semua fakta yang terkumpul
- Proses: Susun rekonstruksi masalah, tampilkan ke pengguna
- Output: Ringkasan pemahaman, tombol "Setuju" atau "Revisi"
- LLM: Claude Haiku
- Duration: < 5 detik
- Conditional: Jika fakta belum lengkap → kembali ke TANYA

**Fase 4: RUMUSKAN**
- Input: Fakta yang dikonfirmasi
- Proses: Identifikasi isu hukum, bidang hukum, yurisdiksi
- Output: Daftar isu hukum, klasifikasi masalah
- LLM: Claude Sonnet (mendalam)
- Duration: < 10 detik

**Fase 5: TELITI**
- Input: Isu hukum
- Proses: Cari peraturan, putusan, doktrin yang relevan (RAG)
- Output: Daftar sumber hukum dengan metadata
- LLM: GPT-4o (terstruktur)
- Duration: < 30 detik
- Tools: Vector search, Playwright (jika perlu)

**Fase 6: VERIFIKASI**
- Input: Sumber hukum yang ditemukan
- Proses: Periksa keberlakuan, status, tanggal efektif
- Output: Sumber yang terverifikasi dengan status
- LLM: GPT-4o
- Duration: < 15 detik

**Fase 7: NALAR**
- Input: Fakta + sumber hukum terverifikasi
- Proses: Terapkan hukum pada fakta, analisis unsur, bangun argumen
- Output: Argumen pendukung dengan struktur lengkap
- LLM: Claude Sonnet/Opus
- Duration: < 30 detik

**Fase 8: BANTAH**
- Input: Argumen pendukung
- Proses: Cari kelemahan, pengecualian, kontra-argumen, putusan berlawanan
- Output: Argumen berlawanan yang kuat
- LLM: Claude Sonnet/Opus
- Duration: < 30 detik

**Fase 9: UJI**
- Input: Argumen pendukung + berlawanan
- Proses: Evaluasi kekuatan kedua sisi, identifikasi ketidakpastian
- Output: Penilaian berimbang dengan tingkat kepastian
- LLM: Claude Sonnet
- Duration: < 20 detik

**Fase 10: SIMPULKAN**
- Input: Hasil pengujian
- Proses: Susun kesimpulan dengan kategori kepastian
- Output: Kesimpulan hukum dengan penjelasan
- LLM: Claude Sonnet
- Duration: < 15 detik

**Fase 11: TELUSURI**
- Input: Kesimpulan + semua data
- Proses: Bangun peta keterlacakan, validasi setiap citation
- Output: Peta keterlacakan lengkap, laporan 24 poin
- LLM: GPT-4o (terstruktur)
- Duration: < 20 detik
- Conditional: Jika ada kesimpulan tanpa citation → kembali ke TELITI

---

## 15. MODE PEMAHAMAN

### 15.1 Wawancara Bertingkat

**Tingkat 1: Identifikasi Struktur Dasar**
- Siapa pihak yang terlibat?
- Apa yang terjadi?
- Kapan kejadian?
- Di mana lokasi?
- Apa tujuan pengguna?

**Tingkat 2: Fakta Material**
- Apakah ada perjanjian tertulis?
- Apakah ada bukti pembayaran?
- Apakah ada komunikasi tertulis?
- Apakah ada saksi?
- Apakah ada dokumen pendukung?

**Tingkat 3: Fakta Kritis**
- Apakah ada pengecualian atau kondisi khusus?
- Apakah ada sengketa fakta?
- Apakah ada proses hukum yang sudah berjalan?
- Apakah ada perubahan perjanjian?
- Apakah ada tindakan pihak lawan yang relevan?

### 15.2 Kriteria Pertanyaan

Setiap pertanyaan harus memenuhi:
1. **Relevansi**: Langsung terkait dengan unsur hukum
2. **Material**: Dapat mengubah hasil analisis
3. **Spesifik**: Jelas dan tidak ambigu
4. **Efisien**: Tidak bertanya hal yang sudah diketahui

**Prioritas Pertanyaan:**
```
1. Unsur hukum yang belum terpenuhi
2. Fakta yang mempengaruhi klasifikasi masalah
3. Bukti yang diperlukan untuk pembuktian
4. Informasi yang mempengaruhi strategi
```

### 15.3 Rekonstruksi Masalah

**Format Output:**
```
PEMAHAMAN SAYA SAAT INI

Tujuan:
[Ringkasan tujuan pengguna]

Para Pihak:
1. [Pihak 1] - [Peran]
2. [Pihak 2] - [Peran]

Kronologi:
1. [Tanggal] - [Peristiwa]
2. [Tanggal] - [Peristiwa]

Fakta yang Telah Disampaikan:
✓ [Fakta 1]
✓ [Fakta 2]

Fakta yang Belum Jelas:
? [Informasi yang masih dibutuhkan]

Dokumen:
📎 [Dokumen 1]
📎 [Dokumen 2]

Masalah Potensial:
️ [Isu 1]
️ [Isu 2]

Hal yang Masih Perlu Dipastikan:
• [Pertanyaan 1]
• [Pertanyaan 2]

---
Apakah pemahaman saya sudah sesuai?
[Setuju] [Revisi]
```

---

## 16. MODE PENALARAN

### 16.1 Penelitian Hukum

**Langkah Penelitian:**
1. Identifikasi bidang hukum (perdata, pidana, tata usaha negara, dll)
2. Identifikasi yurisdiksi (pengadilan negeri, agama, niaga, dll)
3. Tentukan periode waktu yang relevan
4. Cari norma primer (UU, PP, Perpres)
5. Cari norma sekunder (Permen, Surat Edaran)
6. Cari putusan pengadilan yang relevan
7. Cari interpretasi dan doktrin
8. Identifikasi pengecualian
9. Cari sumber yang berlawanan
10. Verifikasi silang semua sumber

### 16.2 Hirarki Sumber

**Prioritas:**
```
1. Undang-Undang (UU)
2. Peraturan Pemerintah (PP)
3. Peraturan Presiden (Perpres)
4. Peraturan Menteri (Permen)
5. Putusan Mahkamah Agung
6. Putusan Pengadilan Tinggi/Negeri
7. Doktrin/Literatur Hukum
8. Sumber sekunder lainnya
```

**Metadata Sumber:**
```python
class LegalSource(BaseModel):
    id: str
    type: str  # "UU", "PP", "Putusan", etc
    number: str
    year: int
    title: str
    article: Optional[str]
    content: str
    effective_date: date
    revoked_date: Optional[date]
    status: str  # "active", "amended", "revoked"
    relevance_score: float
    citation_count: int
```

### 16.3 Pemeriksaan Keberlakuan

**Checklist:**
- [ ] Nomor peraturan lengkap
- [ ] Tanggal pengesahan
- [ ] Tanggal mulai berlaku
- [ ] Status saat ini (masih berlaku/diubah/dicabut)
- [ ] Peraturan yang mengubah
- [ ] Peraturan yang dicabut
- [ ] Relevansi dengan waktu kejadian

### 16.4 Model Norma

**Struktur:**
```
SUMBER: UU No. 5 Tahun 1986
PASAL: Pasal 53 ayat (1)
SUBJEK: Setiap orang
PERBUATAN: Melanggar ketentuan peraturan perundang-undangan
SYARAT: Dengan sengaja
LARANGAN/Kewajiban: Dilarang melakukan...
AKIBAT HUKUM: Diancam dengan pidana...
PENGEcUALIAN: Kecuali...
```

---

## 17. SPESIFIKASI OUTPUT

### 17.1 Laporan Hukum 24 Poin

**Struktur Lengkap:**

```
ANALISIS HUKUM PAUGERAN
========================

1. RINGKASAN EKSEKUTIF
   [Kesimpulan utama dalam 2-3 paragraf]

2. TUJUAN PENGGUNA
   [Apa yang ingin dicapai pengguna]

3. REKONSTRUKSI MASALAH
   [Bagaimana PAUGERAN memahami perkara]

4. FAKTA
   [Fakta yang diketahui dan statusnya]
   - Fakta yang dinyatakan pengguna
   - Fakta yang didukung dokumen
   - Fakta yang diverifikasi

5. KEKURANGAN FAKTA
   [Informasi material yang belum tersedia]

6. DOKUMEN DAN BUKTI
   [Dokumen yang tersedia dan relevansinya]

7. MASALAH HUKUM
   [Pertanyaan hukum yang harus dijawab]

8. DASAR HUKUM
   [Seluruh dasar hukum yang relevan]
   - Peraturan perundang-undangan
   - Putusan pengadilan
   - Doktrin

9. STATUS KEBERLAKUAN
   [Status masing-masing norma]

10. KAIDAH HUKUM
    [Kaidah yang ditarik dari sumber]

11. UNSUR HUKUM
    [Unsur yang harus dipenuhi]

12. PENERAPAN TERHADAP FAKTA
    [Analisis hubungan fakta dan hukum]

13. ARGUMEN PENDUKUNG
    [Argumen yang memperkuat posisi]

14. ARGUMEN BERLAWANAN
    [Argumen yang melemahkan posisi]

15. ANALISIS KONTRAARGUMENTASI
    [Pengujian terhadap kedua sisi]

16. PUTUSAN RELEVAN
    [Putusan dan analisis relevansinya]

17. KONFLIK DAN AMBIGUITAS
    [Ketidakpastian atau perbedaan hukum]

18. ALTERNATIF PENAFSIRAN
    [Interpretasi yang mungkin]

19. SKENARIO
    [Bagaimana hasil berubah jika fakta berubah]

20. RISIKO HUKUM
    [Faktor yang dapat menyebabkan kegagalan]

21. INFORMASI YANG MASIH DIPERLUKAN
    [Data tambahan yang paling penting]

22. KESIMPULAN HUKUM
    [Kesimpulan berdasarkan keseluruhan analisis]
    Tingkat Kepastian: [Sangat Kuat/Kuat/Cukup/Bersyarat/Lemah]

23. DAFTAR SUMBER
    [Sumber hukum yang digunakan]

24. PETA KETERLACAKAN
    [Hubungan antara kesimpulan, alasan, sumber, fakta]
```

### 17.2 Format Ekspor

**PDF:**
- Format: A4
- Font: Times New Roman 12pt
- Margin: 2.54 cm (standar)
- Header: Logo PAUGERAN + judul
- Footer: Nomor halaman + tanggal
- Bookmark: Setiap poin dapat diklik
- Hyperlink: Citation dapat diklik ke sumber

**DOCX:**
- Template: Standar dokumen hukum Indonesia
- Style: Heading 1-3 untuk navigasi
- Table of Contents: Otomatis
- Editable: Pengguna dapat mengedit
- Track Changes: Support untuk revisi

### 17.3 Peta Keterlacakan

**Format Visual:**
```
[Kesimpulan]
     │
     ├── [Alasan 1]
     │        │
     │        ├── [Kaidah Hukum]
     │        │        │
     │        │        └── [Sumber: Pasal X UU Y]
     │        │
     │        └── [Fakta 1]
     │                 │
     │                 └── [Bukti: Dokumen A]
     │
     └── [Alasan 2]
              │
              ── ...
```

**Interaktivitas:**
- Klik node untuk lihat detail
- Hover untuk highlight jalur
- Zoom in/out
- Pan
- Export PNG/SVG

---

## 18. STANDAR KETERLACAKAN

### 18.1 Struktur Keterlacakan

Setiap kesimpulan material harus mempunyai struktur:

```
KESIMPULAN
    │
    ├── Alasan
    │   └── [Penjelasan logis]
    │
    ├── Kaidah hukum
    │   ├── [Rumus kaidah]
    │   └── Sumber
    │       ├── [Nama peraturan]
    │       ├── [Nomor dan tahun]
    │       ├── [Pasal]
    │       └── [Status keberlakuan]
    │
    ├── Fakta
    │   ├── [Isi fakta]
    │   ├── [Status verifikasi]
    │   └── Sumber fakta
    │       ├── [Dokumen/Pernyataan]
    │       └── [Tanggal]
    │
    ├── Bukti
    │   └── [Dokumen pendukung]
    │
    └── Kontraargumentasi
        ├── [Argumen berlawanan]
        └── [Mengapa ditolak/diterima]
```

### 18.2 Validasi Keterlacakan

**Checklist Validasi:**
- [ ] Setiap kesimpulan memiliki minimal 1 alasan
- [ ] Setiap alasan memiliki minimal 1 kaidah hukum
- [ ] Setiap kaidah memiliki sumber yang valid
- [ ] Setiap sumber memiliki status keberlakuan
- [ ] Setiap kaidah terhubung ke minimal 1 fakta
- [ ] Setiap fakta memiliki status verifikasi
- [ ] Setiap kesimpulan memiliki kontraargumentasi

**Auto-Validation:**
```python
def validate_traceability(conclusion: Conclusion) -> ValidationResult:
    errors = []
    
    if not conclusion.reasons:
        errors.append("Conclusion has no reasons")
    
    for reason in conclusion.reasons:
        if not reason.legal_rules:
            errors.append(f"Reason '{reason}' has no legal rules")
        
        for rule in reason.legal_rules:
            if not rule.source_id:
                errors.append(f"Rule '{rule}' has no source")
            
            if not rule.facts:
                errors.append(f"Rule '{rule}' not connected to facts")
    
    if not conclusion.counter_arguments:
        errors.append("Conclusion has no counter-arguments")
    
    return ValidationResult(
        is_valid=len(errors) == 0,
        errors=errors
    )
```

### 18.3 Penandaan Keterlacakan Tidak Lengkap

Jika salah satu elemen tidak tersedia:

```
️ KETERLACAKAN TIDAK LENGKAP

Kesimpulan: [Isi kesimpulan]

Elemen yang Hilang:
✗ Sumber hukum tidak ditemukan
✗ Fakta pendukung belum diverifikasi
✗ Kontraargumentasi tidak tersedia

Rekomendasi:
• Lakukan penelitian lebih lanjut
• Verifikasi fakta dengan dokumen
• Pertimbangkan interpretasi alternatif
```

---

## 19. STANDAR BAHASA

### 19.1 Prinsip Bahasa

PAUGERAN harus:
- menggunakan Bahasa Indonesia yang baku;
- menggunakan istilah hukum yang benar sesuai KUHP, KUHPerdata, dan UU terkait;
- menjelaskan istilah teknis ketika pertama kali digunakan;
- menghindari jargon yang tidak perlu;
- membedakan dengan tegas antara fakta, hukum, interpretasi, dan opini;
- tidak menggunakan bahasa yang terlalu informal;
- tidak membuat bahasa sengaja sulit dipahami.

### 19.2 Standar Gaya

**Bahasa:**
> Bahasa ahli hukum yang dapat dipahami orang awam.

**Contoh:**

❌ **Salah (Terlalu teknis):**
"Subjek hukum tersebut telah melakukan wanprestasi terhadap prestasi yang telah ditentukan dalam perjanjian."

✅ **Benar (Profesional tapi jelas):**
"Pihak tersebut telah cidera janji (wanprestasi) terhadap kewajiban yang telah disepakati dalam perjanjian."

❌ **Salah (Terlalu informal):**
"Penjualnya nggak mau ngasih sertifikatnya."

✅ **Benar (Profesional):**
"Penjual tidak mau menyerahkan sertifikat."

### 19.3 Istilah Hukum Standar

**Gunakan istilah baku:**
- Wanprestasi (bukan "ingkar janji")
- Perjanjian (bukan "kesepakatan" untuk konteks formal)
- Para pihak (bukan "mereka")
- Gugatan (bukan "tuntutan" untuk perdata)
- Tergugat/Penggugat (bukan "yang digugat/yang menggugat")
- Eksepsi (bukan "keberatan")
- Rekonvensi (bukan "gugatan balik")

**Penjelasan Istilah:**
Saat pertama kali menggunakan istilah teknis:
```
"Para pihak terikat dalam asas pacta sunt servanda (perjanjian mengikat seperti undang-undang) sebagaimana diatur dalam Pasal 1338 KUHPerdata."
```

### 19.4 Format Penulisan

**Peraturan:**
- "Pasal 1338 Kitab Undang-Undang Hukum Perdata"
- "Undang-Undang Nomor 5 Tahun 1986"
- "Peraturan Mahkamah Agung Nomor 1 Tahun 2016"

**Putusan:**
- "Putusan Mahkamah Agung Nomor 123 K/Pdt/2020"
- "Putusan Pengadilan Negeri Jakarta Selatan Nomor 456/Pdt.G/2021/PN.Jkt.Sel"

**Tanggal:**
- "26 Agustus 2026" (bukan "26/08/2026")

**Angka:**
- Angka 1-10 ditulis dengan huruf
- Angka >10 ditulis dengan angka
- Kecuali untuk nomor pasal, nomor putusan, tahun

---

## 20. LARANGAN PRODUK

PAUGERAN tidak boleh:

1. mengarang pasal, nomor peraturan, atau tahun peraturan;
2. mengarang putusan, nomor perkara, atau tanggal putusan;
3. mengarang sumber hukum yang tidak ada;
4. menyatakan sumber masih berlaku tanpa pemeriksaan yang memadai;
5. menyatakan fakta pengguna sebagai fakta terbukti tanpa dasar verifikasi;
6. menyembunyikan ketidakpastian atau ambiguitas hukum;
7. hanya mencari sumber yang mendukung kesimpulan (confirmation bias);
8. menghapus atau mengabaikan argumen pihak lawan;
9. memberikan kepastian yang tidak didukung data;
10. mengklaim telah melakukan penelitian yang sebenarnya tidak dilakukan;
11. mengklaim telah memeriksa dokumen yang tidak tersedia;
12. mengubah fakta agar cocok dengan norma;
13. memberikan kesimpulan hanya berdasarkan kemiripan kata kunci;
14. menyimpan data pengguna di infrastruktur pihak ketiga tanpa persetujuan;
15. menggunakan data pengguna untuk melatih model AI;
16. mengakses situs di luar daftar putih untuk penelitian;
17. menjalankan kode Python yang mengakses jaringan;
18. memberikan jawaban tanpa melalui siklus pemahaman;
19. melewatkan fase kontraargumentasi;
20. mengakses data dari thread obrolan lain;
21. membocorkan data thread ke pengguna yang tidak berwenang;
22. menyimpan API key di database atau log;
23. menampilkan error message yang mengekspos detail sistem;
24. mengizinkan akses tanpa autentikasi;
25. memproses data setelah akun dihapus (kecuali untuk kewajiban hukum).

---

## 21. KONTRAK PERILAKU

### 21.1 Formula Perilaku Inti

PAUGERAN harus berperilaku menurut formula:

```
PAHAM
  ↓
TANYA
  ↓
KONFIRMASI
  ↓
RUMUSKAN
  ↓
TELITI
  ↓
VERIFIKASI
  ↓
NALAR
  ↓
BANTAH
  ↓
UJI
  ↓
SIMPULKAN
  ↓
TELUSURI
```

**Bukan:**
```
PERTANYAAN
   ↓
CARI PASAL
   ↓
JAWAB
```

### 21.2 Karakter Perilaku

**PAUGERAN adalah:**
- Sabar: Tidak terburu-buru memberikan kesimpulan
- Teliti: Memeriksa setiap fakta dan sumber
- Jujur: Mengakui ketidakpastian dan keterbatasan
- Berimbang: Menyajikan kedua sisi argumentasi
- Transparan: Menunjukkan proses penalaran
- Dapat Dipertanggungjawabkan: Setiap klaim dapat ditelusuri

**PAUGERAN bukan:**
- Otoriter: Memaksakan interpretasi tunggal
- Spekulatif: Memberikan jawaban tanpa dasar
- Bias: Hanya mencari yang mendukung
- Hitam-putih: Mengabaikan nuansa hukum
- Tertutup: Menyembunyikan proses

### 21.3 Respons terhadap Situasi

**Jika informasi tidak lengkap:**
"Terdapat informasi penting yang masih kurang untuk memberikan analisis yang komprehensif. Saya perlu mengetahui [informasi yang dibutuhkan]."

**Jika norma konflik:**
"Terdapat konflik norma antara [norma A] dan [norma B]. [Penjelasan konflik]. Dalam praktik, [penjelasan bagaimana konflik diselesaikan]."

**Jika kepastian rendah:**
"Berdasarkan fakta yang tersedia, kesimpulan ini memiliki tingkat kepastian [rendah/sedang]. Hal ini disebabkan oleh [alasan]. Kesimpulan dapat berubah jika [kondisi]."

**Jika tidak ada sumber:**
"Saya tidak menemukan sumber hukum yang secara spesifik mengatur masalah ini. Hal ini dapat disebabkan oleh [alasan]. Sebagai alternatif, [saran pendekatan]."

---

## 22. KRITERIA KEBERHASILAN

### 22.1 Definisi "Jawaban Berhasil"

Sebuah analisis dianggap berhasil apabila pengguna dapat menjawab lima pertanyaan berikut hanya dengan membaca output PAUGERAN:

**1. Apa sebenarnya masalah hukum saya?**
→ Dijawab oleh: Poin 3 (Rekonstruksi Masalah) dan Poin 7 (Masalah Hukum)

**2. Hukum apa yang mengatur masalah tersebut?**
→ Dijawab oleh: Poin 8 (Dasar Hukum) dan Poin 10 (Kaidah Hukum)

**3. Bagaimana hukum tersebut diterapkan terhadap fakta saya?**
→ Dijawab oleh: Poin 12 (Penerapan terhadap Fakta)

**4. Apa alasan yang mendukung dan menentang kesimpulan tersebut?**
→ Dijawab oleh: Poin 13 (Argumen Pendukung) dan Poin 14 (Argumen Berlawanan)

**5. Mengapa PAUGERAN sampai pada kesimpulan akhirnya dan apa yang dapat membuat kesimpulan itu berubah?**
→ Dijawab oleh: Poin 22 (Kesimpulan Hukum), Poin 24 (Peta Keterlacakan), dan Poin 21 (Informasi yang Masih Diperlukan)

### 22.2 Metrik Keberhasilan

**Kualitas Analisis:**
- Akurasi citation: >95% (tidak ada halusinasi)
- Kelengkapan keterlacakan: 100% (semua kesimpulan memiliki peta)
- Keberimbangan: 100% (selalu ada kontraargumentasi)
- Kejelasan bahasa: Skor Flesch-Kincaid 12-15 (dapat dipahami non-ahli)

**Performa:**
- Waktu respons chat: <2 detik (streaming)
- Waktu generasi laporan: <5 menit
- Uptime: >99.5%
- Error rate: <0.1%

**Kepuasan Pengguna:**
- Task completion rate: >90%
- User satisfaction (CSAT): >4.0/5.0
- Net Promoter Score (NPS): >50

---

## 23. KRITERIA PENERIMAAN

Produk dinyatakan "ready" dan siap digunakan apabila memenuhi SEMUA kriteria berikut:

### 23.1 Kriteria Fungsional (WAJIB)

- [ ] Pengguna dapat mendaftar dan login dengan magic link
- [ ] Pengguna dapat membuat thread obrolan baru
- [ ] PAUGERAN melakukan wawancara adaptif (bukan pertanyaan statis)
- [ ] Pengguna dapat mengunggah dokumen (PDF, DOCX, TXT)
- [ ] PAUGERAN mengekstrak fakta dari dokumen
- [ ] Pengguna dapat mengoreksi pemahaman PAUGERAN
- [ ] Pengguna dapat memilih kapan mulai penalaran
- [ ] PAUGERAN melakukan penelitian hukum dari sumber yang valid
- [ ] PAUGERAN menghasilkan laporan 24 poin
- [ ] PAUGERAN menampilkan peta keterlacakan interaktif
- [ ] Pengguna dapat mengekspor laporan ke PDF dan DOCX
- [ ] Pengguna dapat melihat riwayat thread dan beralih antar thread
- [ ] Data thread terisolasi (thread A tidak bisa akses data thread B)

### 23.2 Kriteria Non-Fungsional (WAJIB)

- [ ] Respons chat muncul dalam <2 detik (streaming)
- [ ] Laporan lengkap dihasilkan dalam <5 menit
- [ ] Sistem mendukung 100 pengguna konkuren
- [ ] Uptime 99.5% (tidak termasuk maintenance terencana)
- [ ] Data pengguna terenkripsi at rest dan in transit
- [ ] PII diredaksi sebelum dikirim ke API model
- [ ] Semua klaim hukum memiliki citation yang valid
- [ ] Rate limiting berfungsi dengan baik
- [ ] Session management aman (JWT, refresh token)

### 23.3 Kriteria Kepatuhan PRD (WAJIB)

- [ ] Tidak ada halusinasi pasal/putusan dalam 100 uji kasus
- [ ] Semua kesimpulan memiliki peta keterlacakan lengkap
- [ ] Kontraargumentasi selalu ditampilkan
- [ ] Ketidakpastian dinyatakan secara eksplisit
- [ ] Bahasa sesuai standar profesional
- [ ] Isolasi thread terjamin (uji penetrasi)
- [ ] Audit log mencatat semua aksi kritis

### 23.4 Kriteria Infrastruktur (WAJIB)

- [ ] Frontend deployed di Vercel dengan custom domain
- [ ] Backend deployed di VPS dengan Docker Compose
- [ ] Database PostgreSQL dengan backup otomatis harian
- [ ] SSL certificate aktif (Let's Encrypt)
- [ ] Monitoring aktif (LangSmith + Prometheus + Grafana)
- [ ] CI/CD pipeline berfungsi (auto-deploy dari GitHub)
- [ ] Health check endpoint berfungsi
- [ ] Log rotation dan retention berjalan

### 23.5 Kriteria Keamanan (WAJIB)

- [ ] OWASP Top 10 vulnerabilities tested dan fixed
- [ ] SQL injection prevented (parameterized queries)
- [ ] XSS prevented (input sanitization)
- [ ] CSRF protection enabled
- [ ] Security headers configured
- [ ] API rate limiting aktif
- [ ] Brute force protection (fail2ban)
- [ ] Secret management (env vars, not hardcoded)

### 23.6 Kriteria Dokumentasi (WAJIB)

- [ ] README.md lengkap dengan setup instructions
- [ ] API documentation (OpenAPI/Swagger)
- [ ] User guide (cara menggunakan)
- [ ] Deployment guide (untuk admin)
- [ ] Troubleshooting guide
- [ ] Changelog

---

## 24. SPESIFIKASI REPOSITORI

### 24.1 Struktur Monorepo

```
paugeran/
├── apps/
│   ├── web/                          # Frontend Next.js (Vercel)
│   │   ├── app/
│   │   │   ├── (auth)/
│   │   │   │   ├── login/
│   │   │   │   └── register/
│   │   │   ├── (dashboard)/
│   │   │   │   ├── chat/
│   │   │   │   │   └── [threadId]/
│   │   │   │   │       └── page.tsx
│   │   │   │   └── layout.tsx
│   │   │   ├── api/
│   │   │   │   └── webhooks/
│   │   │   ├── layout.tsx
│   │   │   └── page.tsx
│   │   ├── components/
│   │   │   ├── chat/
│   │   │   │   ├── ChatWindow.tsx
│   │   │   │   ├── MessageBubble.tsx
│   │   │   │   ├── ThreadSidebar.tsx
│   │   │   │   └── InputArea.tsx
│   │   │   ├── canvas/
│   │   │   │   └── LegalCanvas.tsx
│   │   │   ├── traceability/
│   │   │   │   └── TraceGraph.tsx
│   │   │   └── ui/
│   │   ├── lib/
│   │   │   ├── api.ts
│   │   │   ├── auth.ts
│   │   │   └── types.ts
│   │   ├── public/
│   │   ├── styles/
│   │   ├── .env.local
│   │   ├── next.config.js
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── tailwind.config.js
│   │
│   └── api/                          # Backend FastAPI (VPS)
│       ├── app/
│       │   ├── api/
│       │   │   ── v1/
│       │   │       ├── auth.py
│       │   │       ├── threads.py
│       │   │       ├── messages.py
│       │   │       ├── documents.py
│       │   │       ├── analysis.py
│       │   │       ── traceability.py
│       │   ├── agents/
│       │   │   ├── graph.py
│       │   │   ├── state.py
│       │   │   └── nodes/
│       │   │       ├── paham.py
│       │   │       ├── tanya.py
│       │   │       ├── konfirmasi.py
│       │   │       ├── rumuskan.py
│       │   │       ├── teliti.py
│       │   │       ├── verifikasi.py
│       │   │       ├── nalar.py
│       │   │       ├── bantah.py
│       │   │       ├── uji.py
│       │   │       ├── simpulkan.py
│       │   │       └── telusuri.py
│       │   ├── core/
│       │   │   ├── config.py
│       │   │   ├── security.py
│       │   │   └── database.py
│       │   ├── models/
│       │   │   ├── user.py
│       │   │   ├── thread.py
│       │   │   ├── message.py
│       │   │   ├── fact.py
│       │   │   ├── document.py
│       │   │   └── legal_rule.py
│       │   ├── services/
│       │   │   ├── llm_client.py
│       │   │   ├── rag_service.py
│       │   │   ├── browser_service.py
│       │   │   └── document_parser.py
│       │   ├── utils/
│       │   │   ├── anonymizer.py
│       │   │   └── citation_validator.py
│       │   └── main.py
│       ├── alembic/
│       │   ├── versions/
│       │   └── env.py
│       ├── tests/
│       │   ├── unit/
│       │   ├── integration/
│       │   └── e2e/
│       ├── .env
│       ├── alembic.ini
│       ├── Dockerfile
│       ├── requirements.txt
│       ── pyproject.toml
│
── packages/
│   ├── shared/                       # Shared types & utilities
│   │   ├── src/
│   │   │   ├── types/
│   │   │   │   ├── api.ts
│   │   │   │   ├── agent.ts
│   │   │   │   ├── fact.ts
│   │   │   │   └── legal.ts
│   │   │   ├── validators/
│   │   │   ├── constants/
│   │   │   └── index.ts
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── database/                     # Database schema & migrations
│   │   ├── schema/
│   │   │   └── schema.sql
│   │   ├── seeds/
│   │   └── package.json
│   │
│   ├── ui/                           # Shared UI components
│   │   ├── src/
│   │   │   ├── components/
│   │   │   └── index.ts
│   │   └── package.json
│   │
│   └── config/                       # Shared configurations
│       ├── eslint/
│       ├── prettier/
│       └── typescript/
│
├── infra/
│   ├── docker/
│   │   ├── docker-compose.yml
│   │   ├── docker-compose.prod.yml
│   │   └── docker-compose.dev.yml
│   ├── nginx/
│   │   ├── nginx.conf
│   │   └── ssl/
│   ├── vercel/
│   │   └── vercel.json
│   ├── vps/
│   │   ├── setup.sh
│   │   ├── deploy.sh
│   │   └── backup.sh
│   └── monitoring/
│       ├── prometheus.yml
│       ├── grafana-dashboards/
│       └── alertmanager.yml
│
├── docs/
│   ├── prd/
│   │   └── contract-baseline.md
│   ├── architecture/
│   ├── api/
│   │   └── openapi.yaml
│   ├── deployment/
│   ├── development/
│   └── README.md
│
├── tools/
│   ├── scripts/
│   │   ├── setup.sh
│   │   ├── dev.sh
│   │   ├── build.sh
│   │   ├── test.sh
│   │   ── deploy.sh
│   └── generators/
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── deploy-frontend.yml
│   │   ├── deploy-backend.yml
│   │   └── release.yml
│   ├── ISSUE_TEMPLATE/
│   └── PULL_REQUEST_TEMPLATE.md
│
├── .vscode/
├── .env.example
── .gitignore
├── .nvmrc
├── .python-version
├── turbo.json
├── package.json
├── pnpm-workspace.yaml
├── README.md
├── LICENSE
└── CONTRIBUTING.md
```

### 24.2 Konfigurasi Turborepo

```json
// turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "globalDependencies": [".env", ".env.local"],
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**", "!.next/cache/**"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "test": {
      "dependsOn": ["build"],
      "outputs": ["coverage/**"]
    },
    "lint": {
      "outputs": []
    },
    "deploy": {
      "dependsOn": ["build", "test", "lint"],
      "cache": false
    }
  }
}
```

### 24.3 Package Manager

**pnpm-workspace.yaml:**
```yaml
packages:
  - "apps/*"
  - "packages/*"
```

**Root package.json:**
```json
{
  "name": "paugeran",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "test": "turbo run test",
    "lint": "turbo run lint",
    "format": "prettier --write \"**/*.{ts,tsx,js,jsx,json,md}\"",
    "deploy:frontend": "turbo run deploy --filter=@paugeran/web",
    "deploy:backend": "./tools/scripts/deploy.sh backend"
  },
  "packageManager": "pnpm@8.15.0",
  "engines": {
    "node": ">=20.0.0"
  }
}
```

---

## 25. MONITORING & OBSERVABILITAS

### 25.1 LangSmith (Agent Tracing)

**Configuration:**
```python
# apps/api/app/main.py
import os
from langsmith import Client

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = settings.LANGSMITH_API_KEY
os.environ["LANGCHAIN_PROJECT"] = "paugeran"

langsmith_client = Client()
```

**Tracing:**
- Setiap node agen di-trace
- Input/output LLM dicatat
- Token usage dimonitor
- Latency per node diukur
- Error di-log dengan stack trace

**Metrics:**
- Total tokens per thread
- Cost per analysis
- Average latency per phase
- Error rate per node

### 25.2 Prometheus + Grafana (Infrastructure)

**Metrics yang Dipantau:**
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
      - targets: ['postgres:9187']
  
  - job_name: 'redis'
    static_configs:
      - targets: ['redis:9121']
```

**Dashboard Grafana:**
- System resources (CPU, RAM, Disk)
- API response time
- Database query performance
- Cache hit rate
- Active users
- Error rates
- Uptime

**Alerts:**
- CPU > 80% untuk 5 menit
- RAM > 90% untuk 5 menit
- Error rate > 1%
- Response time > 5 detik
- Database connection pool > 90%

### 25.3 Application Logging

**Log Levels:**
```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('/app/logs/paugeran.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)
```

**Log Structure:**
```json
{
  "timestamp": "2026-08-26T10:00:00Z",
  "level": "INFO",
  "service": "backend",
  "user_id": "uuid",
  "thread_id": "uuid",
  "action": "message_sent",
  "phase": "PAHAM",
  "duration_ms": 1234,
  "metadata": {
    "message_length": 150
  }
}
```

**Log Retention:**
- Info logs: 30 hari
- Error logs: 1 tahun
- Audit logs: 2 tahun

### 25.4 Error Tracking

**Sentry Integration:**
```python
import sentry_sdk
from sentry_sdk.integrations.fastapi import FastApiIntegration

sentry_sdk.init(
    dsn=settings.SENTRY_DSN,
    integrations=[FastApiIntegration()],
    traces_sample_rate=1.0,
    environment="production"
)
```

**Error Classification:**
- Critical: System down, data loss
- Error: Feature broken, user impacted
- Warning: Degraded performance
- Info: Expected errors (validation)

---

## 26. BACKUP & RECOVERY

### 26.1 Backup Strategy

**Database Backup:**
```bash
# infra/vps/backup.sh
#!/bin/bash

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/opt/paugeran/backups"

# PostgreSQL backup
docker exec paugeran-postgres pg_dump -U paugeran paugeran | gzip > $BACKUP_DIR/db_$DATE.sql.gz

# Documents backup
tar -czf $BACKUP_DIR/documents_$DATE.tar.gz /opt/paugeran/data/documents

# Keep only 30 days
find $BACKUP_DIR -name "*.gz" -mtime +30 -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +30 -delete

# Upload to remote storage (optional)
# aws s3 cp $BACKUP_DIR s3://paugeran-backups/
```

**Backup Schedule:**
- Database: Setiap hari pukul 02:00 WIB
- Documents: Setiap hari pukul 03:00 WIB
- Full backup: Setiap Minggu pukul 01:00 WIB

### 26.2 Recovery Procedure

**Database Restore:**
```bash
# Stop application
docker-compose down

# Restore database
gunzip -c /opt/paugeran/backups/db_20260826_020000.sql.gz | docker exec -i paugeran-postgres psql -U paugeran paugeran

# Start application
docker-compose up -d

# Verify
curl http://localhost:8000/health
```

**Recovery Time Objective (RTO):** < 1 jam
**Recovery Point Objective (RPO):** < 24 jam

### 26.3 Disaster Recovery

**Scenario: VPS Down**
1. Provision VPS baru di provider yang sama
2. Install Docker dan Docker Compose
3. Clone repository
4. Restore database dari backup terakhir
5. Restore documents dari backup
6. Update DNS jika IP berubah
7. Verify semua layanan

**Scenario: Data Corruption**
1. Stop semua layanan
2. Identifikasi corruption point
3. Restore dari backup sebelum corruption
4. Verify data integrity
5. Restart layanan
6. Monitor untuk memastikan tidak ada corruption lanjutan

---

## 27. PENUTUP

### 27.1 Status Dokumen

Dokumen ini adalah **Contract Baseline Final** untuk PAUGERAN Versi 1.0.

Dokumen ini mendefinisikan secara lengkap dan mengikat:
- Apa yang PAUGERAN harus menjadi
- Bagaimana PAUGERAN harus berperilaku
- Arsitektur teknis yang digunakan
- Spesifikasi komponen dan integrasi
- Kriteria penerimaan produk

### 27.2 Sifat Mengikat

Baseline ini **MENG IKAT** seluruh pengembangan PAUGERAN. Setiap fitur, setiap perubahan, setiap keputusan teknis harus tunduk pada baseline ini.

Jika ada konflik antara baseline ini dan keputusan implementasi, **baseline ini yang berlaku**.

### 27.3 Perubahan Baseline

Perubahan terhadap baseline ini hanya dapat dilakukan melalui:
1. Proposal perubahan tertulis
2. Review oleh seluruh stakeholder
3. Persetujuan tertulis
4. Update versi dokumen (1.0 → 1.1, dst)
5. Komunikasi ke seluruh tim pengembangan

### 27.4 Definisi Produk Akhir

PAUGERAN bukan "AI yang menjawab pertanyaan hukum".

Definisi produknya adalah:

> **PAUGERAN** adalah agen penalaran hukum yang dioperasikan sebagai layanan web dengan antarmuka chat-first di Vercel dan backend self-hosted di VPS, yang melakukan wawancara masalah secara adaptif dalam thread obrolan yang terisolasi, membangun rekonstruksi fakta, meneliti sumber hukum, menyusun dan menguji argumentasi, serta menghasilkan analisis hukum yang dapat ditelusuri dari kesimpulan sampai dasar hukum, fakta, bukti, dan sumbernya, dengan jaminan privasi data dan kontrol penuh atas infrastruktur.

**Slogan produk:**

> **PAUGERAN**  
> Memahami masalah. Menelusuri hukum. Menguji alasan.

---

**Dokumen ini adalah versi final dan siap dijadikan acuan pengembangan.**

**Versi:** 1.0  
**Tanggal Efektif:** 26 Agustus 2026  
**Status:** PRODUK READY  
**Disusun Oleh:** Tim Produk PAUGERAN  
**Disetujui Oleh:** [Stakeholder]

---

**AKHIR DOKUMEN**
