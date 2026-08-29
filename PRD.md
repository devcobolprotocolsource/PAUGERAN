# 📜 PAUGERAN — PRODUCT REQUIREMENTS DOCUMENT (PRD)
## Master Specification — End-to-End

**Dokumen:** PRD Master Specification  
**Produk:** PAUGERAN — Agen Penalaran Hukum Indonesia  
**Status:** FINAL — SUMBER KEBENARAN MUTLAK  
**Versi:** 1.0  
**Sifat:** Kontrak produk yang mengikat seluruh aspek pengembangan, deployment, dan operasional  
**Kedudukan:** Sumber kebenaran tunggal (single source of truth) untuk seluruh proyek PAUGERAN

---

## 📋 DAFTAR ISI

**BAGIAN I — IDENTITAS & VISI PRODUK**
1. Definisi Produk
2. Visi & Misi
3. Tujuan Produk
4. Masalah yang Diselesaikan
5. Prinsip Produk
6. Aktor & Stakeholder

**BAGIAN II — SPESIFIKASI FUNGSIONAL LENGKAP**
7. Manajemen Sesi Obrolan
8. Dialog Adaptif & Pemahaman Masalah
9. Penelitian Hukum Multi-Sumber
10. Penalaran & Argumentasi
11. Keterlacakan & Verifikasi
12. Legal Knowledge Base
13. Penelitian Web
14. Multi-Provider LLM
15. Sistem Lisensi
16. Autentikasi & Otorisasi
17. Kustomisasi Antarmuka
18. Aksesibilitas
19. Export Dokumen Profesional

**BAGIAN III — SPESIFIKASI TEKNIS**
20. Arsitektur Sistem
21. Stack Teknologi
22. Custom Graph Engine
23. Database Schema
24. API Specification
25. Frontend Specification

**BAGIAN IV — SPESIFIKASI PERILAKU AGEN**
26. Siklus 11 Fase
27. Detail Setiap Fase
28. Kontrak Perilaku
29. Standar Bahasa
30. Standar Keterlacakan

**BAGIAN V — SPESIFIKASI ANTARMUKA**
31. Layout & Navigasi
32. Komponen UI
33. Design System
34. Responsivitas
35. Keyboard Shortcuts & Command Palette

**BAGIAN VI — SPESIFIKASI DEPLOYMENT & OPERASIONAL**
36. Model Distribusi
37. Deployment Scenarios
38. Environment Variables
39. Backup & Recovery
40. Monitoring & Observabilitas

**BAGIAN VII — SPESIFIKASI KEAMANAN & COMPLIANCE**
41. Keamanan Data
42. Enkripsi & Privacy
43. Compliance Hukum
44. Audit Trail

**BAGIAN VIII — KRITERIA PENERIMAAN & TESTING**
45. Kriteria Keberhasilan
46. Kriteria Penerimaan
47. Testing Strategy
48. Quality Gates

**BAGIAN IX — TATA KELOLA & DOKUMEN TURUNAN**
49. Larangan Produk
50. Dokumen Turunan
51. Perubahan & Versi
52. Penutup

---

# BAGIAN I — IDENTITAS & VISI PRODUK

---

## 1. DEFINISI PRODUK

### 1.1 Apa Itu PAUGERAN

PAUGERAN adalah agen kecerdasan buatan (AI) untuk pemahaman masalah, penelitian, dan penalaran hukum Indonesia yang dioperasikan sebagai **satu binary universal berbasis Rust** yang menjalankan server HTTP.

### 1.2 Karakteristik Utama

**1.2.1** PAUGERAN menerima uraian masalah dari pengguna melalui antarmuka web, kemudian secara bertahap membangun pemahaman terhadap persoalan tersebut melalui dialog adaptif dalam satu sesi obrolan yang terisolasi.

**1.2.2** PAUGERAN mengidentifikasi fakta dan kekurangan informasi, merumuskan masalah hukum, melakukan penelitian terhadap sumber hukum yang relevan — baik dari basis pengetahuan internal (Legal Knowledge Base) maupun dari situs web resmi pemerintah dan sumber tepercaya lainnya.

**1.2.3** PAUGERAN menguji berbagai argumentasi dan penafsiran, kemudian menghasilkan analisis hukum yang memiliki keterlacakan penuh antara kesimpulan, fakta, kaidah hukum, sumber hukum, dan penalarannya.

### 1.3 Apa yang BUKAN PAUGERAN

**1.3.1** PAUGERAN **bukan** mesin pencari pasal yang hanya mengembalikan teks peraturan.

**1.3.2** PAUGERAN **bukan** mesin pemberi jawaban "benar/salah" tanpa penjelasan.

**1.3.3** PAUGERAN **bukan** chatbot umum yang memberikan respons instan tanpa penalaran terstruktur.

**1.3.4** PAUGERAN **bukan** pengganti advokat atau konsultan hukum profesional.

### 1.4 Kemampuan Penjelasan

PAUGERAN harus mampu menjelaskan:
- Apa kesimpulannya
- Mengapa kesimpulan tersebut muncul
- Dasar apa yang digunakan
- Fakta apa yang mendukungnya
- Apa kelemahannya
- Apa yang dapat membuat kesimpulan tersebut berubah

### 1.5 Model Operasi

**1.5.1** PAUGERAN dioperasikan sebagai produk siap pakai berupa satu binary universal.

**1.5.2** Binary ini menyajikan antarmuka web modern dan mesin penalaran hukum dalam satu kesatuan yang tidak terpisahkan.

**1.5.3** Binary ini dapat dijalankan di laptop pribadi, server cloud terkelola (Railway), VPS pribadi, atau homelab tanpa modifikasi kode.

**1.5.4** PAUGERAN tidak memiliki mode desktop terpisah dan tidak memiliki mode cloud terpisah. Satu binary, satu cara kerja, banyak cara deployment.

**1.5.5** Perbedaan deployment hanya ditentukan oleh environment variables saat startup.

### 1.6 Autentikasi

**1.6.1** Autentikasi multi-user adalah fitur opsional yang diaktifkan melalui environment variable `AUTH_ENABLED=true`.

**1.6.2** Secara default, PAUGERAN berjalan tanpa autentikasi untuk penggunaan pribadi.

### 1.7 Dukungan Model LLM

**1.7.1** PAUGERAN mendukung berbagai penyedia dan model LLM — tidak terbatas pada model ternama.

**1.7.2** PAUGERAN dapat menggunakan model dari Anthropic, OpenAI, Groq, Together AI, Fireworks, OpenRouter, Mistral AI, DeepSeek, Ollama (lokal), LM Studio (lokal), vLLM (lokal), dan penyedia lain yang kompatibel dengan API OpenAI atau Anthropic.

### 1.8 Sistem Lisensi

**1.8.1** PAUGERAN menggunakan sistem lisensi untuk mengontrol penggunaan produk.

**1.8.2** Lisensi divalidasi secara berkala saat PAUGERAN terhubung ke internet.

**1.8.3** Lisensi harus aktif agar agen dapat digunakan, terpisah dari konfigurasi API key LLM.

### 1.9 Legal Knowledge Base

**1.9.1** PAUGERAN memiliki Legal Knowledge Base — basis pengetahuan hukum internal yang dibangun dari peraturan, pasal, dan putusan yang pernah diteliti.

**1.9.2** Basis ini dapat digunakan sebagai referensi untuk analisis di masa depan tanpa perlu melakukan penelitian ulang dari internet.

---

## 2. VISI & MISI

### 2.1 Visi

Menjadi standar baru alat bantu penalaran hukum di Indonesia yang mengutamakan kejujuran intelektual, keterlacakan, privasi data, dan aksesibilitas di atas kecepatan dan kemudahan semu.

### 2.2 Misi

Memberikan kepada advokat, legal in-house, akademisi, dan masyarakat umum sebuah agen AI yang:
- Memahami masalah sebelum menyimpulkan
- Meneliti sumber hukum yang valid dari basis pengetahuan internal dan internet
- Menyajikan argumen berimbang dengan kontraargumentasi
- Menjaga kerahasiaan data klien
- Menghasilkan analisis yang dapat dipertanggungjawabkan secara profesional
- Dapat diakses oleh pengguna dengan berbagai kemampuan
- Dapat di-deploy dengan mudah di berbagai environment

---

## 3. TUJUAN PRODUK

PAUGERAN harus memungkinkan pengguna untuk:

### 3.1 Instalasi & Akses

**3.1.1** Menjalankan produk dengan satu perintah (binary langsung, `docker run`, atau klik deploy di Railway).

**3.1.2** Mengakses produk dari browser mana saja pada deployment cloud atau VPS.

**3.1.3** Menggunakan produk tanpa keahlian teknis untuk instalasi.

### 3.2 Konfigurasi

**3.3.1** Mengaktivasi produk dengan lisensi key yang valid pada penggunaan pertama.

**3.3.2** Menggunakan produk tanpa harus memasukkan lisensi key berulang kali selama produk terhubung ke internet secara berkala.

**3.3.3** Memasukkan API key LLM melalui antarmuka web tanpa menyentuh terminal.

**3.3.4** Memilih penyedia dan model LLM dari berbagai pilihan yang didukung.

**3.3.5** Mengganti atau menghapus API key dan provider kapan saja melalui pengaturan.

### 3.3 Manajemen Sesi

**3.3.1** Membuat sesi obrolan baru kapan saja untuk topik hukum yang berbeda.

**3.3.2** Mengelola banyak sesi obrolan untuk kasus yang berbeda.

**3.3.3** Membuka kembali sesi obrolan lama kapan saja tanpa batas waktu.

**3.3.4** Menghapus sesi obrolan kapan saja secara permanen.

### 3.4 Dialog & Pemahaman

**3.4.1** Menjelaskan masalah menggunakan bahasa natural dalam antarmuka chat.

**3.4.2** Mendapatkan pertanyaan klarifikasi yang relevan dan adaptif.

**3.4.3** Membangun pemahaman masalah secara bertahap dalam satu sesi obrolan.

**3.4.4** Mengoreksi pemahaman PAUGERAN melalui dialog.

**3.4.5** Menentukan kapan proses pemahaman dianggap cukup.

### 3.5 Penelitian & Penalaran

**3.5.1** Meminta PAUGERAN melakukan penalaran hukum.

**3.5.2** Memperoleh penelitian hukum dari basis pengetahuan internal dan dari situs web resmi pemerintah serta sumber tepercaya lainnya di internet.

**3.5.3** Menyimpan peraturan, pasal, PP, dan sejenisnya yang pernah diteliti ke dalam Legal Knowledge Base untuk digunakan sebagai referensi di sesi berikutnya.

**3.5.4** Mengelola Legal Knowledge Base: menambah, memperbarui, menandai sebagai tidak berlaku, dan menghapus entri.

### 3.6 Output & Analisis

**3.6.1** Mengetahui dasar hukum yang digunakan dengan inline citation yang detail pada setiap bagian output.

**3.6.2** Memahami penerapan hukum terhadap fakta.

**3.6.3** Melihat argumentasi yang mendukung dan berlawanan.

**3.6.4** Mengetahui ketidakpastian dan kelemahan analisis.

**3.6.5** Melihat alternatif penafsiran.

**3.6.6** Menelusuri setiap kesimpulan menuju dasar dan sumbernya melalui peta keterlacakan.

**3.6.7** Memperoleh laporan hukum dalam Bahasa Indonesia yang profesional dan mudah dipahami.

### 3.7 Data & Ekspor

**3.7.1** Menyimpan seluruh data kasus secara privat di penyimpanan yang dikendalikan pengguna.

**3.7.2** Mengekspor hasil analisis dalam format PDF profesional dan DOCX profesional dengan template yang dapat dikustomisasi.

**3.7.3** Mengunggah dokumen pendukung dalam format PDF, DOCX, dan TXT ke dalam sesi obrolan.

**3.7.4** Mem-backup dan merestore seluruh data dengan mudah.

### 3.8 Kustomisasi & Aksesibilitas

**3.8.1** Mengustomisasi antarmuka pengguna meliputi tema warna, ukuran font, tata letak, dan bahasa sesuai preferensi pribadi.

**3.8.2** Menyimpan preferensi kustomisasi secara persisten.

**3.8.3** Menggunakan PAUGERAN dengan aksesibilitas tinggi melalui keyboard shortcuts, command palette, screen reader support, dan mode aksesibilitas.

### 3.9 Manajemen Tim (jika AUTH_ENABLED=true)

**3.9.1** Mengelola anggota tim melalui sistem undangan.

**3.9.2** Menyediakan API key global untuk kenyamanan tim.

**3.9.3** Memantau penggunaan tim melalui statistik agregat.

---

## 4. MASALAH YANG DISELESAIKAN

### 4.1 Masalah Sistem Hukum

**4.1.1** Fakta pengguna sering tidak lengkap.

**4.1.2** Istilah yang digunakan pengguna belum tentu merupakan istilah hukum yang tepat.

**4.1.3** Satu fakta dapat memiliki beberapa konsekuensi hukum.

**4.1.4** Satu masalah dapat melibatkan beberapa bidang hukum.

**4.1.5** Aturan hukum dapat berubah, memiliki pengecualian, dan bertentangan satu sama lain.

**4.1.6** Putusan pengadilan dapat memiliki fakta yang berbeda meskipun kasusnya tampak serupa.

**4.1.7** Suatu norma dapat memiliki beberapa interpretasi yang sah.

**4.1.8** Kekuatan suatu kesimpulan bergantung pada fakta dan bukti yang tersedia.

**4.1.9** Informasi hukum di internet memiliki tingkat keandalan yang berbeda.

### 4.2 Masalah Data & Privasi

**4.2.1** Data hukum bersifat sensitif dan harus dijaga kerahasiaannya.

**4.2.2** Advokat membutuhkan alat yang dapat dipertanggungjawabkan secara profesional.

**4.2.3** Pengguna membutuhkan isolasi data antar kasus yang berbeda.

**4.2.4** Pengguna tidak ingin API key mereka disimpan di server pihak ketiga.

**4.2.5** Pengguna tidak ingin membuat akun untuk alat pribadi.

### 4.3 Masalah Teknis

**4.3.1** Instalasi produk AI hukum umumnya rumit dan membutuhkan keahlian teknis.

**4.3.2** Solusi berbasis Python memiliki overhead performa yang signifikan untuk agen yang berjalan lokal.

**4.3.3** Pengguna memiliki preferensi visual dan aksesibilitas yang berbeda.

**4.3.4** Firma hukum membutuhkan alat yang dapat digunakan bersama oleh tim dengan kontrol biaya terpusat.

**4.3.5** Profesional hukum membutuhkan deployment yang fleksibel, baik lokal untuk privasi maupun cloud untuk kolaborasi.

### 4.4 Masalah Model AI

**4.4.1** Model AI ternama seringkali mahal atau tidak tersedia di wilayah tertentu.

**4.4.2** Pengguna membutuhkan fleksibilitas untuk memilih model yang sesuai dengan kebutuhan, ketersediaan, dan biaya.

### 4.5 Masalah Efisiensi

**4.5.1** Penelitian hukum yang sama sering diulang dari nol padahal peraturan yang sama telah diteliti sebelumnya.

**4.5.2** Output AI hukum seringkali tidak mencantumkan sumber secara detail, menyulitkan verifikasi profesional.

**4.5.3** Laporan hukum yang dihasilkan AI seringkali tidak siap digunakan dalam konteks profesional tanpa formatting ulang.

### 4.6 Masalah Lisensi

**4.6.1** Produk AI hukum perlu memiliki mekanisme lisensi yang jelas untuk penggunaan komersial yang berkelanjutan.

---

## 5. PRINSIP PRODUK

Prinsip-prinsip berikut bersifat mengikat dan tidak dapat dikompromikan.

### 5.1 Prinsip Penalaran

**P-01 — Pemahaman sebelum kesimpulan**
PAUGERAN tidak boleh langsung memberikan kesimpulan hukum mendalam apabila informasi material belum memadai.

**P-02 — Dialog adaptif**
Pertanyaan lanjutan harus dihasilkan berdasarkan informasi yang telah diperoleh dalam sesi obrolan. PAUGERAN tidak boleh menggunakan daftar pertanyaan statis sebagai satu-satunya mekanisme wawancara.

**P-03 — Pengguna dapat mengoreksi**
PAUGERAN harus menyajikan pemahaman sementara dan memberikan kesempatan kepada pengguna untuk memperbaikinya dalam sesi yang sama.

**P-04 — Fakta bukan asumsi**
Pernyataan pengguna harus dibedakan dari fakta yang telah diverifikasi melalui dokumen atau sumber lain.

**P-05 — Sumber adalah bagian dari penalaran**
Sumber hukum bukan sekadar daftar referensi di bagian akhir. Setiap sumber harus mempunyai hubungan dengan klaim atau bagian analisis yang menggunakannya.

**P-06 — Kesimpulan harus dapat ditelusuri**
Setiap kesimpulan material harus dapat ditelusuri melalui rantai: Kesimpulan → alasan → kaidah → sumber hukum → fakta → bukti atau sumber fakta.

**P-07 — Penalaran harus berimbang**
PAUGERAN wajib mencari argumentasi yang dapat melemahkan kesimpulannya sendiri.

**P-08 — Ketidakpastian harus terlihat**
PAUGERAN tidak boleh menyembunyikan informasi yang kurang, fakta yang belum diverifikasi, konflik norma, konflik putusan, ketidakpastian interpretasi, atau keterbatasan sumber.

**P-09 — Tidak memalsukan kepastian**
Jika kesimpulan tidak dapat ditentukan secara pasti, PAUGERAN harus menyatakan kondisi tersebut secara eksplisit.

**P-10 — Bahasa profesional dan mudah dipahami**
Bahasa keluaran harus memenuhi standar komunikasi hukum profesional Indonesia tanpa sengaja dibuat rumit.

### 5.2 Prinsip Privasi & Data

**P-11 — Privasi dan isolasi sesi**
Data dalam satu sesi obrolan tidak boleh bocor ke sesi lain. Setiap sesi adalah entitas terisolasi. Data pengguna tidak boleh bocor ke pihak ketiga.

**P-12 — Data 100% lokal atau terkelola**
Seluruh data pengguna tersimpan di penyimpanan yang dikendalikan pengguna. Tidak ada replikasi ke pihak ketiga tanpa persetujuan eksplisit.

**P-13 — API key adalah milik pengguna**
API key LLM disimpan terenkripsi di penyimpanan pengguna. API key tidak pernah dikirim ke server PAUGERAN. Pengguna memiliki kontrol penuh.

### 5.3 Prinsip Produk

**P-14 — Produk harus siap pakai**
PAUGERAN harus dapat digunakan segera setelah instalasi tanpa konfigurasi tambahan yang rumit oleh pengguna akhir.

**P-15 — Chat-first experience**
Antarmuka utama adalah chat yang intuitif. Fitur kompleks seperti peta keterlacakan dan laporan harus dapat diakses dari dalam chat tanpa meninggalkan konteks obrolan.

**P-16 — Instalasi satu langkah**
Pengguna harus dapat menjalankan PAUGERAN dengan satu perintah. Tidak perlu setup database terpisah, tidak perlu konfigurasi server, tidak perlu keahlian teknis.

**P-17 — Autentikasi opsional**
Autentikasi multi-user adalah fitur opsional yang diaktifkan melalui `AUTH_ENABLED=true`. Secara default, PAUGERAN berjalan tanpa autentikasi untuk kemudahan penggunaan pribadi.

**P-18 — Sesi adalah entitas independen**
Setiap sesi obrolan berdiri sendiri. Sesi A tidak mengetahui keberadaan sesi B. Satu-satunya data yang bersifat global adalah API key, preferensi UI per pengguna, Legal Knowledge Base, dan lisensi.

**P-19 — Kustomisasi adalah hak pengguna**
Pengguna harus dapat menyesuaikan antarmuka sesuai preferensi visual dan ergonomi mereka. Preferensi disimpan secara persisten dan diaplikasikan secara global.

### 5.4 Prinsip Teknis

**P-20 — Performa dan keamanan melalui Rust**
Mesin inti PAUGERAN dibangun di atas Rust untuk menjamin keamanan memori tanpa garbage collector, konkurensi berperforma tinggi melalui Tokio, ukuran biner yang kecil, startup time yang cepat, dan konsumsi resource yang minimal.

**P-21 — Universal binary**
PAUGERAN didistribusikan sebagai satu binary universal yang dapat dijalankan di laptop pribadi, server cloud, VPS, atau homelab tanpa modifikasi kode. Perbedaan deployment hanya ditentukan oleh environment variables.

### 5.5 Prinsip Tim & Admin

**P-22 — First user is admin**
Saat `AUTH_ENABLED=true`, user pertama yang mendaftar otomatis mendapatkan peran admin. Admin bertanggung jawab mengelola anggota tim berikutnya melalui sistem undangan.

**P-23 — Admin adalah user biasa plus hak istimewa**
Admin memiliki data, sesi, dan preferensi sendiri seperti user biasa. Hak istimewa admin terbatas pada manajemen tim dan konfigurasi sistem.

**P-24 — Privacy-preserving administration**
Admin tidak dapat melihat API key pribadi user lain, isi sesi obrolan user lain, atau data pribadi user lain. Admin hanya dapat melihat metadata statistik untuk keperluan manajemen.

**P-25 — Fallback API key hierarchy**
API key dievaluasi dengan urutan: pertama API key pribadi user, kedua API key global yang disediakan admin, ketiga error jika keduanya tidak ada.

### 5.6 Prinsip Multi-Provider

**P-26 — Multi-provider LLM agnostik**
PAUGERAN tidak mengunci pengguna pada satu penyedia atau model LLM tertentu. PAUGERAN mendukung berbagai penyedia dan model — termasuk yang tidak ternama — selama model tersebut menyediakan API yang kompatibel. Pengguna bebas memilih berdasarkan kebutuhan, ketersediaan, dan biaya.

### 5.7 Prinsip Sumber & Penelitian

**P-27 — Sumber eksplisit dalam setiap output**
Setiap klaim hukum, setiap kutipan, setiap referensi dalam output PAUGERAN harus disertai dengan inline citation yang detail — mencakup nama peraturan, nomor, tahun, pasal, dan URL sumber jika berasal dari internet. Tidak ada klaim tanpa sumber.

**P-28 — Penelitian web yang bertanggung jawab**
PAUGERAN hanya boleh mengakses situs web yang masuk dalam daftar putih (whitelist) yang mencakup situs pemerintah resmi dan sumber hukum tepercaya. Akses ke situs di luar whitelist dilarang. Setiap akses web harus mencantumkan sumber secara eksplisit dalam output.

**P-29 — Basis pengetahuan yang dapat dibangun**
Peraturan, pasal, PP, dan sejenisnya yang pernah diteliti oleh PAUGERAN dapat disimpan ke dalam Legal Knowledge Base internal. Basis pengetahuan ini menjadi sumber referensi untuk analisis di masa depan tanpa perlu melakukan penelitian ulang dari internet, mempercepat analisis dan mengurangi ketergantungan pada koneksi internet.

### 5.8 Prinsip Lisensi

**P-30 — Lisensi yang adil dan transparan**
PAUGERAN menggunakan sistem lisensi untuk penggunaan berkelanjutan. Lisensi divalidasi secara berkala saat PAUGERAN online tanpa mengganggu pengguna. Jika lisensi tidak valid, agen tidak dapat digunakan, tetapi data pengguna tetap aman dan dapat diakses untuk keperluan backup atau migrasi.

### 5.9 Prinsip Aksesibilitas & Export

**P-31 — Aksesibilitas adalah standar, bukan fitur tambahan**
PAUGERAN harus dapat digunakan oleh pengguna dengan berbagai kemampuan. Keyboard navigation, screen reader support, high contrast mode, dan reduced motion harus tersedia sebagai standar.

**P-32 — Export profesional siap pakai**
Laporan yang diekspor harus siap digunakan dalam konteks profesional tanpa perlu formatting ulang. Template harus memenuhi standar dokumen hukum Indonesia.

---

## 6. AKTOR & STAKEHOLDER

### 6.1 Pengguna (User)

**6.1.1** Orang yang menggunakan PAUGERAN.

**6.1.2** Pengguna memiliki data, sesi, dan preferensi sendiri.

**6.1.3** Dalam deployment tanpa auth, semua pengguna adalah local user tunggal.

**6.1.4** Dalam deployment dengan auth aktif, pengguna adalah anggota tim yang di-invite oleh admin.

### 6.2 Administrator (Admin)

**6.2.1** User pertama yang mendaftar saat `AUTH_ENABLED=true`.

**6.2.2** Admin memiliki hak istimewa untuk mengelola anggota tim, menyediakan API key global, mengelola Legal Knowledge Base, mengonfigurasi sistem, dan mengelola lisensi.

**6.2.3** Admin tetap memiliki data pribadi sendiri yang tidak dapat diakses oleh admin lain.

### 6.3 PAUGERAN (Agen AI)

**6.3.1** Agen AI yang melakukan wawancara adaptif dalam sesi obrolan, pemodelan masalah, penelitian hukum (dari basis pengetahuan internal dan internet), penalaran, pengujian, dan penyusunan laporan.

### 6.4 Sesi Obrolan (Chat Session)

**6.4.1** Entitas yang menaungi satu topik analisis kasus.

**6.4.2** Setiap sesi terisolasi dari sesi lain dan memiliki:
- ID unik
- Judul (auto-generated atau manual)
- Daftar pesan
- Fakta yang diekstrak
- Dokumen yang diunggah
- Peta keterlacakan
- Laporan hukum
- Timestamp pembuatan dan pembaruan

### 6.5 Sumber Hukum

**6.5.1** Sumber eksternal yang digunakan sebagai dasar analisis.

**6.5.2** Termasuk peraturan perundang-undangan, putusan pengadilan, doktrin, dan dokumen resmi lembaga.

**6.5.3** Sumber dapat berasal dari Legal Knowledge Base internal atau dari internet melalui penelitian web.

### 6.6 Dokumen Pengguna

**6.6.1** Dokumen yang diberikan pengguna sebagai sumber fakta atau bukti.

**6.6.2** Disimpan secara aman dan terisolasi dalam sesi obrolan.

### 6.7 Penyedia LLM

**6.7.1** Layanan eksternal yang dipanggil oleh PAUGERAN menggunakan API key milik pengguna atau global.

**6.7.2** PAUGERAN mendukung berbagai penyedia termasuk Anthropic, OpenAI, Groq, Together AI, Fireworks, OpenRouter, Mistral AI, DeepSeek, Ollama (lokal), LM Studio (lokal), vLLM (lokal), dan penyedia lain yang kompatibel dengan API OpenAI atau Anthropic.

**6.7.3** PAUGERAN tidak menyimpan, memproses, atau meneruskan API key ke pihak lain selain penyedia resmi yang dipilih pengguna.

### 6.8 Server Lisensi PAUGERAN

**6.8.1** Layanan eksternal yang memvalidasi lisensi key PAUGERAN.

**6.8.2** Server ini hanya menerima lisensi key dan identifier instalasi, tidak menerima data pengguna, API key LLM, atau konten sesi.

### 6.9 Legal Knowledge Base

**6.9.1** Basis pengetahuan hukum internal yang dibangun dari peraturan, pasal, PP, dan sejenisnya yang pernah diteliti oleh PAUGERAN dan disimpan secara eksplisit oleh pengguna atau admin.

**6.9.2** Basis ini bersifat global (dapat diakses semua sesi) dan read-only selama analisis.

---

# BAGIAN II — SPESIFIKASI FUNGSIONAL LENGKAP

---

## 7. MANAJEMEN SESI OBROLAN

### 7.1 Konsep Dasar

**7.1.1** Setiap sesi obrolan adalah entitas independen yang berdiri sendiri.

**7.1.2** Sesi A tidak mengetahui keberadaan sesi B.

**7.1.3** Data yang bersifat global hanya API key, preferensi UI, Legal Knowledge Base, dan lisensi.

### 7.2 Membuat Sesi Baru

**7.2.1** Sesi baru dapat dibuat melalui:
- Tombol "+ Sesi Baru" di sidebar
- Shortcut keyboard `Ctrl+N` (Cmd+N di Mac)
- Command palette (Ctrl+K) → "New Session"
- Otomatis saat mengirim pesan pertama di halaman kosong

**7.2.2** Setelah dibuat:
- Sesi langsung aktif dan fokus
- User bisa langsung mengetik pesan pertama
- Judul sesi di-generate otomatis dari pesan pertama (atau bisa diubah manual)

**7.2.3** Setiap sesi dapat memiliki model LLM yang dipilih sendiri melalui dropdown di header sesi.

### 7.3 Sidebar Daftar Sesi

**7.3.1** Sidebar menampilkan daftar sesi dikelompokkan berdasarkan waktu:
- Hari Ini
- Kemarin
- 7 Hari Terakhir
- 30 Hari Terakhir
- Lebih Lama

**7.3.2** Setiap item menampilkan:
- Judul sesi
- Timestamp terakhir (format sesuai preferensi)
- Status indicator (aktif/selesai)
- Preview pesan terakhir (1 baris)

**7.3.3** Aksi pada setiap item (hover untuk reveal):
- ✏️ Rename
- 📋 Duplikat
- 🗑️ Hapus

**7.3.4** Sesi aktif di-highlight.

### 7.4 Membuka Sesi Lama

**7.4.1** Sesi lama tetap ada di sidebar selama tidak dihapus.

**7.4.2** Klik sesi lama → semua pesan, fakta, dokumen, peta keterlacakan, dan laporan tetap tersedia.

**7.4.3** User bisa melanjutkan obrolan dari titik terakhir.

**7.4.4** Tidak ada batas waktu untuk membuka sesi lama.

### 7.5 Menghapus Sesi

**7.5.1** User bisa menghapus sesi kapan saja.

**7.5.2** Konfirmasi dialog sebelum hapus: "Hapus sesi ini? Semua data akan dihapus permanen."

**7.5.3** Yang dihapus:
- Semua pesan di sesi tersebut
- Semua fakta, dokumen, peta keterlacakan
- File fisik di `{data_dir}/documents/{session_id}/`
- Laporan yang di-generate untuk sesi tersebut

**7.5.4** Yang TIDAK dihapus:
- API key (tetap tersimpan)
- Preferensi UI (tetap tersimpan)
- Sesi lain (tidak terpengaruh)
- Legal Knowledge Base (tidak terpengaruh)
- Lisensi (tidak terpengaruh)

### 7.6 Isolasi Sesi

**7.6.1** Query database WAJIB filter by `session_id`.

**7.6.2** File storage menggunakan path per session: `{data_dir}/documents/{session_id}/`.

**7.6.3** Tidak ada endpoint yang bisa mengakses data sesi lain.

**7.6.4** Tidak ada "global search" yang melintasi sesi.

**7.6.5** Setiap sesi adalah "dunia" yang terisolasi.

### 7.7 Siklus Hidup Sesi

```
CREATED → ACTIVE → COMPLETED → ARCHIVED → DELETED
```

**CREATED:** Sesi baru dibuat, belum ada pesan
**ACTIVE:** Sedang ada obrolan, agen berjalan
**COMPLETED:** Analisis selesai, laporan di-generate
**ARCHIVED:** Tidak aktif tapi masih bisa dibuka
**DELETED:** Dihapus permanen (tidak bisa dikembalikan)

### 7.8 Batasan & Limit

**7.8.1** Tidak ada batasan:
- Jumlah sesi (unlimited)
- Jumlah pesan per sesi (unlimited)
- Jumlah dokumen per sesi (hanya dibatasi disk space)
- Usia sesi (unlimited, bisa dibuka selamanya)

**7.8.2** Batasan teknis:
- Ukuran file dokumen: max 10 MB per file
- Jumlah file per sesi: max 50 file
- Context window LLM: sesuai limit provider (100k-200k tokens)

---

## 8. DIALOG ADAPTIF & PEMAHAMAN MASALAH

### 8.1 Wawancara Bertingkat

**8.1.1 Tingkat 1 — Identifikasi Struktur Dasar:**
- Siapa pihak yang terlibat?
- Apa yang terjadi?
- Kapan kejadian?
- Di mana lokasi?
- Apa tujuan pengguna?

**8.1.2 Tingkat 2 — Fakta Material:**
- Apakah ada perjanjian tertulis?
- Apakah ada bukti pembayaran?
- Apakah ada komunikasi tertulis?
- Apakah ada saksi?
- Apakah ada dokumen pendukung?

**8.1.3 Tingkat 3 — Fakta Kritis:**
- Apakah ada pengecualian atau kondisi khusus?
- Apakah ada sengketa fakta?
- Apakah ada proses hukum yang sudah berjalan?
- Apakah ada perubahan perjanjian?
- Apakah ada tindakan pihak lawan yang relevan?

**8.1.4** Jumlah tingkat tidak harus selalu tiga. Jumlah pertanyaan ditentukan oleh kompleksitas masalah.

### 8.2 Kriteria Pertanyaan

**8.2.1** Setiap pertanyaan harus memiliki alasan yang dapat dijelaskan secara internal: "Mengapa pertanyaan ini diperlukan?"

**8.2.2** Pertanyaan diprioritaskan berdasarkan pengaruhnya terhadap:
- Klasifikasi masalah
- Penerapan norma
- Unsur hukum
- Pembuktian
- Kemungkinan hasil
- Strategi atau tujuan pengguna

**8.2.3** Pertanyaan yang tidak mempunyai pengaruh material harus dihindari.

### 8.3 Rekonstruksi Masalah

**8.3.1** PAUGERAN harus secara berkala memberikan ringkasan yang mencakup:

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
⚠️ [Isu 1]
⚠️ [Isu 2]

Hal yang Masih Perlu Dipastikan:
• [Pertanyaan 1]
• [Pertanyaan 2]

---
Apakah pemahaman saya sudah sesuai?
[Setuju] [Revisi]
```

**8.3.2** Ringkasan harus diakhiri dengan pertanyaan: "Apakah pemahaman saya sudah sesuai?" disertai opsi Setuju dan Revisi.

### 8.4 Gerbang Kesiapan Penalaran

**8.4.1** Kondisi minimal untuk memulai penelitian:
- Tujuan diketahui
- Pihak utama diketahui
- Kronologi material diketahui
- Hubungan hukum dapat diperkirakan
- Isu utama dapat dirumuskan
- Fakta kritis telah diidentifikasi
- Kekurangan informasi telah dicatat

**8.4.2** Jika belum: lanjutkan pemahaman (kembali ke TANYA).

**8.4.3** Jika cukup: mulai penalaran hukum (lanjut ke RUMUSKAN).

**8.4.4** Keputusan ini harus dapat dijelaskan kepada pengguna.

### 8.5 Kontrol Pengguna

**8.5.1** Ketika PAUGERAN menilai informasi cukup, antarmuka harus memberikan pilihan eksplisit:
- Lanjutkan Pemahaman
- Mulai Penalaran Hukum

**8.5.2** Pengguna harus dapat memilih.

---

## 9. PENELITIAN HUKUM MULTI-SUMBER

### 9.1 Sumber Penelitian

PAUGERAN melakukan penelitian dari tiga sumber utama:

**9.1.1 Legal Knowledge Base Internal**
- Basis pengetahuan lokal yang dibangun dari penelitian sebelumnya
- Paling cepat, tidak butuh internet
- Sumber utama untuk analisis rutin

**9.1.2 Dokumen Pengguna**
- Dokumen yang diunggah pengguna ke sesi
- Sumber fakta dan bukti spesifik kasus

**9.1.3 Penelitian Web**
- Situs pemerintah resmi dan sumber tepercaya
- Untuk verifikasi dan penelitian baru
- Hanya ke domain whitelist

### 9.2 Urutan Pencarian

**9.2.1** Selama fase TELITI, PAUGERAN mencari dengan urutan:
1. Legal Knowledge Base internal (paling cepat, offline)
2. Dokumen yang diunggah pengguna
3. Penelitian web ke situs pemerintah dan sumber tepercaya

**9.2.2** Jika ditemukan di Knowledge Base dan statusnya masih berlaku, PAUGERAN menggunakan entri tersebut tanpa perlu mengakses internet.

**9.2.3** Jika tidak ditemukan atau statusnya tidak pasti, PAUGERAN melakukan penelitian web untuk verifikasi.

### 9.3 Hirarki Sumber

**9.3.1** Urutan prioritas:
1. Undang-Undang (UU)
2. Peraturan Pemerintah (PP)
3. Peraturan Presiden (Perpres)
4. Peraturan Menteri (Permen)
5. Putusan Mahkamah Agung
6. Putusan Pengadilan Tinggi/Negeri
7. Doktrin/Literatur Hukum
8. Sumber sekunder lainnya

**9.3.2** Urutan ini bukan aturan mutlak. Kekuatan sumber ditentukan berdasarkan jenis, kewenangan, yurisdiksi, waktu, dan relevansi.

### 9.4 Pemeriksaan Keberlakuan

**9.4.1** Untuk setiap peraturan penting, sistem harus menentukan:
- Nomor
- Nama
- Tanggal
- Tanggal mulai berlaku
- Status
- Perubahan
- Pencabutan
- Peraturan terkait
- Relevansi terhadap waktu kejadian

**9.4.2** PAUGERAN tidak boleh menggunakan norma historis sebagai norma saat ini tanpa menjelaskan statusnya.

### 9.5 Model Norma

**9.5.1** Setiap norma direpresentasikan dengan struktur:
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

### 9.6 Identifikasi Isu Hukum

**9.6.1** PAUGERAN mengubah masalah menjadi pertanyaan hukum yang dipecah menjadi unsur-unsur.

**9.6.2** Contoh:
> "Apakah pembeli dapat menuntut penjual?"

Dipecah menjadi:
- Apakah perjanjian sah?
- Apakah kewajiban telah lahir?
- Apakah kewajiban telah jatuh tempo?
- Apakah terjadi pelanggaran?
- Apakah terdapat alasan pembebasan?
- Apa upaya hukum yang tersedia?
- Apa akibat hukum yang mungkin?

### 9.7 Analisis Unsur

**9.7.1** Setiap klaim hukum dipecah menjadi unsur:
```
Klaim
  ↓
Unsur 1
  ↓
Unsur 2
  ↓
Unsur 3
  ↓
Pengecualian
  ↓
Kesimpulan
```

**9.7.2** Setiap unsur dipetakan terhadap fakta.

---

## 10. PENALARAN & ARGUMENTASI

### 10.1 Penerapan Hukum

**10.1.1** Untuk setiap isu:
- **Kaidah**: Apa hukum yang relevan?
- **Fakta**: Apa fakta yang relevan?
- **Penerapan**: Bagaimana fakta memenuhi atau tidak memenuhi kaidah?
- **Kesimpulan antara**: Apa hasil analisis terhadap isu tersebut?

### 10.2 Argumentasi Dua Sisi

**10.2.1** PAUGERAN wajib menghasilkan:
- Argumen yang mendukung
- Argumen yang berlawanan
- Kelemahan argumen pendukung
- Kelemahan argumen berlawanan
- Penilaian PAUGERAN

**10.2.2** PAUGERAN tidak boleh melakukan pencarian konfirmasi saja.

### 10.3 Analisis Alternatif

**10.3.1** Apabila terdapat lebih dari satu interpretasi yang masuk akal:
```
Interpretasi A
  Dasar
  Kekuatan
  Kelemahan
  Konsekuensi

Interpretasi B
  Dasar
  Kekuatan
  Kelemahan
  Konsekuensi
```

**10.3.2** Sistem memberikan penilaian berdasarkan sumber yang ditemukan.

### 10.4 Putusan Pengadilan

**10.4.1** Putusan tidak boleh digunakan hanya karena memiliki kata kunci yang sama.

**10.4.2** PAUGERAN harus membandingkan:
- Fakta
- Isu
- Norma
- Pertimbangan
- Hasil
- Konteks
- Perbedaan material

**10.4.3** Output minimal:
> "Putusan ini relevan karena..."

dan:

> "Putusan ini tidak dapat diterapkan secara langsung karena..."

### 10.5 Analisis Konflik

**10.5.1** PAUGERAN harus mendeteksi:
- Konflik antarperaturan
- Perbedaan interpretasi
- Perbedaan putusan
- Perbedaan fakta
- Perubahan hukum

**10.5.2** Jika terdapat konflik: konflik tersebut harus ditampilkan, bukan disembunyikan.

### 10.6 Analisis Kontra

**10.6.1** Sebelum kesimpulan final, PAUGERAN harus melakukan pemeriksaan:
> "Apa alasan terkuat bahwa kesimpulan ini dapat keliru?"

**10.6.2** Kemudian mencari:
- Sumber hukum yang berlawanan
- Fakta yang belum dipertimbangkan
- Pengecualian
- Putusan yang berbeda
- Interpretasi lain
- Kelemahan bukti

### 10.7 Penilaian Kepastian

**10.7.1** Kesimpulan dikategorikan:
- Sangat kuat
- Kuat
- Cukup kuat
- Bersyarat
- Belum dapat ditentukan
- Lemah
- Bertentangan

**10.7.2** Penilaian harus disertai alasan.

---

## 11. KETERLACAKAN & VERIFIKASI

### 11.1 Struktur Keterlacakan

**11.1.1** Setiap kesimpulan material harus mempunyai struktur:
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

### 11.2 Validasi Keterlacakan

**11.2.1** Checklist validasi:
- Setiap kesimpulan memiliki minimal 1 alasan
- Setiap alasan memiliki minimal 1 kaidah hukum
- Setiap kaidah memiliki sumber yang valid
- Setiap sumber memiliki status keberlakuan
- Setiap kaidah terhubung ke minimal 1 fakta
- Setiap fakta memiliki status verifikasi
- Setiap kesimpulan memiliki kontraargumentasi

### 11.3 Penandaan Keterlacakan Tidak Lengkap

**11.3.1** Jika salah satu elemen tidak tersedia:
```
⚠️ KETERLACAKAN TIDAK LENGKAP

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

### 11.4 Peta Keterlacakan Visual

**11.4.1** Format visual berupa graf dengan node dan edge:
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

**11.4.2** Interaktivitas:
- Klik node untuk lihat detail
- Hover untuk highlight jalur
- Zoom in/out
- Pan
- Export PNG/SVG

---

## 12. LEGAL KNOWLEDGE BASE

### 12.1 Definisi

**12.1.1** Legal Knowledge Base adalah basis pengetahuan hukum internal yang menyimpan peraturan, pasal, PP, dan sejenisnya yang pernah diteliti oleh PAUGERAN.

**12.1.2** Basis ini menjadi sumber referensi untuk analisis di masa depan tanpa perlu melakukan penelitian ulang dari internet.

### 12.2 Jenis Dokumen yang Dapat Disimpan

**12.2.1** Undang-Undang (UU)
**12.2.2** Peraturan Pemerintah (PP)
**12.2.3** Peraturan Presiden (Perpres)
**12.2.4** Peraturan Menteri (Permen)
**12.2.5** Putusan Pengadilan (Mahkamah Agung, Pengadilan Tinggi, Pengadilan Negeri)
**12.2.6** Doktrin hukum (buku, jurnal)
**12.2.7** Lainnya (Surat Edaran, Peraturan Daerah, dll)

**12.2.8** Hanya peraturan dan dokumen hukum formal yang dapat disimpan. Dokumen pengguna (kontrak pribadi, surat, dll) tidak disimpan di sini.

### 12.3 Struktur Penyimpanan

**12.3.1** Setiap entri Legal Knowledge Base terdiri dari:
- **Metadata**: judul, jenis sumber, nomor, tahun, tanggal berlaku, tanggal dicabut, status, URL sumber
- **Konten**: teks lengkap peraturan
- **Pasal-pasal**: dipecah per pasal dengan embedding vector untuk pencarian semantik
- **Embeddings**: vektor numerik untuk setiap pasal, dihasilkan oleh model embedding

### 12.4 Cara Penambahan ke Knowledge Base

**12.4.1** Otomatis saat penelitian web:
- Ketika PAUGERAN menemukan peraturan dari situs pemerintah selama fase TELITI
- Sistem menawarkan opsi "Simpan ke Knowledge Base" setelah analisis selesai

**12.4.2** Manual oleh pengguna:
- Pengguna dapat mengunggah file PDF peraturan
- Meminta PAUGERAN mengekstrak dan menyimpannya ke Knowledge Base

**12.4.3** Manual oleh admin:
- Admin dapat melakukan bulk import peraturan melalui halaman admin

**12.4.4** Import dari URL:
- Pengguna atau admin dapat memberikan URL peraturan
- PAUGERAN akan mengambil, memproses, dan menyimpannya

### 12.5 Pencarian di Knowledge Base

**12.5.1** Pencarian semantik: Menggunakan vector embeddings untuk menemukan pasal yang relevan secara kontekstual.

**12.5.2** Pencarian keyword: Pencarian teks tradisional untuk kasus spesifik.

**12.5.3** Pencarian hybrid: Kombinasi keduanya untuk hasil terbaik.

**12.5.4** Filter: Berdasarkan jenis sumber, tahun, status (berlaku/dicabut).

### 12.6 Penggunaan dalam Analisis

**12.6.1** Selama fase TELITI, PAUGERAN pertama-tama mencari di Knowledge Base sebelum melakukan penelitian web.

**12.6.2** Jika ditemukan di Knowledge Base dan statusnya masih berlaku, PAUGERAN menggunakan entri tersebut tanpa perlu mengakses internet.

**12.6.3** Jika tidak ditemukan atau statusnya tidak pasti, PAUGERAN melakukan penelitian web untuk verifikasi.

**12.6.4** Setiap penggunaan entri Knowledge Base dicatat dalam traceability_edges dengan source_type='knowledge_base'.

### 12.7 Manajemen Knowledge Base

**12.7.1** Halaman Knowledge Base di UI menampilkan daftar semua entri dengan filter dan pencarian.

**12.7.2** Setiap entri dapat:
- Dilihat detailnya (metadata, konten, pasal-pasal)
- Diperbarui (jika ada amandemen)
- Ditandai sebagai tidak berlaku (revoked)
- Dihapus

**12.7.3** Admin dapat mengelola Knowledge Base global (berlaku untuk semua user).

**12.7.4** User dapat memiliki Knowledge Base pribadi (opsional, jika AUTH_ENABLED=true).

### 12.8 Sinkronisasi dan Update

**12.8.1** PAUGERAN dapat dikonfigurasi untuk memeriksa update peraturan secara berkala (opsional).

**12.8.2** Jika peraturan di Knowledge Base sudah dicabut atau diamendemen, sistem memberi notifikasi kepada pengguna.

**12.8.3** Pengguna dapat memilih untuk update manual atau otomatis.

### 12.9 Privasi dan Lokasi Data

**12.9.1** Legal Knowledge Base disimpan lokal di database PAUGERAN.

**12.9.2** Tidak ada data Knowledge Base yang dikirim ke server PAUGERAN atau pihak ketiga.

**12.9.3** Knowledge Base ikut ter-backup saat backup database.

---

## 13. PENELITIAN WEB

### 13.1 Konsep

**13.1.1** PAUGERAN dapat melakukan penelitian web untuk menemukan sumber hukum dari internet.

**13.1.2** Penelitian web hanya dilakukan pada situs yang masuk dalam daftar putih (whitelist).

### 13.2 Whitelist Domain

**13.2.1** Domain pemerintah resmi:
- `jdih.setkab.go.id` (Jaringan Dokumentasi dan Informasi Hukum Nasional)
- `mahkamahagung.go.id` (Mahkamah Agung RI)
- `putusan.mahkamahagung.go.id` (Direktori Putusan)
- `jdih.kemenkeu.go.id` (JDIH Kemenkeu)
- `jdih.kumham.go.id` (JDIH Kemenkumham)
- `peraturan.bpk.go.id` (BPJN BPK)
- `ojk.go.id` (Otoritas Jasa Keuangan)
- `bi.go.id` (Bank Indonesia)
- `kppu.go.id` (Komisi Pengawas Persaingan Usaha)

**13.2.2** Domain pengadilan:
- `pt-*.go.id` (Pengadilan Tinggi)
- `pn-*.go.id` (Pengadilan Negeri)
- `pa-*.go.id` (Pengadilan Agama)
- `ptun-*.go.id` (Pengadilan Tata Usaha Negara)

**13.2.3** Domain tepercaya lainnya (dapat dikonfigurasi admin):
- `houkou.co.id`
- `hukumonline.com` (dengan catatan)
- Domain lain yang ditambahkan admin

**13.2.4** Admin dapat menambah atau menghapus domain dari whitelist melalui Settings → Penelitian Web.

### 13.3 Mekanisme Penelitian Web

**13.3.1** PAUGERAN menggunakan HTTP client (Reqwest) untuk mengambil halaman web.

**13.3.2** HTML di-parse menggunakan scraper crate untuk mengekstrak konten relevan.

**13.3.3** Konten dibersihkan dari elemen non-esensial (navigasi, iklan, footer).

**13.3.4** Metadata diekstrak: judul, tanggal terbit, nomor peraturan, URL.

**13.3.5** Konten di-chunk menjadi bagian-bagian yang dapat diproses oleh LLM.

**13.3.6** Setiap chunk dicatat dengan URL sumber untuk inline citation.

### 13.4 Rate Limiting dan Etika

**13.4.1** PAUGERAN menghormati robots.txt dari setiap situs.

**13.4.2** Delay minimum 1 detik antara request ke domain yang sama.

**13.4.3** User-Agent header mengidentifikasi PAUGERAN dengan jelas: `Paugeran/1.0 (+https://paugeran.com)`.

**13.4.4** Maksimal 10 request per menit per domain.

**13.4.5** Jika situs mengembalikan error 429 atau 503, PAUGERAN berhenti mengakses domain tersebut untuk sesi tersebut.

### 13.5 Penanganan Kegagalan

**13.5.1** Jika situs tidak dapat diakses (timeout, DNS error, dll), PAUGERAN mencatat kegagalan dan melanjutkan ke sumber berikutnya.

**13.5.2** PAUGERAN tidak menyembunyikan kegagalan akses dari pengguna.

**13.5.3** Output harus mencantumkan: "Situs [URL] tidak dapat diakses pada [waktu]. Analisis berdasarkan sumber alternatif."

### 13.6 Integrasi dengan Knowledge Base

**13.6.1** Setelah penelitian web selesai, PAUGERAN menawarkan opsi untuk menyimpan peraturan yang ditemukan ke Knowledge Base.

**13.6.2** Jika pengguna menyetujui, peraturan diproses (dibersihkan, dipecah per pasal, di-embed) dan disimpan ke Knowledge Base.

**13.6.3** Peraturan yang disimpan dari web tetap mencantumkan URL sumber asli.

---

## 14. MULTI-PROVIDER LLM

### 14.1 Konsep

**14.1.1** PAUGERAN mendukung berbagai penyedia LLM melalui arsitektur provider-agnostic.

**14.1.2** Setiap penyedia diimplementasikan sebagai adapter yang mengimplementasikan trait `LlmProvider`.

### 14.2 Trait LlmProvider

```rust
#[async_trait]
pub trait LlmProvider: Send + Sync {
    /// Nama unik provider (contoh: "anthropic", "openai", "ollama")
    fn id(&self) -> &str;
    
    /// Daftar model yang didukung
    fn supported_models(&self) -> Vec<ModelInfo>;
    
    /// Panggil chat completion
    async fn chat_completion(&self, request: ChatRequest) -> Result<ChatResponse>;
    
    /// Panggil chat completion dengan streaming
    async fn chat_completion_stream(
        &self, 
        request: ChatRequest
    ) -> Result<impl Stream<Item = Result<ChatChunk>>>;
    
    /// Generate embedding untuk vector search
    async fn embedding(&self, text: &str) -> Result<Vec<f32>>;
    
    /// Validasi API key (optional)
    async fn validate_key(&self) -> Result<bool>;
}
```

### 14.3 Provider Bawaan

**14.3.1 AnthropicProvider**
- Endpoint default: `https://api.anthropic.com/v1/messages`
- Model: claude-3-5-sonnet, claude-3-5-haiku, claude-3-opus, dan model Claude lainnya
- Custom endpoint: dapat dikonfigurasi untuk proxy atau mirror
- Format: Anthropic Messages API

**14.3.2 OpenAIProvider**
- Endpoint default: `https://api.openai.com/v1`
- Model: gpt-4o, gpt-4o-mini, gpt-4-turbo, o1, o1-mini, dan model OpenAI lainnya
- Custom endpoint: dapat dikonfigurasi untuk Azure OpenAI atau proxy
- Format: OpenAI Chat Completions API

**14.3.3 OpenAICompatibleProvider**
- Provider generik untuk semua API yang kompatibel dengan format OpenAI
- Endpoint: dikonfigurasi pengguna
- Model: dikonfigurasi pengguna
- Contoh penggunaan: Groq, Together AI, Fireworks, OpenRouter, Mistral AI, DeepSeek, local Ollama, local LM Studio, local vLLM, dan penyedia lainnya
- Format: OpenAI Chat Completions API

**14.3.4 OllamaProvider (Opsional)**
- Endpoint default: `http://localhost:11434`
- Model: model lokal yang di-pull melalui Ollama
- Untuk penggunaan offline atau privasi maksimal
- Format: Ollama API

### 14.4 Konfigurasi Provider per Pengguna

**14.4.1** Setiap pengguna (atau admin untuk global) dapat mengonfigurasi satu atau lebih provider:

```json
{
  "id": "uuid",
  "provider_type": "openai_compatible",
  "name": "Groq Llama 3.3",
  "endpoint_url": "https://api.groq.com/openai/v1",
  "api_key_encrypted": "...",
  "default_model": "llama-3.3-70b-versatile",
  "available_models": ["llama-3.3-70b-versatile", "mixtral-8x7b-32768"],
  "max_tokens": 8192,
  "timeout_seconds": 60,
  "priority": 1,
  "is_active": true
}
```

### 14.5 Model Routing

**14.5.1** Untuk setiap tugas, router LLM memilih provider dan model berdasarkan:
- Preferensi pengguna (jika diset)
- Tipe tugas (interactive, reasoning, extraction)
- Ketersediaan provider
- Prioritas yang dikonfigurasi
- Biaya (opsional, berdasarkan metadata provider)

**14.5.2** Tugas interaktif (PAHAM, TANYA, KONFIRMASI): model cepat dan murah.

**14.5.3** Tugas penalaran (NALAR, BANTAH, UJI, SIMPULKAN): model dengan kemampuan reasoning tinggi.

**14.5.4** Tugas ekstraksi (TELITI, VERIFIKASI, TELUSURI): model dengan structured output yang baik.

**14.5.5** Fallback: Jika provider utama gagal, router mencoba provider berikutnya sesuai prioritas.

### 14.6 UI Konfigurasi Provider

**14.6.1** Halaman Settings → Provider LLM menampilkan daftar provider yang dikonfigurasi.

**14.6.2** Tombol "Tambah Provider" membuka form dengan pilihan tipe provider.

**14.6.3** Form konfigurasi mencakup:
- Nama provider
- Endpoint URL
- API key
- Model default
- Daftar model tersedia (auto-detect atau manual)
- Timeout
- Prioritas

**14.6.4** Tombol "Test Koneksi" untuk memvalidasi API key dan endpoint.

**14.6.5** Tombol "Detect Models" untuk mengambil daftar model dari provider.

**14.6.6** Setiap provider dapat diaktifkan, dinonaktifkan, dihapus, atau diedit.

### 14.7 Model Selection per Sesi

**14.7.1** Pengguna dapat memilih model yang digunakan untuk sesi tertentu melalui dropdown di header sesi.

**14.7.2** Pilihan model terbatas pada model yang tersedia dari provider yang dikonfigurasi.

**14.7.3** Default: model dengan prioritas tertinggi dari provider yang aktif.

---

## 15. SISTEM LISENSI

### 15.1 Konsep

**15.1.1** PAUGERAN menggunakan sistem lisensi untuk mengontrol penggunaan produk.

**15.1.2** Lisensi diperlukan untuk menggunakan agen PAUGERAN, terpisah dari konfigurasi API key LLM.

**15.1.3** Lisensi key adalah string unik yang diterbitkan oleh server lisensi PAUGERAN.

**15.1.4** Setiap instalasi PAUGERAN memiliki `installation_id` unik yang di-generate saat pertama kali dijalankan dan disimpan di database.

### 15.2 Tipe Lisensi

**15.2.1** Trial: Lisensi percobaan dengan batas waktu (misalnya 14 hari) dan fitur terbatas.

**15.2.2** Personal: Lisensi untuk penggunaan individu dengan fitur penuh.

**15.2.3** Team: Lisensi untuk tim dengan multi-user support.

**15.2.4** Enterprise: Lisensi untuk organisasi besar dengan fitur tambahan dan SLA.

### 15.3 Validasi Lisensi

**15.3.1** Saat pertama kali dijalankan, PAUGERAN menampilkan halaman aktivasi yang meminta lisensi key.

**15.3.2** Validasi awal: PAUGERAN mengirim request ke server lisensi dengan payload:
```json
{
  "license_key": "...",
  "installation_id": "...",
  "version": "1.0.0",
  "timestamp": "ISO8601"
}
```

**15.3.3** Server lisensi membalas dengan:
```json
{
  "valid": true,
  "license_type": "personal",
  "expires_at": "2027-08-28T00:00:00Z",
  "features": ["unlimited_sessions", "knowledge_base", "export_pdf"],
  "max_users": 1,
  "message": "License activated successfully"
}
```

**15.3.4** Jika valid, lisensi disimpan terenkripsi di database dan PAUGERAN dapat digunakan.

**15.3.5** Jika tidak valid, PAUGERAN menampilkan pesan error dan tidak mengizinkan penggunaan agen, tetapi data dan pengaturan tetap dapat diakses.

### 15.4 Validasi Berkala

**15.4.1** Setelah aktivasi, PAUGERAN melakukan validasi lisensi secara berkala setiap 24 jam saat terhubung ke internet.

**15.4.2** Validasi dilakukan di background tanpa mengganggu pengguna.

**15.4.3** Pengguna tidak perlu memasukkan lisensi key berulang kali.

**15.4.4** Jika validasi gagal (server tidak dapat dijangkau), PAUGERAN memasuki grace period 7 hari.

**15.4.5** Selama grace period, PAUGERAN tetap dapat digunakan.

**15.4.6** Jika grace period berakhir tanpa validasi berhasil, agen dikunci hingga validasi berhasil.

**15.4.7** Data pengguna tetap aman dan dapat diakses untuk backup atau migrasi.

### 15.5 Lisensi di Settings

**15.5.1** Halaman Settings → Lisensi menampilkan:
- Status lisensi (aktif, expired, grace period)
- Tipe lisensi
- Tanggal kedaluwarsa
- Fitur yang tersedia
- Tombol "Perbarui Lisensi" untuk mengganti lisensi key
- Tombol "Validasi Sekarang" untuk validasi manual

**15.5.2** Lisensi key ditampilkan dalam bentuk masked (hanya beberapa karakter terakhir terlihat).

### 15.6 Offline Mode

**15.6.1** PAUGERAN dapat digunakan tanpa koneksi internet selama grace period belum berakhir.

**15.6.2** Fitur yang membutuhkan internet (penelitian web, validasi lisensi) tidak tersedia saat offline.

**15.6.3** Analisis dengan Knowledge Base internal tetap dapat dilakukan saat offline.

### 15.7 Environment Variable untuk Lisensi

**15.7.1** `LICENSE_KEY`: Lisensi key dapat di-set via environment variable untuk deployment otomatis.

**15.7.2** `LICENSE_SERVER_URL`: URL server lisensi (default: `https://license.paugeran.com`).

**15.7.3** `LICENSE_CHECK_INTERVAL`: Interval validasi dalam jam (default: 24).

**15.7.4** `LICENSE_GRACE_PERIOD`: Grace period dalam hari (default: 7).

### 15.8 Keamanan Lisensi

**15.8.1** Lisensi key disimpan terenkripsi di database.

**15.8.2** Request ke server lisensi menggunakan HTTPS.

**15.8.3** Payload request di-sign dengan installation_id untuk mencegah replay attack.

**15.8.4** Server lisensi tidak menerima data pengguna, API key LLM, atau konten sesi.

### 15.9 Fallback untuk Deployment Terisolasi

**15.9.1** Untuk deployment di environment yang tidak dapat terhubung ke internet (air-gapped), admin dapat mengonfigurasi lisensi offline.

**15.9.2** Lisensi offline adalah file ter-sign yang di-generate oleh server lisensi dan di-import ke PAUGERAN.

**15.9.3** Lisensi offline memiliki masa berlaku terbatas dan harus diperbarui secara manual.

---

## 16. AUTENTIKASI & OTORISASI

### 16.1 Konsep

**16.1.1** Autentikasi adalah fitur opsional yang diaktifkan melalui `AUTH_ENABLED=true`.

**16.1.2** Ketika `AUTH_ENABLED=false` (default):
- Tidak ada halaman login
- Semua data milik "local user" tunggal
- Setup wizard muncul pertama kali untuk input API key
- Cocok untuk: individu, freelancer, penggunaan pribadi

**16.1.3** Ketika `AUTH_ENABLED=true`:
- Halaman login/register muncul
- User pertama yang mendaftar otomatis jadi admin
- Setiap user punya data terisolasi
- Admin bisa manage user via UI
- Cocok untuk: firma hukum, tim legal, korporat

### 16.2 First User is Admin

**16.2.1** User pertama yang mendaftar saat `AUTH_ENABLED=true` otomatis mendapatkan peran admin.

**16.2.2** Implementasi:
```rust
async fn register_handler(
    State(state): State<AppState>,
    Json(input): Json<RegisterRequest>,
) -> Result<Json<RegisterResponse>> {
    // Cek apakah ini user pertama
    let user_count = sqlx::query_scalar!("SELECT COUNT(*) FROM users")
        .fetch_one(&state.db)
        .await?;
    
    let role = if user_count == 0 {
        "admin"  // User pertama otomatis admin
    } else {
        // Cek invite code
        if input.invite_code.is_none() {
            return Err(AppError::InviteCodeRequired);
        }
        validate_invite_code(&state.db, input.invite_code.as_ref().unwrap()).await?;
        "user"
    };
    
    // Create user...
}
```

### 16.3 User Roles

**16.3.1** Admin:
- Semua hak user biasa
- Kelola user (invite, deactivate, promote, demote, delete)
- Lihat statistik usage tim
- Set global API keys dan global LLM providers
- Kelola Legal Knowledge Base global
- Konfigurasi sistem termasuk whitelist domain penelitian web
- Kelola lisensi tim

**16.3.2** User:
- Kelola sesi obrolan sendiri
- Kelola LLM providers sendiri
- Kustomisasi UI sendiri
- Ekspor laporan sendiri
- Kelola Legal Knowledge Base pribadi (opsional)

### 16.4 Proteksi Admin

**16.4.1** Admin tidak bisa menurunkan diri sendiri dari role admin:
```rust
if current_user.id == target_user_id && new_role == "user" {
    return Err(AppError::CannotDemoteSelf);
}
```

**16.4.2** Harus ada minimal 1 admin di sistem:
```rust
if new_role == "user" {
    let admin_count = count_admins(&db).await?;
    if admin_count <= 1 {
        let target = get_user(&db, target_user_id).await?;
        if target.role == "admin" {
            return Err(AppError::LastAdminCannotBeDemoted);
        }
    }
}
```

**16.4.3** Admin tidak bisa lihat data user lain:
```rust
// Endpoint ini SELALU filter by current_user.id
let sessions = sqlx::query_as!(
    Session,
    "SELECT * FROM chat_sessions WHERE user_id = ?",
    user.id  // SELALU current user
)
.fetch_all(&db)
.await?;
```

**16.4.4** Admin tidak bisa:
- Melihat API key pribadi user lain
- Melihat isi sesi obrolan user lain
- Melihat data pribadi user lain

**16.4.5** Semua aksi admin dicatat di admin_audit_logs.

### 16.5 Invitation System

**16.5.1** Admin dapat mengundang user baru melalui:

**Opsi A: Via Email** (jika SMTP dikonfigurasi)
- Admin masukkan email calon user
- Sistem kirim email dengan link undangan
- User klik link → buat akun

**Opsi B: Via Link Undangan**
- Admin generate link undangan
- Bagikan link ke calon user
- Link memiliki expiry dan maximum usage

**16.5.2** Undangan memiliki expiry date dan maximum usage.

**16.5.3** Undangan yang sudah dipakai ditandai dengan used_by dan used_at.

### 16.6 Admin Panel

**16.6.1** Admin panel diakses via tab "👥 Tim" di Settings.

**16.6.2** Fitur:
- Daftar anggota tim
- Undang anggota baru
- Promosi/demosi role
- Nonaktifkan/aktifkan user
- Reset password user
- Hapus user permanen
- Lihat statistik penggunaan
- Konfigurasi sistem
- Kelola global API keys dan LLM providers
- Kelola Legal Knowledge Base global
- Kelola lisensi tim

### 16.7 Audit Trail

**16.7.1** Setiap aksi admin dicatat:
```rust
pub async fn log_admin_action(
    db: &DbPool,
    admin_id: &str,
    action: &str,
    target_user_id: Option<&str>,
    details: Option<&str>,
    ip_address: &str,
) -> Result<()> {
    sqlx::query!(
        r#"INSERT INTO admin_audit_logs 
           (id, admin_id, action, target_user_id, details, ip_address) 
           VALUES (?, ?, ?, ?, ?, ?)"#,
        Uuid::new_v4().to_string(),
        admin_id,
        action,
        target_user_id,
        details,
        ip_address
    )
    .execute(db)
    .await?;
    Ok(())
}
```

**16.7.2** Aksi yang di-log:
- User di-invite
- User di-promote/demote
- User di-deactivate/reactivate
- User dihapus
- Global API key ditambah/dihapus
- Global LLM provider ditambah/dihapus
- Konfigurasi sistem diubah
- Legal Knowledge Base global dimodifikasi

---

## 17. KUSTOMISASI ANTARMUKA

### 17.1 Prinsip

**17.1.1** Antarmuka adalah milik pengguna.

**17.1.2** Pengguna harus dapat menyesuaikan tampilan sesuai preferensi visual dan ergonomi mereka.

**17.1.3** Preferensi disimpan secara persisten dan diaplikasikan secara global (ke semua sesi).

### 17.2 Opsi Kustomisasi

**17.2.1** Tema Warna:
- Light (default)
- Dark
- System (mengikuti preferensi OS)
- Sepia (ramah mata untuk baca panjang)
- High Contrast (untuk aksesibilitas)

**17.2.2** Ukuran Font:
- Small (14px)
- Medium (16px, default)
- Large (18px)
- Extra Large (20px)

**17.2.3** Lebar Konten:
- Narrow (700px, fokus, minim distraksi)
- Medium (900px, default, seimbang)
- Wide (1200px, banyak ruang)
- Full (100% lebar layar)

**17.2.4** Posisi Sidebar:
- Kiri (default)
- Kanan
- Tersembunyi (auto-hide, muncul saat hover)

**17.2.5** Bahasa Antarmuka:
- Bahasa Indonesia (default)
- English

**Catatan:** Bahasa analisis hukum tetap Bahasa Indonesia, hanya antarmuka yang bisa diubah.

**17.2.6** Font Family:
- Sans-serif (default, modern) — Inter, system-ui
- Serif (klasik, formal) — Georgia, Times
- Monospace (teknis) — JetBrains Mono, Fira Code

**17.2.7** Animasi & Transisi:
- Full (default) — Semua animasi aktif
- Reduced — Animasi minimal
- None — Tanpa animasi (untuk aksesibilitas)

**17.2.8** Format Tanggal:
- 28 Agustus 2026 (default, Indonesia formal)
- 28/08/2026 (format pendek)
- August 28, 2026 (format English)
- 2026-08-28 (ISO format)

**17.2.9** Notifikasi Suara:
- Aktif — Bunyi saat analisis selesai
- Senyap — Tanpa suara

**17.2.10** Density:
- Comfortable (default)
- Compact
- Spacious

### 17.3 Panel Pengaturan

**17.3.1** Akses:
- Tombol ⚙️ di kanan atas
- Shortcut `Ctrl+,` (Cmd+, di Mac)
- Command palette → "Settings"

**17.3.2** Kategori:
1. 🎨 Tampilan — Tema, warna, layout
2. 🔤 Font — Ukuran, family
3. 🌐 Bahasa — Bahasa UI
4. ♿ Aksesibilitas — Mode aksesibilitas
5. 🤖 LLM Providers — Manajemen provider
6. 🔑 API Keys (Legacy) — Manajemen API key
7. 📚 Legal Knowledge Base — Kelola basis pengetahuan
8. 🌐 Penelitian Web — Whitelist domain
9. 🔐 Lisensi — Status dan manajemen lisensi
10. 💾 Data — Backup, restore, reset
11. 👥 Tim — Manajemen anggota (jika AUTH_ENABLED=true)
12. ℹ️ Tentang — Versi, lisensi, kredit

### 17.4 Penyimpanan Preferensi

**17.4.1** Lokasi: Tabel `ui_preferences` di database.

**17.4.2** Struktur:
```json
{
  "theme": "dark",
  "font_size": "medium",
  "content_width": "medium",
  "sidebar_position": "left",
  "ui_language": "id",
  "font_family": "sans-serif",
  "animations": "full",
  "date_format": "id-formal",
  "sound_notifications": true,
  "density": "comfortable",
  "updated_at": "2026-08-28T10:00:00Z"
}
```

### 17.5 Aplikasi Preferensi

**17.5.1** Saat Load Aplikasi:
1. Frontend fetch preferensi dari `/api/v1/preferences`
2. Apply ke CSS variables dan state
3. Render UI sesuai preferensi

**17.5.2** Saat Ubah Preferensi:
1. User pilih opsi di settings panel
2. Frontend langsung apply (instant feedback)
3. Simultan, POST ke `/api/v1/preferences` untuk persist
4. Jika gagal simpan, tampilkan warning tapi tetap apply (graceful degradation)

### 17.6 Reset ke Default

**17.6.1** Tombol "Reset ke Default" di bagian bawah settings.

**17.6.2** Konfirmasi dialog.

**17.6.3** Hapus semua preferensi tersimpan.

**17.6.4** Reload dengan default values.

**17.6.5** Tidak mempengaruhi data sesi atau API key.

---

## 18. AKSESIBILITAS

### 18.1 Prinsip

**18.1.1** PAUGERAN harus dapat digunakan oleh pengguna dengan berbagai kemampuan.

**18.1.2** Aksesibilitas adalah standar, bukan fitur tambahan.

### 18.2 Keyboard Navigation

**18.2.1** Semua interaksi dapat dilakukan melalui keyboard.

**18.2.2** Focus order harus logis dan dapat diprediksi.

**18.2.3** Focus indicator harus terlihat jelas.

**18.2.4** Tab, Shift+Tab, Enter, Space, Arrow keys, Escape harus berfungsi sesuai konvensi.

### 18.3 Keyboard Shortcuts Lengkap

**18.3.1** Navigasi:
- `Ctrl+N`: Sesi baru
- `Ctrl+K`: Command palette (pencarian cepat)
- `Ctrl+P`: Pilih sesi sebelumnya
- `Ctrl+/`: Toggle sidebar
- `Ctrl+1` sampai `Ctrl+9`: Pindah ke sesi ke-1 sampai ke-9
- `Ctrl+,`: Buka pengaturan
- `Ctrl+Shift+L`: Buka Legal Knowledge Base
- `Ctrl+Shift+E`: Export laporan
- `Ctrl+Shift+K`: Kelola LLM providers
- `Esc`: Tutup modal/drawer/command palette

**18.3.2** Dalam chat:
- `Enter`: Kirim pesan
- `Shift+Enter`: Baris baru
- `Ctrl+↑`: Edit pesan terakhir
- `Ctrl+↓`: Batal edit
- `Ctrl+C`: Salin pesan terpilih
- `Ctrl+Shift+C`: Salin dengan citation

**18.3.3** Dalam dokumen/laporan:
- `Ctrl+F`: Cari dalam dokumen
- `Ctrl+G`: Cari berikutnya
- `Ctrl+Shift+G`: Cari sebelumnya
- `Ctrl+P`: Print / Export
- `Ctrl+S`: Simpan perubahan

### 18.4 Command Palette

**18.4.1** Command palette (Ctrl+K) adalah cara cepat untuk mengakses semua fitur tanpa navigasi menu.

**18.4.2** Command palette mendukung:
- Pencarian fuzzy untuk semua perintah
- Recent commands
- Keyboard navigation
- Preview untuk beberapa perintah

**18.4.3** Perintah yang tersedia di command palette:
- Buat sesi baru
- Pindah sesi
- Buka pengaturan
- Kelola API key
- Kelola LLM providers
- Kelola Knowledge Base
- Export laporan
- Backup data
- Restore data
- Toggle tema
- Ubah bahasa
- Dan semua aksi lainnya

### 18.5 Screen Reader Support

**18.5.1** Semua elemen interaktif memiliki ARIA labels yang sesuai.

**18.5.2** Perubahan status diumumkan melalui ARIA live regions.

**18.5.3** Streaming teks di chat diumumkan secara bertahap.

**18.5.4** Error dan notifikasi diumumkan dengan jelas.

### 18.6 Mode Aksesibilitas

**18.6.1** High Contrast Mode: Kontras warna ditingkatkan untuk pengguna dengan gangguan penglihatan.

**18.6.2** Reduced Motion Mode: Animasi diminimalkan untuk pengguna yang sensitif terhadap gerakan.

**18.6.3** Large Text Mode: Font diperbesar secara default.

**18.6.4** Screen Reader Optimized Mode: Layout dioptimalkan untuk screen reader.

### 18.7 Warna dan Kontras

**18.7.1** Semua teks harus memiliki rasio kontras minimal 4.5:1 terhadap latar belakang (WCAG 2.1 AA).

**18.7.2** Teks besar (18pt atau 14pt bold) harus memiliki rasio kontras minimal 3:1.

**18.7.3** Informasi tidak boleh disampaikan hanya melalui warna.

### 18.8 Fokus dan Interaksi

**18.8.1** Focus indicator harus memiliki rasio kontras minimal 3:1.

**18.8.2** Target klik minimal 44x44 piksel.

**18.8.3** Hover states harus jelas terlihat.

### 18.9 Toast Notifications

**18.9.1** Notifikasi non-modal untuk aksi yang berhasil.

**18.9.2** Toast otomatis hilang setelah 5 detik.

**18.9.3** Toast dapat ditutup manual.

**18.9.4** Toast tidak mengganggu fokus keyboard.

### 18.10 Breadcrumb Navigation

**18.10.1** Breadcrumb ditampilkan di atas konten untuk navigasi yang jelas.

**18.10.2** Contoh: `Beranda > Sesi > Sengketa Tanah > Laporan`

**18.10.3** Setiap item breadcrumb dapat diklik.

### 18.11 Quick Actions

**18.11.1** Tombol aksi cepat di header sesi:
- 📊 Peta Keterlacakan
- 📄 Laporan
- 📚 Simpan ke Knowledge Base
- 📥 Export
- ⋯ Menu lainnya

**18.11.2** Setiap quick action memiliki tooltip dan keyboard shortcut.

### 18.12 Undo/Redo

**18.12.1** Aksi yang dapat di-undo:
- Edit judul sesi
- Edit preferensi
- Hapus pesan (dalam batas waktu)
- Hapus dokumen (dalam batas waktu)

**18.12.2** Keyboard shortcut: `Ctrl+Z` untuk undo, `Ctrl+Shift+Z` atau `Ctrl+Y` untuk redo.

---

## 19. EXPORT DOKUMEN PROFESIONAL

### 19.1 Konsep

**19.1.1** PAUGERAN dapat mengekspor hasil analisis ke dalam format PDF profesional dan DOCX profesional yang siap digunakan dalam konteks profesional tanpa formatting ulang.

### 19.2 Format PDF Profesional

**19.2.1** Layout:
- Kertas A4 (210mm x 297mm)
- Margin: 2.54 cm semua sisi (standar dokumen hukum Indonesia)
- Header: Nama dokumen, judul sesi, tanggal
- Footer: Nomor halaman, nama pengguna (opsional), watermark "Dokumen PAUGERAN" (opsional)

**19.2.2** Tipografi:
- Font utama: Times New Roman 12pt untuk isi
- Font heading: Times New Roman Bold, ukuran bertingkat (16pt untuk H1, 14pt untuk H2, 12pt untuk H3)
- Line spacing: 1.5
- Justified alignment untuk paragraf

**19.2.3** Struktur dokumen:
- Halaman judul (cover) dengan:
  - Judul: "ANALISIS HUKUM"
  - Subjudul: Judul sesi
  - Tanggal analisis
  - Nama pengguna (opsional)
  - Logo PAUGERAN (kecil)
- Daftar Isi otomatis dengan nomor halaman
- 24 poin laporan sesuai struktur
- Daftar Sumber di akhir
- Lampiran (jika ada)

**19.2.4** Fitur khusus:
- Bookmark PDF untuk setiap poin (navigasi cepat)
- Hyperlink untuk citation (klik untuk lihat detail)
- Hyperlink untuk URL sumber (untuk web)
- Metadata PDF: judul, penulis, subjek, kata kunci
- Kompresi gambar (jika ada) untuk ukuran file optimal

### 19.3 Format DOCX Profesional

**19.3.1** Menggunakan template standar dokumen hukum Indonesia.

**19.3.2** Style Heading 1-3 untuk navigasi.

**19.3.3** Table of Contents otomatis.

**19.3.4** Editable: Pengguna dapat mengedit setelah ekspor.

**19.3.5** Support Track Changes untuk kolaborasi.

**19.3.6** Page numbering dan header/footer.

### 19.4 Template Kustomisasi

**19.4.1** Pengguna dapat memilih template:
- Standar (default)
- Formal (untuk pengadilan)
- Memorandum (untuk internal)
- Opinion Letter (untuk klien)
- Custom (dikonfigurasi pengguna)

**19.4.2** Setiap template memiliki:
- Layout yang berbeda
- Font yang berbeda
- Struktur heading yang berbeda
- Header/footer yang berbeda

**19.4.3** Admin dapat mengunggah template custom untuk tim (jika AUTH_ENABLED=true).

### 19.5 Opsi Export

**19.5.1** Dialog export menampilkan opsi:
- Format: PDF atau DOCX
- Template: pilihan template
- Sertakan log reasoning: ya/tidak
- Sertakan peta keterlacakan: ya/tidak (sebagai gambar)
- Watermark: ya/tidak
- Kop surat: upload gambar (opsional)
- Tanda tangan digital: ya/tidak (opsional, fitur premium)

**19.5.2** Tombol "Preview" menampilkan preview dokumen sebelum export.

**19.5.3** Tombol "Export" memulai proses export dan menawarkan download.

### 19.6 Implementasi Teknis

**19.6.1** PDF generation menggunakan printpdf atau genpdf crate di Rust.

**19.6.2** DOCX generation menggunakan docx-rs crate di Rust.

**19.6.3** File disimpan sementara di `{data_dir}/exports/{user_id}/` dan dihapus setelah 24 jam.

**19.6.4** Download URL memiliki expiry time 1 jam.

### 19.7 Inline Citation dalam Export

**19.7.1** Inline citation ter-embed dalam dokumen yang diekspor.

**19.7.2** Dalam PDF: citation ditampilkan sebagai footnote atau hyperlink.

**19.7.3** Dalam DOCX: citation ditampilkan sebagai comment atau hyperlink.

**19.7.4** Daftar sumber lengkap di akhir dokumen.

---

# BAGIAN III — SPESIFIKASI TEKNIS

---

## 20. ARSITEKTUR SISTEM

### 20.1 Arsitektur Keseluruhan

PAUGERAN dioperasikan sebagai satu binary universal yang menjalankan server HTTP. Binary ini menyajikan antarmuka web dan mesin penalaran hukum dalam satu kesatuan.

```
┌─────────────────────────────────────────────────────────────────┐
│  BINARY PAUGERAN (Rust)                                         │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Axum HTTP Server                                         │ │
│  │  ├── Serve frontend SolidJS (static files)                │ │
│  │  ├── API endpoints                                        │ │
│  │  ├── SSE streaming                                        │ │
│  │  └── Conditional auth middleware (berdasarkan AUTH_ENABLED)│ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Custom Graph Engine (11 Fase)                            │ │
│  │  ├── Node Executor                                        │ │
│  │  ├── Conditional Edge Evaluator                           │ │
│  │  ├── State Machine                                        │ │
│  │  └── Event Streaming                                      │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Multi-Provider LLM Router                                │ │
│  │  ├── Anthropic Client                                     │ │
│  │  ├── OpenAI Client                                        │ │
│  │  ├── OpenAI-Compatible Client                             │ │
│  │  ├── Ollama Client (opsional)                             │ │
│  │  └── Model Selector                                       │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Web Research Module                                      │ │
│  │  ├── Whitelist Manager                                    │ │
│  │  ├── HTTP Client (Reqwest)                                │ │
│  │  ├── HTML Parser (scraper)                                │ │
│  │  └── Content Extractor                                    │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Legal Knowledge Base                                     │ │
│  │  ├── Document Store                                       │ │
│  │  ├── Vector Embeddings                                    │ │
│  │  ├── Semantic Search                                      │ │
│  │  └── Metadata Manager                                     │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  License Manager                                          │ │
│  │  ├── License Validator                                    │ │
│  │  ├── Grace Period Handler                                 │ │
│  │  └── Offline License Support                              │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Data Layer                                               │ │
│  │  ├── SQLite (default) atau PostgreSQL (skala besar)       │ │
│  │  ├── File Storage                                         │ │
│  │  └── Encryption (AES-256-GCM)                             │ │
│  └───────────────────────────────────────────────────────────┘ │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Frontend Static Files (SolidJS build output)             │ │
│  │  ├── Chat Interface                                       │ │
│  │  ├── Session Sidebar                                      │ │
│  │  ├── Traceability Graph                                   │ │
│  │  ├── Report Viewer                                        │ │
│  │  ├── Settings Panel                                       │ │
│  │  ├── Admin Panel (jika AUTH_ENABLED=true)                 │ │
│  │  └── Knowledge Base Manager                               │ │
│  └───────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
         │
         │ HTTP (sama persis untuk semua deployment)
         ▼
┌─────────────────────────────────────────────────────────────────┐
│  BROWSER / WEBVIEW                                              │
│  • Browser biasa (Chrome, Firefox, Safari, Edge)                │
│  • WebView di Tauri (untuk native desktop experience)           │
│  • Mobile browser (responsive)                                  │
└─────────────────────────────────────────────────────────────────┘
         │
         │ HTTPS (Hanya saat panggil LLM API atau validasi lisensi)
         ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYANAN EKSTERNAL                                              │
│  ├── api.anthropic.com (Claude)                                 │
│  ├── api.openai.com (GPT-4o)                                    │
│  ├── api.groq.com, api.together.ai, dll (provider lain)         │
│  ├── license.paugeran.com (server lisensi)                      │
│  └── Situs Hukum Resmi (JDIH, MA - via web scraping)            │
└─────────────────────────────────────────────────────────────────┘
```

### 20.2 Alur Akses Universal

Binary PAUGERAN selalu berjalan sebagai HTTP server. Cara pengguna mengaksesnya bergantung pada deployment:

**Laptop Pribadi:**
```bash
./paugeran
# Server jalan di localhost:3000
# Buka browser ke http://localhost:3000
```

**Railway (Cloud Managed):**
```
Binary jalan di server Railway
Akses via https://paugeran.up.railway.app
```

**VPS Pribadi:**
```
Binary jalan di VPS dengan Caddy sebagai reverse proxy
Akses via https://paugeran.domainanda.com (auto-SSL)
```

**Homelab:**
```
Binary jalan di server rumah
Akses via Tailscale atau Cloudflare Tunnel
```

**Tauri Desktop (Optional Wrapper):**
```
Tauri menjalankan binary sebagai sidecar
Membuka WebView yang connect ke localhost:3000
Memberikan native features (system tray, notifications)
```

**Semua skenario mengakses binary yang SAMA, UI yang SAMA, data format yang SAMA.**

### 20.3 Conditional Auth Middleware

```rust
// Auth middleware hanya aktif jika AUTH_ENABLED=true
let app = if config.auth_enabled {
    app.layer(middleware::from_fn(auth_middleware))
} else {
    app.layer(middleware::from_fn(single_user_middleware))
};
```

### 20.4 License Middleware

**20.4.1** License middleware berjalan di latar belakang dan memeriksa validitas lisensi secara berkala saat PAUGERAN terhubung ke internet.

**20.4.2** Jika lisensi tidak valid, semua endpoint agen dikunci tetapi endpoint data dan pengaturan tetap dapat diakses.

---

## 21. STACK TEKNOLOGI

### 21.1 Backend — Rust

**21.1.1** Framework web: Axum 0.7 atau lebih baru

**21.1.2** Async runtime: Tokio 1.x dengan fitur full

**21.1.3** Database client: SQLx 0.7 atau lebih baru dengan fitur runtime-tokio-rustls, sqlite, dan postgres

**21.1.4** Serialization: Serde 1.x dengan fitur derive

**21.1.5** HTTP client: Reqwest 0.11 atau lebih baru dengan fitur json, stream, dan rustls-tls

**21.1.6** HTML parsing untuk web scraping: scraper crate

**21.1.7** Error handling: Thiserror 1.x

**21.1.8** Logging: Tracing 0.1 atau lebih baru

**21.1.9** Enkripsi: aes-gcm 0.10 atau lebih baru

**21.1.10** Password hashing: argon2 crate

**21.1.11** Random generation: Rand 0.8 atau lebih baru

**21.1.12** UUID: Uuid 1.x dengan fitur v4 dan serde

**21.1.13** Date/time: Chrono 0.4 atau lebih baru dengan fitur serde

**21.1.14** Streaming: Tokio-stream 0.1 atau lebih baru

**21.1.15** PDF parsing: lopdf atau pdf-extract crate

**21.1.16** DOCX parsing: docx-rs crate

**21.1.17** PDF generation: printpdf atau genpdf crate dengan dukungan template profesional

**21.1.18** Vector embeddings: candle atau ort crate untuk inferensi embedding lokal, atau API call untuk embedding remote

### 21.2 Frontend — SolidJS

**21.2.1** Framework: SolidJS dengan fine-grained reactivity

**21.2.2** Build tool: Vite 5 atau lebih baru

**21.2.3** Bahasa: TypeScript 5 atau lebih baru

**21.2.4** Styling: Tailwind CSS 3 atau lebih baru

**21.2.5** Server state: TanStack Query

**21.2.6** Graph visualization: Cytoscape.js

**21.2.7** Rich text editor: TipTap

**21.2.8** Markdown rendering: marked dengan plugin untuk inline citation

**21.2.9** Internationalization: @solid-primitives/i18n

**21.2.10** Command palette: cmdk-solid atau implementasi custom

**21.2.11** Accessibility: @solidjs-aria atau implementasi ARIA manual

### 21.3 Database

**21.3.1** Default: SQLite. Embedded, single file, tidak perlu server terpisah.

**21.3.2** Opsional: PostgreSQL. Untuk deployment skala besar. Auto-detect dari `DATABASE_URL`.

**21.3.3** SQLx mendukung kedua database dengan query yang sama melalui compile-time checking.

**21.3.4** Vector search untuk Legal Knowledge Base menggunakan sqlite-vec extension untuk SQLite atau pgvector untuk PostgreSQL.

### 21.4 Desktop Wrapper — Tauri (Opsional)

**21.4.1** Tauri 2.x sebagai wrapper tipis yang menjalankan binary PAUGERAN sebagai sidecar.

**21.4.2** Membuka WebView yang connect ke localhost.

**21.4.3** Memberikan native features: system tray, native notifications, auto-updater, file dialogs.

### 21.5 License Server (Eksternal)

**21.5.1** Server lisensi PAUGERAN adalah layanan terpisah yang memvalidasi lisensi key.

**21.5.2** Protokol validasi: HTTPS POST dengan payload `{license_key, installation_id, version}`.

**21.5.3** Response: `{valid: bool, expires_at: ISO8601, features: [...], message: string}`.

**21.5.4** Validasi dilakukan secara berkala (setiap 24 jam) saat PAUGERAN online.

**21.5.5** Grace period: 7 hari setelah validasi terakhir jika PAUGERAN offline.

---

## 22. CUSTOM GRAPH ENGINE

### 22.1 Konsep

**22.1.1** Custom Graph Engine adalah pengganti LangGraph yang dibangun native di Rust untuk performa optimal dan integrasi langsung dengan Tokio async runtime.

### 22.2 Komponen Utama

**22.2.1** `AgentGraph`: Definisi graph yang berisi nodes, edges, dan entry point.

**22.2.2** `Node` trait: Interface yang harus diimplementasikan oleh setiap fase agen.

```rust
pub trait Node: Send + Sync {
    fn id(&self) -> NodeId;
    async fn execute(&self, state: &mut AgentState, ctx: &ExecutionContext) -> Result<NodeResult>;
}
```

**22.2.3** `Edge`: Enum dengan varian `Static(NodeId)` dan `Conditional(ConditionalEdgeFn)`.

```rust
pub enum Edge {
    Static(NodeId),
    Conditional(ConditionalEdgeFn),
}

pub type ConditionalEdgeFn = Arc<dyn Fn(&AgentState) -> NodeId + Send + Sync>;
```

**22.2.4** `AgentState`: Struct yang berisi state agen.

```rust
#[derive(Serialize, Deserialize, Clone)]
pub struct AgentState {
    pub session_id: String,
    pub user_id: String,
    pub messages: Vec<Message>,
    pub facts: Vec<Fact>,
    pub documents: Vec<Document>,
    pub user_goals: Vec<String>,
    pub identified_issues: Vec<String>,
    pub retrieved_laws: Vec<LegalRule>,
    pub arguments: Vec<Argument>,
    pub counter_arguments: Vec<Argument>,
    pub traceability_map: HashMap<String, TraceNode>,
    pub current_phase: AgentPhase,
    pub facts_complete: bool,
    pub report_generated: bool,
}
```

**22.2.5** `ExecutionContext`: Struct yang berisi konteks eksekusi.

```rust
pub struct ExecutionContext {
    pub llm_router: Arc<LlmRouter>,
    pub knowledge_base: Arc<KnowledgeBase>,
    pub web_researcher: Arc<WebResearcher>,
    pub db: Arc<Database>,
    pub event_sender: tokio::sync::mpsc::Sender<AgentEvent>,
    pub cancellation_token: CancellationToken,
}
```

**22.2.6** `AgentEvent`: Enum dengan varian event.

```rust
pub enum AgentEvent {
    PhaseStarted { phase: AgentPhase },
    PhaseCompleted { phase: AgentPhase, output: String },
    TokenStreamed { token: String },
    FactExtracted { fact: Fact },
    LawRetrieved { law: LegalRule },
    WebSourceAccessed { url: String, title: String },
    KnowledgeBaseHit { entry_id: String },
    ArgumentGenerated { argument: Argument },
    Error { message: String },
    Completed { report: Report },
}
```

### 22.3 Graph Definition

**22.3.1** Entry point: PAHAM.

**22.3.2** Edge statis:
- PAHAM → TANYA
- TANYA → KONFIRMASI
- RUMUSKAN → TELITI
- TELITI → VERIFIKASI
- VERIFIKASI → NALAR
- NALAR → BANTAH
- BANTAH → UJI
- UJI → SIMPULKAN
- SIMPULKAN → TELUSURI

**22.3.3** Conditional edge dari KONFIRMASI:
- Jika facts_complete true → ke RUMUSKAN
- Jika false → ke TANYA

**22.3.4** Conditional edge dari TELUSURI:
- Jika semua citation valid → ke END
- Jika ada citation tidak valid → ke TELITI

### 22.4 Fitur Wajib

**22.4.1** Node eksekusi async.

**22.4.2** Conditional edges berdasarkan state.

**22.4.3** State persistence ke database setelah setiap node.

**22.4.4** Event streaming ke frontend via SSE.

**22.4.5** Cancellation support melalui CancellationToken.

**22.4.6** Error recovery: node yang gagal bisa di-retry tanpa mengulang dari awal.

**22.4.7** Setiap node harus menghasilkan inline citation untuk setiap klaim hukum.

**22.4.8** Setiap node harus mencatat sumber (Knowledge Base atau Web) untuk setiap informasi yang digunakan.

---

## 23. DATABASE SCHEMA

### 23.1 Tabel Utama

**23.1.1** Tabel `users`:
```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    email TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    name TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'user' CHECK (role IN ('admin', 'user')),
    is_active INTEGER NOT NULL DEFAULT 1,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    last_login DATETIME
);
```

**23.1.2** Tabel `api_keys`:
```sql
CREATE TABLE api_keys (
    id TEXT PRIMARY KEY,
    user_id TEXT NOT NULL,
    provider TEXT NOT NULL,
    encrypted_key TEXT NOT NULL,
    is_active INTEGER DEFAULT 1,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, provider)
);
```

**23.1.3** Tabel `llm_providers`:
```sql
CREATE TABLE llm_providers (
    id TEXT PRIMARY KEY,
    user_id TEXT NOT NULL,
    provider_type TEXT NOT NULL,
    name TEXT NOT NULL,
    endpoint_url TEXT NOT NULL,
    encrypted_api_key TEXT NOT NULL,
    default_model TEXT,
    available_models JSON,
    max_tokens INTEGER,
    timeout_seconds INTEGER,
    priority INTEGER DEFAULT 1,
    is_active INTEGER DEFAULT 1,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.4** Tabel `global_llm_providers`:
```sql
CREATE TABLE global_llm_providers (
    id TEXT PRIMARY KEY,
    provider_type TEXT NOT NULL,
    name TEXT NOT NULL,
    endpoint_url TEXT NOT NULL,
    encrypted_api_key TEXT NOT NULL,
    default_model TEXT,
    available_models JSON,
    max_tokens INTEGER,
    timeout_seconds INTEGER,
    priority INTEGER DEFAULT 1,
    is_active INTEGER DEFAULT 1,
    created_by TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.5** Tabel `global_api_keys`:
```sql
CREATE TABLE global_api_keys (
    id TEXT PRIMARY KEY,
    provider TEXT NOT NULL UNIQUE,
    encrypted_key TEXT NOT NULL,
    created_by TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.6** Tabel `ui_preferences`:
```sql
CREATE TABLE ui_preferences (
    user_id TEXT PRIMARY KEY,
    preferences_json TEXT NOT NULL,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.7** Tabel `setup_state`:
```sql
CREATE TABLE setup_state (
    user_id TEXT PRIMARY KEY,
    is_setup_complete INTEGER DEFAULT 0,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.8** Tabel `chat_sessions`:
```sql
CREATE TABLE chat_sessions (
    id TEXT PRIMARY KEY,
    user_id TEXT NOT NULL,
    title TEXT,
    status TEXT DEFAULT 'created',
    current_phase TEXT,
    facts_complete INTEGER DEFAULT 0,
    selected_model TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.9** Tabel `messages`:
```sql
CREATE TABLE messages (
    id TEXT PRIMARY KEY,
    session_id TEXT REFERENCES chat_sessions(id) ON DELETE CASCADE,
    role TEXT NOT NULL,
    content TEXT NOT NULL,
    phase TEXT,
    metadata JSON,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.10** Tabel `facts`:
```sql
CREATE TABLE facts (
    id TEXT PRIMARY KEY,
    session_id TEXT REFERENCES chat_sessions(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    source TEXT NOT NULL,
    status TEXT NOT NULL,
    relevance TEXT,
    certainty TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.11** Tabel `documents`:
```sql
CREATE TABLE documents (
    id TEXT PRIMARY KEY,
    session_id TEXT REFERENCES chat_sessions(id) ON DELETE CASCADE,
    filename TEXT NOT NULL,
    file_type TEXT NOT NULL,
    file_size INTEGER NOT NULL,
    file_path TEXT NOT NULL,
    uploaded_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.12** Tabel `legal_knowledge_base`:
```sql
CREATE TABLE legal_knowledge_base (
    id TEXT PRIMARY KEY,
    title TEXT NOT NULL,
    source_type TEXT NOT NULL CHECK (source_type IN ('uu','pp','perpres','permen','putusan','doktrin','lainnya')),
    number TEXT,
    year INTEGER,
    effective_date DATE,
    revoked_date DATE,
    status TEXT DEFAULT 'active' CHECK (status IN ('active','amended','revoked')),
    content TEXT NOT NULL,
    metadata JSON,
    added_by TEXT NOT NULL,
    source_url TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.13** Tabel `legal_knowledge_articles`:
```sql
CREATE TABLE legal_knowledge_articles (
    id TEXT PRIMARY KEY,
    knowledge_id TEXT REFERENCES legal_knowledge_base(id) ON DELETE CASCADE,
    article_number TEXT NOT NULL,
    content TEXT NOT NULL,
    embedding BLOB,
    metadata JSON
);
```

**23.1.14** Tabel `legal_knowledge_embeddings`:
```sql
CREATE TABLE legal_knowledge_embeddings (
    id TEXT PRIMARY KEY,
    article_id TEXT REFERENCES legal_knowledge_articles(id) ON DELETE CASCADE,
    embedding BLOB NOT NULL,
    model TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.15** Tabel `license`:
```sql
CREATE TABLE license (
    id INTEGER PRIMARY KEY CHECK (id = 1),
    license_key_encrypted TEXT NOT NULL,
    installation_id TEXT NOT NULL,
    validated_at DATETIME,
    expires_at DATETIME,
    is_valid INTEGER DEFAULT 1,
    last_check_attempt DATETIME,
    grace_period_until DATETIME,
    features JSON,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.16** Tabel `invitations`:
```sql
CREATE TABLE invitations (
    id TEXT PRIMARY KEY,
    email TEXT,
    token TEXT UNIQUE NOT NULL,
    created_by TEXT NOT NULL,
    expires_at DATETIME NOT NULL,
    used_by TEXT,
    used_at DATETIME,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.17** Tabel `system_config`:
```sql
CREATE TABLE system_config (
    key TEXT PRIMARY KEY,
    value TEXT NOT NULL,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.18** Tabel `admin_audit_logs`:
```sql
CREATE TABLE admin_audit_logs (
    id TEXT PRIMARY KEY,
    admin_id TEXT NOT NULL,
    action TEXT NOT NULL,
    target_user_id TEXT,
    details TEXT,
    ip_address TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.19** Tabel `audit_logs`:
```sql
CREATE TABLE audit_logs (
    id TEXT PRIMARY KEY,
    user_id TEXT,
    session_id TEXT,
    action TEXT NOT NULL,
    details JSON,
    ip_address TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.20** Tabel `session_states`:
```sql
CREATE TABLE session_states (
    session_id TEXT PRIMARY KEY REFERENCES chat_sessions(id) ON DELETE CASCADE,
    state_json TEXT NOT NULL,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**23.1.21** Tabel `traceability_edges`:
```sql
CREATE TABLE traceability_edges (
    id TEXT PRIMARY KEY,
    session_id TEXT REFERENCES chat_sessions(id) ON DELETE CASCADE,
    conclusion_id TEXT,
    reason TEXT,
    rule_id TEXT,
    fact_id TEXT,
    evidence_source TEXT,
    source_type TEXT CHECK (source_type IN ('knowledge_base','web','document','user_statement')),
    source_url TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### 23.2 Indexes

```sql
CREATE INDEX idx_messages_session_id ON messages(session_id);
CREATE INDEX idx_facts_session_id ON facts(session_id);
CREATE INDEX idx_documents_session_id ON documents(session_id);
CREATE INDEX idx_traceability_session_id ON traceability_edges(session_id);
CREATE INDEX idx_chat_sessions_user_id ON chat_sessions(user_id);
CREATE INDEX idx_chat_sessions_status ON chat_sessions(status);
CREATE INDEX idx_chat_sessions_updated_at ON chat_sessions(updated_at DESC);
CREATE INDEX idx_api_keys_user_provider ON api_keys(user_id, provider);
CREATE INDEX idx_users_role ON users(role);
CREATE INDEX idx_users_is_active ON users(is_active);
CREATE INDEX idx_invitations_token ON invitations(token);
CREATE INDEX idx_legal_knowledge_source_type ON legal_knowledge_base(source_type);
CREATE INDEX idx_legal_knowledge_status ON legal_knowledge_base(status);
CREATE INDEX idx_legal_knowledge_year ON legal_knowledge_base(year);
CREATE INDEX idx_legal_articles_knowledge_id ON legal_knowledge_articles(knowledge_id);
CREATE INDEX idx_legal_embeddings_article_id ON legal_knowledge_embeddings(article_id);
```

### 23.3 Penyimpanan File

**23.3.1** Struktur:
```
{data_dir}/
├── paugeran.db              # Database
├── .secret                  # AES-256 encryption key (permission 600)
├── documents/
│   ├── {user_id}/
│   │   ├── {session_id_1}/
│   │   │   ├── original/
│   │   │   └── processed/
│   │   └── {session_id_2}/
│   └── ...
├── exports/
│   ├── {user_id}/
│   │   ├── {session_id_1}/
│   │   └── ...
│   └── ...
└── logs/
    ├── paugeran.log
    └── audit.log
```

**23.3.2** File `.secret` berisi kunci enkripsi AES-256 dengan permission 600 di Unix.

### 23.4 Retensi Data

**23.4.1** Sesi aktif: disimpan selama pengguna menggunakan produk.

**23.4.2** Sesi archived: disimpan tanpa batas.

**23.4.3** Sesi deleted: dihapus permanen segera.

**23.4.4** Legal Knowledge Base: disimpan sampai dihapus oleh admin.

**23.4.5** Audit logs: disimpan 2 tahun.

**23.4.6** API keys: disimpan sampai dihapus pengguna.

**23.4.7** Preferensi UI: disimpan sampai di-reset pengguna.

**23.4.8** Lisensi: disimpan permanen, diperbarui saat validasi.

---

## 24. API SPECIFICATION

### 24.1 Base URL

**24.1.1** Local: `http://localhost:{PORT}/api/v1`

**24.1.2** Cloud: `https://{domain}/api/v1`

### 24.2 Autentikasi

**24.2.1** Ketika `AUTH_ENABLED=false`: Tidak ada header Authorization yang diperlukan.

**24.2.2** Ketika `AUTH_ENABLED=true`: Header `Authorization: Bearer {jwt_token}` dan `Content-Type: application/json`.

### 24.3 Middleware Lisensi

**24.3.1** Semua endpoint agen (sessions, messages, analysis) memerlukan lisensi valid.

**24.3.2** Jika lisensi tidak valid, endpoint mengembalikan HTTP 402 Payment Required dengan pesan yang jelas.

**24.3.3** Endpoint data (settings, backup, export lama) tetap dapat diakses tanpa lisensi valid.

### 24.4 Endpoint Setup

**24.4.1** `GET /setup/status`
```json
Response:
{
  "is_setup_complete": true,
  "has_license": true,
  "has_llm_providers": true
}
```

**24.4.2** `POST /setup/complete`
```json
Response:
{
  "message": "Setup selesai",
  "redirect": "/"
}
```

### 24.5 Endpoint Lisensi

**24.5.1** `GET /license/status`
```json
Response:
{
  "status": "active",
  "type": "personal",
  "expires_at": "2027-08-28T00:00:00Z",
  "features": ["unlimited_sessions", "knowledge_base", "export_pdf"],
  "masked_key": "PAUG-****-****-1234"
}
```

**24.5.2** `POST /license/activate`
```json
Request:
{
  "license_key": "PAUG-XXXX-XXXX-XXXX"
}

Response:
{
  "message": "License activated successfully",
  "expires_at": "2027-08-28T00:00:00Z"
}
```

**24.5.3** `POST /license/validate`
```json
Response:
{
  "valid": true,
  "message": "License is valid"
}
```

**24.5.4** `POST /license/import-offline`
```
Request: multipart/form-data
- file: license.offline

Response:
{
  "message": "Offline license imported",
  "expires_at": "2027-08-28T00:00:00Z"
}
```

### 24.6 Endpoint Auth (hanya jika AUTH_ENABLED=true)

**24.6.1** `POST /auth/register`
```json
Request:
{
  "email": "user@example.com",
  "password": "secure_password",
  "name": "John Doe",
  "invite_code": "abc123"  // wajib jika bukan user pertama
}

Response:
{
  "message": "User registered",
  "user_id": "uuid",
  "role": "admin"  // atau "user"
}
```

**24.6.2** `POST /auth/login`
```json
Request:
{
  "email": "user@example.com",
  "password": "secure_password"
}

Response:
{
  "access_token": "jwt",
  "refresh_token": "jwt",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "role": "admin"
  }
}
```

### 24.7 Endpoint Settings — LLM Providers

**24.7.1** `GET /settings/providers`
```json
Response:
{
  "providers": [
    {
      "id": "uuid",
      "provider_type": "openai_compatible",
      "name": "Groq Llama 3.3",
      "endpoint_url": "https://api.groq.com/openai/v1",
      "default_model": "llama-3.3-70b-versatile",
      "available_models": ["llama-3.3-70b-versatile", "mixtral-8x7b-32768"],
      "priority": 1,
      "is_active": true
    }
  ]
}
```

**24.7.2** `POST /settings/providers`
```json
Request:
{
  "provider_type": "openai_compatible",
  "name": "Groq Llama 3.3",
  "endpoint_url": "https://api.groq.com/openai/v1",
  "api_key": "gsk_...",
  "default_model": "llama-3.3-70b-versatile",
  "priority": 1
}

Response:
{
  "id": "uuid",
  "message": "Provider added"
}
```

**24.7.3** `PATCH /settings/providers/{id}`
```json
Request:
{
  "name": "Updated Name",
  "priority": 2
}

Response:
{
  "message": "Provider updated"
}
```

**24.7.4** `DELETE /settings/providers/{id}`
```json
Response:
{
  "message": "Provider deleted"
}
```

**24.7.5** `POST /settings/providers/{id}/test`
```json
Response:
{
  "success": true,
  "message": "Connection successful",
  "available_models": ["model1", "model2"]
}
```

**24.7.6** `POST /settings/providers/{id}/detect-models`
```json
Response:
{
  "models": ["model1", "model2", "model3"]
}
```

### 24.8 Endpoint Settings — API Keys (Legacy)

**24.8.1** `GET /settings/api-keys`
```json
Response:
{
  "providers": [
    {
      "provider": "anthropic",
      "is_configured": true,
      "masked_key": "sk-ant-****abcd"
    }
  ]
}
```

**24.8.2** `POST /settings/api-keys`
```json
Request:
{
  "provider": "anthropic",
  "api_key": "sk-ant-..."
}

Response:
{
  "message": "API key untuk anthropic berhasil disimpan"
}
```

**24.8.3** `DELETE /settings/api-keys/{provider}`
```json
Response:
{
  "message": "API key anthropic dihapus"
}
```

### 24.9 Endpoint Settings — UI Preferences

**24.9.1** `GET /settings/preferences`
```json
Response:
{
  "theme": "dark",
  "font_size": "medium",
  "content_width": "medium",
  "sidebar_position": "left",
  "ui_language": "id",
  "font_family": "sans-serif",
  "animations": "full",
  "date_format": "id-formal",
  "sound_notifications": true,
  "density": "comfortable"
}
```

**24.9.2** `PUT /settings/preferences`
```json
Request:
{
  "theme": "dark",
  "font_size": "large"
}

Response:
{
  "message": "Preferensi diperbarui",
  "preferences": { ... }
}
```

**24.9.3** `POST /settings/preferences/reset`
```json
Response:
{
  "message": "Preferensi direset ke default",
  "preferences": { ... }
}
```

### 24.10 Endpoint Legal Knowledge Base

**24.10.1** `GET /knowledge`
```json
Response:
{
  "entries": [
    {
      "id": "uuid",
      "title": "Undang-Undang Nomor 13 Tahun 2003",
      "source_type": "uu",
      "number": "13",
      "year": 2003,
      "status": "active",
      "added_at": "2026-08-28T10:00:00Z"
    }
  ],
  "total": 1
}
```

**24.10.2** `POST /knowledge`
```json
Request:
{
  "title": "Undang-Undang Nomor 13 Tahun 2003",
  "source_type": "uu",
  "number": "13",
  "year": 2003,
  "content": "...",
  "source_url": "https://jdih.setkab.go.id/..."
}

Response:
{
  "id": "uuid",
  "message": "Knowledge base entry added"
}
```

**24.10.3** `GET /knowledge/{id}`
```json
Response:
{
  "id": "uuid",
  "title": "...",
  "source_type": "uu",
  "content": "...",
  "articles": [
    {
      "id": "uuid",
      "article_number": "1",
      "content": "..."
    }
  ]
}
```

**24.10.4** `PATCH /knowledge/{id}`
```json
Request:
{
  "status": "revoked",
  "revoked_date": "2026-01-01"
}

Response:
{
  "message": "Knowledge base entry updated"
}
```

**24.10.5** `DELETE /knowledge/{id}`
```json
Response:
{
  "message": "Knowledge base entry deleted"
}
```

**24.10.6** `POST /knowledge/{id}/refresh`
```json
Response:
{
  "message": "Knowledge base entry refreshed from source"
}
```

**24.10.7** `POST /knowledge/import`
```
Request: multipart/form-data
- file: regulations.json

Response:
{
  "imported": 10,
  "failed": 0,
  "message": "Import completed"
}
```

**24.10.8** `POST /knowledge/search`
```json
Request:
{
  "query": "wanprestasi kontrak",
  "limit": 10
}

Response:
{
  "results": [
    {
      "id": "uuid",
      "title": "...",
      "article_number": "1238",
      "content": "...",
      "relevance_score": 0.95
    }
  ]
}
```

### 24.11 Endpoint Sessions

**24.11.1** `GET /sessions`
```json
Response:
{
  "sessions": [
    {
      "id": "uuid",
      "title": "Sengketa Tanah",
      "status": "active",
      "current_phase": "NALAR",
      "selected_model": "claude-3-5-sonnet",
      "created_at": "2026-08-28T10:00:00Z",
      "updated_at": "2026-08-28T11:30:00Z",
      "last_message_preview": "Saya memahami masalah Anda..."
    }
  ],
  "total": 1
}
```

**24.11.2** `POST /sessions`
```json
Request:
{
  "title": "Sengketa Kontrak",
  "selected_model": "gpt-4o"
}

Response:
{
  "id": "uuid",
  "title": "Sengketa Kontrak",
  "status": "created",
  "selected_model": "gpt-4o",
  "created_at": "2026-08-28T12:00:00Z"
}
```

**24.11.3** `GET /sessions/{session_id}`
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

**24.11.4** `PATCH /sessions/{session_id}`
```json
Request:
{
  "title": "Judul Baru",
  "status": "archived",
  "selected_model": "claude-3-5-sonnet"
}

Response:
{
  "message": "Sesi diperbarui"
}
```

**24.11.5** `DELETE /sessions/{session_id}`
```json
Response:
{
  "message": "Sesi dihapus permanen"
}
```

**24.11.6** `POST /sessions/{session_id}/duplicate`
```json
Response:
{
  "new_session_id": "uuid",
  "message": "Sesi diduplikat"
}
```

### 24.12 Endpoint Messages

**24.12.1** `POST /sessions/{session_id}/messages`
```json
Request:
{
  "content": "Saya membeli tanah pada Januari 2024"
}

Response:
{
  "message_id": "uuid",
  "session_id": "uuid",
  "status": "processing",
  "phase": "TANYA"
}
```

**24.12.2** `GET /sessions/{session_id}/messages/stream`
```
Response: Server-Sent Events (SSE)

data: {"type": "phase", "phase": "PAHAM"}

data: {"type": "token", "content": "Saya"}

data: {"type": "token", "content": " memahami"}

data: {"type": "complete", "session_id": "uuid"}
```

**24.12.3** `POST /sessions/{session_id}/cancel`
```json
Response:
{
  "message": "Eksekusi dibatalkan"
}
```

### 24.13 Endpoint Documents

**24.13.1** `POST /sessions/{session_id}/documents`
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

**24.13.2** `GET /sessions/{session_id}/documents`
```json
Response:
{
  "documents": [...]
}
```

**24.13.3** `DELETE /sessions/{session_id}/documents/{document_id}`
```json
Response:
{
  "message": "Dokumen dihapus"
}
```

### 24.14 Endpoint Analysis

**24.14.1** `POST /sessions/{session_id}/analysis/confirm`
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

**24.14.2** `POST /sessions/{session_id}/analysis/start-reasoning`
```json
Response:
{
  "status": "started",
  "phase": "TELITI",
  "estimated_time": 180
}
```

**24.14.3** `GET /sessions/{session_id}/analysis/report`
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
  "generated_at": "2026-08-28T12:00:00Z"
}
```

**24.14.4** `POST /sessions/{session_id}/analysis/export`
```json
Request:
{
  "format": "pdf",
  "template": "formal",
  "include_reasoning_log": true,
  "include_traceability_graph": true
}

Response:
{
  "download_url": "/exports/...",
  "expires_at": "2026-08-28T13:00:00Z"
}
```

**24.14.5** `POST /sessions/{session_id}/analysis/save-to-knowledge`
```json
Request:
{
  "regulation_ids": ["uuid1", "uuid2"]
}

Response:
{
  "saved": 2,
  "message": "Regulations saved to knowledge base"
}
```

### 24.15 Endpoint Traceability

**24.15.1** `GET /sessions/{session_id}/traceability`
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

### 24.16 Endpoint Admin (hanya jika AUTH_ENABLED=true dan role=admin)

**24.16.1** `GET /admin/users`
```json
Response:
{
  "users": [
    {
      "id": "uuid",
      "email": "admin@example.com",
      "name": "Admin User",
      "role": "admin",
      "is_active": true,
      "created_at": "2026-08-28T10:00:00Z",
      "last_login": "2026-08-28T11:30:00Z"
    }
  ]
}
```

**24.16.2** `PATCH /admin/users/{user_id}`
```json
Request:
{
  "role": "user",
  "is_active": false
}

Response:
{
  "message": "User diperbarui"
}
```

**24.16.3** `DELETE /admin/users/{user_id}`
```json
Response:
{
  "message": "User dihapus permanen"
}
```

**24.16.4** `POST /admin/invitations`
```json
Request:
{
  "email": "newuser@example.com",
  "expires_in_days": 7,
  "max_uses": 1
}

Response:
{
  "invitation_id": "uuid",
  "invite_link": "https://paugeran.com/invite/abc123",
  "expires_at": "2026-09-04T10:00:00Z"
}
```

**24.16.5** `GET /admin/stats/overview`
```json
Response:
{
  "total_users": 10,
  "active_users": 8,
  "total_sessions": 247,
  "total_analyses": 189,
  "estimated_api_cost": 47.30
}
```

**24.16.6** `GET /admin/providers`
```json
Response:
{
  "providers": [...]
}
```

**24.16.7** `POST /admin/providers`
```json
Request:
{
  "provider_type": "anthropic",
  "name": "Global Anthropic",
  "endpoint_url": "https://api.anthropic.com",
  "api_key": "sk-ant-...",
  "priority": 1
}

Response:
{
  "id": "uuid",
  "message": "Global provider added"
}
```

**24.16.8** `PATCH /admin/providers/{id}`
```json
Request:
{
  "priority": 2
}

Response:
{
  "message": "Global provider updated"
}
```

**24.16.9** `DELETE /admin/providers/{id}`
```json
Response:
{
  "message": "Global provider deleted"
}
```

**24.16.10** `GET /admin/knowledge`
```json
Response:
{
  "entries": [...]
}
```

**24.16.11** `POST /admin/knowledge/import`
```
Request: multipart/form-data
- file: regulations.json

Response:
{
  "imported": 50,
  "failed": 0,
  "message": "Bulk import completed"
}
```

**24.16.12** `GET /admin/license`
```json
Response:
{
  "status": "active",
  "type": "team",
  "expires_at": "2027-08-28T00:00:00Z",
  "max_users": 10,
  "current_users": 8
}
```

**24.16.13** `PUT /admin/config`
```json
Request:
{
  "allow_self_registration": false,
  "require_email_verification": true,
  "max_sessions_per_user": null,
  "max_uploads_per_session": 50,
  "web_research_whitelist": ["jdih.setkab.go.id", "mahkamahagung.go.id"]
}

Response:
{
  "message": "Konfigurasi diperbarui",
  "config": { ... }
}
```

### 24.17 Endpoint Health

**24.17.1** `GET /health`
```json
Response:
{
  "status": "healthy",
  "version": "1.0.0",
  "database": "connected",
  "auth_enabled": false,
  "license_valid": true,
  "timestamp": "2026-08-28T10:00:00Z"
}
```

### 24.18 Format Error

**24.18.1** Format:
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
    "timestamp": "2026-08-28T10:00:00Z",
    "request_id": "req-uuid"
  }
}
```

**24.18.2** Error codes:
- VALIDATION_ERROR
- NOT_FOUND
- FORBIDDEN
- UNAUTHORIZED
- LLM_ERROR
- NO_LLM_PROVIDER
- NO_LICENSE
- LICENSE_EXPIRED
- LICENSE_INVALID
- CANCELLED
- INVITE_CODE_REQUIRED
- INVITE_CODE_INVALID
- CANNOT_DEMOTE_SELF
- LAST_ADMIN
- KNOWLEDGE_BASE_ERROR
- INTERNAL_ERROR

### 24.19 Rate Limiting

**24.19.1** Login: 5 requests per minute per IP

**24.19.2** Register: 3 requests per hour per IP

**24.19.3** Create session: 60 per jam per user

**24.19.4** Send message: 120 per menit per sesi

**24.19.5** Upload document: 20 per jam per sesi

**24.19.6** Export report: 60 per jam per user

**24.19.7** Knowledge base search: 200 per jam per user

---

## 25. FRONTEND SPECIFICATION

### 25.1 Struktur Direktori

```
apps/web/
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── components/
│   │   ├── chat/
│   │   │   ├── ChatWindow.tsx
│   │   │   ├── MessageBubble.tsx
│   │   │   ├── SessionSidebar.tsx
│   │   │   ├── InputArea.tsx
│   │   │   └── PhaseIndicator.tsx
│   │   ├── settings/
│   │   │   ├── SettingsPanel.tsx
│   │   │   ├── ThemeSelector.tsx
│   │   │   ├── FontSelector.tsx
│   │   │   ├── LanguageSelector.tsx
│   │   │   ├── AccessibilitySettings.tsx
│   │   │   ├── LLMProviderManager.tsx
│   │   │   ├── ApiKeyManager.tsx
│   │   │   ├── KnowledgeBaseManager.tsx
│   │   │   ├── WebResearchSettings.tsx
│   │   │   ├── LicenseManager.tsx
│   │   │   └── DataManager.tsx
│   │   ├── traceability/
│   │   │   └── TraceGraph.tsx
│   │   ├── report/
│   │   │   └── ReportViewer.tsx
│   │   ├── admin/
│   │   │   ├── UserManagement.tsx
│   │   │   ├── InvitationManager.tsx
│   │   │   ├── GlobalProviderManager.tsx
│   │   │   ├── GlobalKnowledgeBase.tsx
│   │   │   ├── SystemConfig.tsx
│   │   │   └── Statistics.tsx
│   │   ├── knowledge/
│   │   │   ├── KnowledgeBaseList.tsx
│   │   │   ├── KnowledgeBaseDetail.tsx
│   │   │   └── KnowledgeBaseImport.tsx
│   │   ├── license/
│   │   │   ├── LicenseActivation.tsx
│   │   │   └── LicenseStatus.tsx
│   │   ├── setup/
│   │   │   └── SetupWizard.tsx
│   │   ├── common/
│   │   │   ├── CommandPalette.tsx
│   │   │   ├── Toast.tsx
│   │   │   ├── Modal.tsx
│   │   │   ├── Drawer.tsx
│   │   │   └── Breadcrumb.tsx
│   │   └── ui/
│   │       ├── Button.tsx
│   │       ├── Input.tsx
│   │       ├── Select.tsx
│   │       ├── Checkbox.tsx
│   │       ├── Radio.tsx
│   │       ├── Switch.tsx
│   │       ├── Slider.tsx
│   │       ├── Tabs.tsx
│   │       ├── Accordion.tsx
│   │       ├── Tooltip.tsx
│   │       ├── Dropdown.tsx
│   │       ├── Avatar.tsx
│   │       ├── Badge.tsx
│   │       ├── Card.tsx
│   │       ├── Alert.tsx
│   │       └── Skeleton.tsx
│   ├── pages/
│   │   ├── ChatPage.tsx
│   │   ├── SettingsPage.tsx
│   │   ├── SetupPage.tsx
│   │   ├── LicenseActivationPage.tsx
│   │   ├── KnowledgeBasePage.tsx
│   │   └── AdminPage.tsx
│   ├── lib/
│   │   ├── api.ts
│   │   ├── sse.ts
│   │   ├── preferences.ts
│   │   ├── auth.ts
│   │   ├── license.ts
│   │   └── types.ts
│   ├── locales/
│   │   ├── id.json
│   │   └── en.json
│   ├── styles/
│   │   ├── themes/
│   │   │   ├── light.css
│   │   │   ├── dark.css
│   │   │   ├── sepia.css
│   │   │   └── high-contrast.css
│   │   └── globals.css
│   └── hooks/
│       ├── useSessions.ts
│       ├── useMessages.ts
│       ├── usePreferences.ts
│       ├── useLicense.ts
│       ├── useKnowledgeBase.ts
│       └── useCommandPalette.ts
├── index.html
├── vite.config.ts
├── tailwind.config.js
├── tsconfig.json
└── package.json
```

### 25.2 State Management

**25.2.1** Global state menggunakan SolidJS signals dan stores.

**25.2.2** Server state menggunakan TanStack Query.

**25.2.3** State utama:
- `user`: Informasi user saat ini
- `license`: Status lisensi
- `sessions`: Daftar sesi
- `currentSession`: Sesi aktif
- `messages`: Pesan dalam sesi aktif
- `preferences`: Preferensi UI
- `providers`: LLM providers yang dikonfigurasi
- `knowledgeBase`: Legal Knowledge Base entries
- `admin`: Admin state (jika role=admin)

### 25.3 Routing

**25.3.1** Menggunakan SolidJS Router.

**25.3.2** Routes:
- `/` — Redirect ke `/chat` atau `/setup`
- `/setup` — Setup wizard
- `/license/activate` — Aktivasi lisensi
- `/chat` — Chat utama
- `/chat/:sessionId` — Sesi spesifik
- `/settings` — Pengaturan
- `/knowledge` — Legal Knowledge Base
- `/admin` — Admin panel (jika role=admin)

### 25.4 API Client

**25.4.1** Menggunakan fetch API dengan wrapper.

**25.4.2** Fitur:
- Automatic JWT token injection
- Error handling terpusat
- Request/response typing
- Retry logic untuk transient errors
- Request cancellation

### 25.5 SSE Client

**25.5.1** Menggunakan EventSource API.

**25.5.2** Fitur:
- Automatic reconnection
- Event parsing
- State updates
- Error handling

---

# BAGIAN IV — SPESIFIKASI PERILAKU AGEN

---

## 26. SIKLUS 11 FASE

### 26.1 Diagram Siklus

```
PAHAM → TANYA → KONFIRMASI → (kondisional) → RUMUSKAN → TELITI → 
VERIFIKASI → NALAR → BANTAH → UJI → SIMPULKAN → TELUSURI → END
```

### 26.2 Formula Perilaku Inti

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

---

## 27. DETAIL SETIAP FASE

### 27.1 Fase PAHAM

**27.1.1** Input: Pesan pertama pengguna

**27.1.2** Proses: Ekstrak informasi dasar meliputi pihak, objek, kronologi

**27.1.3** Output: Fakta awal dan tujuan pengguna

**27.1.4** Model: Claude Haiku atau GPT-4o-mini (cepat)

**27.1.5** Durasi target: kurang dari 5 detik

### 27.2 Fase TANYA

**27.2.1** Input: Fakta awal

**27.2.2** Proses: Identifikasi informasi yang hilang, generate pertanyaan adaptif

**27.2.3** Output: 1 hingga 3 pertanyaan klarifikasi

**27.2.4** Model: Claude Haiku atau GPT-4o-mini

**27.2.5** Durasi target: kurang dari 5 detik

### 27.3 Fase KONFIRMASI

**27.3.1** Input: Semua fakta yang terkumpul

**27.3.2** Proses: Susun rekonstruksi masalah, tampilkan ke pengguna

**27.3.3** Output: Ringkasan pemahaman dengan tombol Setuju atau Revisi

**27.3.4** Model: Claude Haiku atau GPT-4o-mini

**27.3.5** Durasi target: kurang dari 5 detik

**27.3.6** Conditional: Jika facts_complete false maka kembali ke TANYA. Jika true maka lanjut ke RUMUSKAN.

### 27.4 Fase RUMUSKAN

**27.4.1** Input: Fakta yang dikonfirmasi

**27.4.2** Proses: Identifikasi isu hukum, bidang hukum, yurisdiksi

**27.4.3** Output: Daftar isu hukum dan klasifikasi masalah

**27.4.4** Model: Claude Sonnet atau GPT-4o (mendalam)

**27.4.5** Durasi target: kurang dari 10 detik

### 27.5 Fase TELITI

**27.5.1** Input: Isu hukum

**27.5.2** Proses: Cari peraturan, putusan, doktrin yang relevan melalui RAG

**27.5.3** Output: Daftar sumber hukum dengan metadata

**27.5.4** Model: GPT-4o JSON mode atau Claude Sonnet

**27.5.5** Durasi target: kurang dari 60 detik

**27.5.6** Urutan pencarian:
1. Legal Knowledge Base internal (paling cepat, offline)
2. Dokumen yang diunggah pengguna
3. Penelitian web ke situs pemerintah dan sumber tepercaya

**27.5.7** Setiap sumber yang ditemukan harus dicatat dengan:
- Tipe sumber (knowledge_base, document, web)
- Metadata lengkap (nomor, tahun, pasal)
- URL sumber (untuk web)
- Status keberlakuan

**27.5.8** Setelah penelitian, PAUGERAN menawarkan opsi "Simpan ke Knowledge Base" untuk peraturan yang ditemukan dari web.

### 27.6 Fase VERIFIKASI

**27.6.1** Input: Sumber hukum yang ditemukan

**27.6.2** Proses: Periksa keberlakuan, status, tanggal efektif

**27.6.3** Output: Sumber yang terverifikasi dengan status

**27.6.4** Model: GPT-4o atau Claude Sonnet

**27.6.5** Durasi target: kurang dari 15 detik

### 27.7 Fase NALAR

**27.7.1** Input: Fakta dan sumber hukum terverifikasi

**27.7.2** Proses: Terapkan hukum pada fakta, analisis unsur, bangun argumen

**27.7.3** Output: Argumen pendukung dengan inline citation

**27.7.4** Model: Claude Sonnet/Opus atau GPT-4o

**27.7.5** Durasi target: kurang dari 30 detik

### 27.8 Fase BANTAH

**27.8.1** Input: Argumen pendukung

**27.8.2** Proses: Cari kelemahan, pengecualian, kontra-argumen, putusan berlawanan

**27.8.3** Output: Argumen berlawanan dengan inline citation

**27.8.4** Model: Claude Sonnet/Opus atau GPT-4o, menggunakan model berbeda dari NALAR untuk menghindari bias

**27.8.5** Durasi target: kurang dari 30 detik

### 27.9 Fase UJI

**27.9.1** Input: Argumen pendukung dan berlawanan

**27.9.2** Proses: Evaluasi kekuatan kedua sisi, identifikasi ketidakpastian

**27.9.3** Output: Penilaian berimbang dengan tingkat kepastian

**27.9.4** Model: Claude Sonnet atau GPT-4o

**27.9.5** Durasi target: kurang dari 20 detik

### 27.10 Fase SIMPULKAN

**27.10.1** Input: Hasil pengujian

**27.10.2** Proses: Susun kesimpulan dengan kategori kepastian

**27.10.3** Output: Kesimpulan hukum dengan inline citation

**27.10.4** Model: Claude Sonnet atau GPT-4o

**27.10.5** Durasi target: kurang dari 15 detik

### 27.11 Fase TELUSURI

**27.11.1** Input: Kesimpulan dan semua data

**27.11.2** Proses: Bangun peta keterlacakan, validasi setiap citation

**27.11.3** Output: Peta keterlacakan lengkap dan laporan 24 poin

**27.11.4** Model: GPT-4o JSON mode atau Claude Sonnet

**27.11.5** Durasi target: kurang dari 20 detik

**27.11.6** Conditional: Jika ada kesimpulan tanpa citation valid maka kembali ke TELITI. Jika semua valid maka END.

---

## 28. KONTRAK PERILAKU

### 28.1 Karakter Perilaku

**28.1.1** PAUGERAN adalah:
- Sabar: Tidak terburu-buru memberikan kesimpulan
- Teliti: Memeriksa setiap fakta dan sumber
- Jujur: Mengakui ketidakpastian dan keterbatasan
- Berimbang: Menyajikan kedua sisi argumentasi
- Transparan: Menunjukkan proses penalaran
- Dapat Dipertanggungjawabkan: Setiap klaim dapat ditelusuri
- Hormat: Menghormati preferensi pengguna
- Cepat: Berperforma tinggi melalui Rust
- Aman: Memory-safe dan bebas dari kelas bug umum

**28.1.2** PAUGERAN bukan:
- Otoriter: Memaksakan interpretasi tunggal
- Spekulatif: Memberikan jawaban tanpa dasar
- Bias: Hanya mencari yang mendukung
- Hitam-putih: Mengabaikan nuansa hukum
- Tertutup: Menyembunyikan proses
- Intrusif: Mengubah pengaturan tanpa izin
- Lambat: Overhead karena garbage collection atau interpreter

### 28.2 Respons terhadap Situasi

**28.2.1** Jika informasi tidak lengkap:
PAUGERAN harus menyatakan informasi apa yang masih kurang dan mengapa informasi tersebut diperlukan.

**28.2.2** Jika norma konflik:
PAUGERAN harus menampilkan konflik, menjelaskan kedua norma, dan menjelaskan bagaimana konflik diselesaikan dalam praktik.

**28.2.3** Jika kepastian rendah:
PAUGERAN harus menyatakan tingkat kepastian, alasan ketidakpastian, dan kondisi yang dapat mengubah kesimpulan.

**28.2.4** Jika tidak ada sumber:
PAUGERAN harus menyatakan bahwa sumber tidak ditemukan, menjelaskan kemungkinan penyebab, dan menyarankan pendekatan alternatif.

**28.2.5** Jika API key tidak dikonfigurasi:
PAUGERAN harus meminta pengguna memasukkan API key melalui halaman Pengaturan.

**28.2.6** Jika lisensi tidak valid:
PAUGERAN harus menyatakan bahwa lisensi tidak valid dan meminta pengguna mengaktivasi lisensi melalui halaman Pengaturan.

**28.2.7** Jika eksekusi dibatalkan:
PAUGERAN harus menyatakan bahwa analisis dibatalkan dan data yang sudah terkumpul tetap tersimpan.

**28.2.8** Jika situs web tidak dapat diakses:
PAUGERAN harus menyatakan situs mana yang tidak dapat diakses dan melanjutkan dengan sumber alternatif.

**28.2.9** Jika provider LLM gagal:
PAUGERAN harus mencoba provider fallback dan menyatakan kepada pengguna jika semua provider gagal.

---

## 29. STANDAR BAHASA

### 29.1 Prinsip Bahasa

**29.1.1** PAUGERAN harus menggunakan Bahasa Indonesia yang baku untuk analisis hukum.

**29.1.2** PAUGERAN harus menggunakan istilah hukum yang benar sesuai KUHP, KUHPerdata, dan UU terkait.

**29.1.3** PAUGERAN harus menjelaskan istilah teknis ketika pertama kali digunakan.

**29.1.4** PAUGERAN harus menghindari jargon yang tidak perlu.

**29.1.5** PAUGERAN harus membedakan dengan tegas antara fakta, hukum, interpretasi, dan opini.

**29.1.6** PAUGERAN tidak boleh menggunakan bahasa yang terlalu informal atau sengaja sulit dipahami.

### 29.2 Standar Gaya

**29.2.1** Bahasa ahli hukum yang dapat dipahami orang awam.

### 29.3 Istilah Hukum Standar

**29.3.1** Gunakan istilah baku:
- Wanprestasi (bukan ingkar janji)
- Perjanjian (bukan kesepakatan untuk konteks formal)
- Para pihak (bukan mereka)
- Gugatan (bukan tuntutan untuk perdata)
- Tergugat/Penggugat
- Eksepsi (bukan keberatan)
- Rekonvensi (bukan gugatan balik)

### 29.4 Format Penulisan

**29.4.1** Format penulisan peraturan:
- "Pasal 1338 Kitab Undang-Undang Hukum Perdata"
- "Undang-Undang Nomor 5 Tahun 1986"

**29.4.2** Format penulisan putusan:
- "Putusan Mahkamah Agung Nomor 123 K/Pdt/2020"

**29.4.3** Format tanggal default:
- "28 Agustus 2026"

### 29.5 Inline Citation

**29.5.1** Dalam teks chat:
```
Berdasarkan Pasal 1338 Kitab Undang-Undang Hukum Perdata, perjanjian yang dibuat secara sah berlaku sebagai undang-undang bagi mereka yang membuatnya. [sumber: KUHPerdata, Pasal 1338]
```

**29.5.2** Dalam laporan:
```
Dasar hukum utama dalam kasus ini adalah Undang-Undang Nomor 13 Tahun 2003 tentang Ketenagakerjaan, khususnya Pasal 158 yang mengatur tentang pemutusan hubungan kerja. [sumber: UU 13/2003, Pasal 158, diakses dari JDIH Setkab pada 28 Agustus 2026]
```

**29.5.3** Format standar inline citation:
- Nama peraturan lengkap
- Nomor dan tahun
- Pasal/angka/bagian spesifik
- Sumber (Knowledge Base, web URL, atau dokumen pengguna)
- Tanggal akses (untuk web)

---

## 30. STANDAR KETERLACAKAN

### 30.1 Struktur Keterlacakan

**30.1.1** Setiap kesimpulan material harus mempunyai struktur:
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

### 30.2 Validasi Keterlacakan

**30.2.1** Checklist validasi:
- Setiap kesimpulan memiliki minimal 1 alasan
- Setiap alasan memiliki minimal 1 kaidah hukum
- Setiap kaidah memiliki sumber yang valid
- Setiap sumber memiliki status keberlakuan
- Setiap kaidah terhubung ke minimal 1 fakta
- Setiap fakta memiliki status verifikasi
- Setiap kesimpulan memiliki kontraargumentasi

### 30.3 Penandaan Keterlacakan Tidak Lengkap

**30.3.1** Jika salah satu elemen tidak tersedia:
```
⚠️ KETERLACAKAN TIDAK LENGKAP

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

# BAGIAN V — SPESIFIKASI ANTARMUKA

---

## 31. LAYOUT & NAVIGASI

### 31.1 Layout Utama

```
┌─────────────────────────────────────────────────────────┐
│  PAUGERAN                    [🔍] [🎨] [⚙️ Pengaturan]  │
│  ─────────────────────────────────────────────────────  │
│                                                         │
│  ┌──────────┐  ─────────────────────────────────────┐ │
│  │ Riwayat  │  │  Chat Interface                     │ │
│  │ Sesi     │  │                                     │ │
│  │          │  │  ┌─────────────────────────────┐   │ │
│  │ [+ Sesi  │  │  │  Judul Sesi (editable)      │   │ │
│  │  Baru]   │  │  │  [📊 Peta] [📄 Laporan]     │   │ │
│  │          │  │  └─────────────────────────────┘   │ │
│  │ ▼ Hari   │  │                                     │ │
│  │  Ini     │  │  ┌─────────────────────────────┐   │ │
│  │  • Sengketa│ │  │  🧑 User                    │   │ │
│  │    Tanah │  │  │  Saya membeli tanah...      │   │ │
│  │  • PHK   │  │  └─────────────────────────────┘   │ │
│  │          │  │                                     │ │
│  │ ▼ Ming-  │  │  ┌─────────────────────────────┐   │ │
│  │  gu Lalu │  │  │  🤖 PAUGERAN (Fase: PAHAM)   │   │ │
│  │  • Kontrak│ │  │  Saya memahami masalah...    │   │ │
│  │    Kerja │  │  └─────────────────────────────┘   │ │
│  │          │  │                                     │ │
│  │ ▼ Lebih  │  │  ┌─────────────────────────────┐   │ │
│  │  Lama    │  │  │  [Ketik pesan...]  [📎][➤] │   │ │
│  │  • Warisan│ │  └─────────────────────────────┘   │ │
│  └──────────┘  └─────────────────────────────────────┘ │
│                                                         │
│  Status Bar: 🔐 Lisensi Aktif | 🌐 Online | 🤖 Claude   │
└─────────────────────────────────────────────────────────┘
```

### 31.2 Header Global

**31.2.1** Logo PAUGERAN (kiri)

**31.2.2** Breadcrumb (opsional)

**31.2.3** Tombol pencarian (command palette) — Ctrl+K

**31.2.4** Tombol quick theme toggle — cycle light/dark

**31.2.5** Tombol notifikasi

**31.2.6** Tombol pengaturan — Ctrl+,

### 31.3 Sidebar

**31.3.1** Tombol "+ Sesi Baru" di atas

**31.3.2** Daftar sesi dikelompokkan berdasarkan waktu:
- Hari Ini
- Kemarin
- 7 Hari Terakhir
- 30 Hari Terakhir
- Lebih Lama

**31.3.3** Status lisensi di bagian bawah

### 31.4 Area Chat Utama

**31.4.1** Header sesi:
- Judul editable
- Indikator fase
- Dropdown model
- Quick actions

**31.4.2** Area pesan

**31.4.3** Input area

### 31.5 Status Bar

**31.5.1** Status bar di bagian bawah menampilkan:
- Status lisensi (ikon + tooltip)
- Status koneksi internet
- Provider LLM aktif
- Jumlah sesi

**31.5.2** Status bar dapat di-klik untuk detail.

---

## 32. KOMPONEN UI

### 32.1 Chat Components

**32.1.1** Pesan user: aligned right dengan warna aksen

**32.1.2** Pesan PAUGERAN: aligned left dengan warna netral, indikator fase di header, inline citation yang dapat di-hover

**32.1.3** Streaming text dengan efek typing

**32.1.4** Tombol aksi kontekstual berdasarkan fase

**32.1.5** Input area: textarea multi-line auto-resize, tombol upload dokumen, tombol pilih model, tombol kirim

### 32.2 Panel Samping

**32.2.1** Slide dari kanan

**32.2.2** Mode Peta Keterlacakan:
- Graf interaktif dengan zoom, pan, click untuk detail
- Export PNG/SVG

**32.2.3** Mode Laporan:
- 24 poin dengan collapsible sections
- Tombol export

**32.2.4** Mode Log Reasoning:
- Detail proses penalaran dengan sumber

### 32.3 Panel Pengaturan

**32.3.1** Akses dari tombol ⚙️ atau shortcut Ctrl+,

**32.3.2** Kategori:
- Tampilan
- Font
- Bahasa
- Aksesibilitas
- LLM Providers
- API Keys (Legacy)
- Legal Knowledge Base
- Penelitian Web (whitelist domain)
- Lisensi
- Data (backup, restore)
- Tim (jika AUTH_ENABLED=true)
- Tentang

### 32.4 Setup Wizard

**32.4.1** Tampil otomatis saat pertama kali dijalankan

**32.4.2** Langkah-langkah:
1. Input lisensi key
2. Konfigurasi minimal satu LLM provider
3. Pilih preferensi dasar (tema, bahasa)
4. Selesai

### 32.5 Halaman Aktivasi Lisensi

**32.5.1** Tampil saat lisensi tidak valid atau belum diaktivasi

**32.5.2** Input lisensi key dengan tombol "Aktivasi"

**32.5.3** Status validasi ditampilkan secara real-time

**32.5.4** Link ke halaman pembelian lisensi

### 32.6 Halaman Legal Knowledge Base

**32.6.1** Daftar entri dengan filter (jenis, tahun, status) dan pencarian

**32.6.2** Tombol "Tambah" untuk menambah entri baru

**32.6.3** Tombol "Import" untuk bulk import

**32.6.4** Setiap entri memiliki menu aksi: Lihat, Edit, Refresh, Hapus

### 32.7 Command Palette

**32.7.1** Modal di tengah layar dengan input pencarian

**32.7.2** Hasil pencarian ditampilkan secara real-time

**32.7.3** Keyboard navigation dengan arrow keys

**32.7.4** Preview untuk beberapa perintah

---

## 33. DESIGN SYSTEM

### 33.1 Design Tokens

**33.1.1** Colors:
- Primary: Blue (#2563eb)
- Secondary: Gray (#6b7280)
- Success: Green (#10b981)
- Warning: Yellow (#f59e0b)
- Error: Red (#ef4444)
- Background: White/Light Gray (light mode), Dark Gray/Black (dark mode)
- Text: Black/Dark Gray (light mode), White/Light Gray (dark mode)

**33.1.2** Typography:
- Font family: Inter (sans-serif), Georgia (serif), JetBrains Mono (monospace)
- Font sizes: 14px, 16px, 18px, 20px
- Line heights: 1.5 (body), 1.2 (heading)

**33.1.3** Spacing:
- Base unit: 4px
- Common spacings: 4px, 8px, 12px, 16px, 24px, 32px, 48px

**33.1.4** Shadows:
- Small: 0 1px 2px rgba(0,0,0,0.05)
- Medium: 0 4px 6px rgba(0,0,0,0.1)
- Large: 0 10px 15px rgba(0,0,0,0.1)

**33.1.5** Border radius:
- Small: 4px
- Medium: 8px
- Large: 12px
- Full: 9999px

### 33.2 Component Library

**33.2.1** Button:
- Variants: primary, secondary, ghost, danger
- Sizes: small, medium, large
- States: default, hover, active, disabled, loading

**33.2.2** Input:
- Types: text, password, email, number, textarea
- States: default, focus, error, disabled
- Features: label, placeholder, helper text, error message

**33.2.3** Select:
- Single select
- Multi-select
- Searchable
- Grouped options

**33.2.4** Modal:
- Sizes: small, medium, large, full-screen
- Features: close button, backdrop click to close, ESC to close

**33.2.5** Drawer:
- Positions: left, right, bottom
- Sizes: small, medium, large

**33.2.6** Toast:
- Types: success, error, warning, info
- Auto-dismiss: 5 seconds
- Manual close

**33.2.7** Tabs:
- Horizontal
- Vertical
- With icons

**33.2.8** Accordion:
- Single expand
- Multiple expand

**33.2.9** Tooltip:
- Positions: top, bottom, left, right
- Delay: 300ms

**33.2.10** Dropdown:
- Trigger: button, icon, text
- Menu items with icons

### 33.3 Icon Library

**33.3.1** Menggunakan Lucide Icons atau Heroicons

**33.3.2** Ukuran: 16px, 20px, 24px

**33.3.3** Warna: inherit dari parent

---

## 34. RESPONSIVITAS

### 34.1 Breakpoints

**34.1.1** Mobile: < 768px

**34.1.2** Tablet: 768px - 1024px

**34.1.3** Desktop: > 1024px

### 34.2 Layout per Breakpoint

**34.2.1** Desktop (>1024px):
- Sidebar dan chat area berdampingan
- Panel samping sebagai drawer

**34.2.2** Tablet (768-1024px):
- Sidebar collapsible
- Chat area full width saat sidebar hidden

**34.2.3** Mobile (<768px):
- Sidebar sebagai drawer overlay
- Chat area full width
- Input area fixed di bawah

---

## 35. KEYBOARD SHORTCUTS & COMMAND PALETTE

### 35.1 Keyboard Shortcuts

**35.1.1** Navigasi:
- `Ctrl+N`: Sesi baru
- `Ctrl+K`: Command palette
- `Ctrl+P`: Pilih sesi sebelumnya
- `Ctrl+/`: Toggle sidebar
- `Ctrl+1` sampai `Ctrl+9`: Pindah ke sesi ke-1 sampai ke-9
- `Ctrl+,`: Buka pengaturan
- `Ctrl+Shift+L`: Buka Legal Knowledge Base
- `Ctrl+Shift+E`: Export laporan
- `Ctrl+Shift+K`: Kelola LLM providers
- `Esc`: Tutup modal/drawer/command palette

**35.1.2** Dalam chat:
- `Enter`: Kirim pesan
- `Shift+Enter`: Baris baru
- `Ctrl+↑`: Edit pesan terakhir
- `Ctrl+↓`: Batal edit
- `Ctrl+C`: Salin pesan terpilih
- `Ctrl+Shift+C`: Salin dengan citation

**35.1.3** Dalam dokumen/laporan:
- `Ctrl+F`: Cari dalam dokumen
- `Ctrl+G`: Cari berikutnya
- `Ctrl+Shift+G`: Cari sebelumnya
- `Ctrl+P`: Print / Export
- `Ctrl+S`: Simpan perubahan

### 35.2 Command Palette

**35.2.1** Akses: `Ctrl+K`

**35.2.2** Fitur:
- Pencarian fuzzy untuk semua perintah
- Recent commands
- Keyboard navigation
- Preview untuk beberapa perintah

**35.2.3** Perintah yang tersedia:
- Buat sesi baru
- Pindah sesi
- Buka pengaturan
- Kelola API key
- Kelola LLM providers
- Kelola Knowledge Base
- Export laporan
- Backup data
- Restore data
- Toggle tema
- Ubah bahasa
- Dan semua aksi lainnya

---

# BAGIAN VI — SPESIFIKASI DEPLOYMENT & OPERASIONAL

---

## 36. MODEL DISTRIBUSI

### 36.1 Binary Standalone

**36.1.1** Linux: `paugeran-linux` untuk x86_64

**36.1.2** macOS: `paugeran-macos` universal untuk Intel dan Apple Silicon

**36.1.3** Windows: `paugeran-windows.exe` untuk x86_64

### 36.2 Docker

**36.2.1** Image: `ghcr.io/paugeran/paugeran:latest`

**36.2.2** Perintah: `docker run -d -p 3000:3000 -v paugeran_data:/data ghcr.io/paugeran/paugeran`

### 36.3 Railway

**36.3.1** One-click deploy melalui tombol "Deploy to Railway" di repository

**36.3.2** Volume `/data` wajib ditambahkan untuk persistent storage

### 36.4 VPS

**36.4.1** Quick install script: `curl -fsSL https://get.paugeran.com/install.sh | bash`

**36.4.2** Docker Compose dengan Caddy sebagai reverse proxy untuk auto-SSL

### 36.5 Homelab

**36.5.1** Docker run dengan akses via Tailscale atau Cloudflare Tunnel

### 36.6 Tauri Desktop (Opsional)

**36.6.1** Installer: `.msi` untuk Windows, `.dmg` untuk macOS, `.appimage` untuk Linux

**36.6.2** Tauri menjalankan binary PAUGERAN sebagai sidecar dan membuka WebView ke localhost

---

## 37. DEPLOYMENT SCENARIOS

### 37.1 Laptop Pribadi

**37.1.1** Download binary dari GitHub Releases

**37.1.2** Jalankan binary

**37.1.3** Buka browser ke `http://localhost:3000`

**37.1.4** Setup wizard muncul

**37.1.5** Input lisensi key dan API key

**37.1.6** Mulai gunakan

### 37.2 Railway

**37.2.1** Fork repository

**37.2.2** Connect ke Railway

**37.2.3** Railway auto-detect Dockerfile

**37.2.4** Add environment variables:
```
AUTH_ENABLED=true
JWT_SECRET=<random-string>
LICENSE_KEY=<license-key>
ANTHROPIC_API_KEY=sk-ant-...
```

**37.2.5** Add Volume `/data` untuk persistent storage

**37.2.6** Deploy

**37.2.7** Akses via URL yang diberikan Railway

### 37.3 VPS

**37.3.1** SSH ke VPS

**37.3.2** Jalankan install script:
```bash
curl -fsSL https://get.paugeran.com/install.sh | bash
```

**37.3.3** Script otomatis:
- Install Docker
- Pull image PAUGERAN
- Setup Caddy untuk auto-SSL
- Tanya domain Anda
- Start service

**37.3.4** Akses via `https://paugeran.domainanda.com`

### 37.4 Homelab

**37.4.1** Jalankan Docker container di server rumah

**37.4.2** Akses via Tailscale:
```bash
docker run -d -p 3000:3000 -v paugeran_data:/data \
  -e AUTH_ENABLED=true \
  ghcr.io/paugeran/paugeran
```

**37.4.3** Akses dari perangkat lain di Tailscale network via `http://paugeran:3000`

**37.4.4** Atau gunakan Cloudflare Tunnel untuk akses via domain publik

### 37.5 Air-Gapped Deployment

**37.5.1** Untuk deployment di environment yang tidak dapat terhubung ke internet

**37.5.2** Gunakan lisensi offline

**37.5.3** Import lisensi offline melalui Settings → Lisensi → Import Offline

---

## 38. ENVIRONMENT VARIABLES

### 38.1 Variabel Wajib

**38.1.1** `AUTH_ENABLED`: Default false. Set true untuk mengaktifkan autentikasi multi-user.

**38.1.2** `JWT_SECRET`: Wajib jika AUTH_ENABLED=true. Random string 256-bit.

### 38.2 Variabel Opsional

**38.2.1** `DATABASE_URL`: Default `sqlite:///data/paugeran.db`. Bisa diganti ke PostgreSQL.

**38.2.2** `PORT`: Default 3000.

**38.2.3** `DATA_DIR`: Default `/data`.

**38.2.4** `CORS_ORIGINS`: Default `http://localhost:3000`.

**38.2.5** `LOG_LEVEL`: Default `info`.

**38.2.6** `MAX_UPLOAD_SIZE`: Default 10485760 (10MB).

### 38.3 Variabel Lisensi

**38.3.1** `LICENSE_KEY`: Lisensi key untuk aktivasi otomatis.

**38.3.2** `LICENSE_SERVER_URL`: URL server lisensi (default: `https://license.paugeran.com`).

**38.3.3** `LICENSE_CHECK_INTERVAL`: Interval validasi dalam jam (default: 24).

**38.3.4** `LICENSE_GRACE_PERIOD`: Grace period dalam hari (default: 7).

---

## 39. BACKUP & RECOVERY

### 39.1 Backup Strategy

**39.1.1** Backup mencakup:
- Database
- Encryption key
- Dokumen pengguna
- Laporan yang diekspor
- Legal Knowledge Base
- Log files

**39.1.2** Lisensi key ikut ter-backup sebagai bagian dari database.

### 39.2 Binary Mode Backup

**39.2.1** Backup dilakukan dengan mengarsipkan data directory:
```bash
tar czf paugeran-backup-$(date +%Y%m%d).tar.gz -C ~/.local/share/paugeran .
```

### 39.3 Docker Mode Backup

**39.3.1** Backup dilakukan dengan menjalankan container alpine yang mengarsipkan volume:
```bash
docker run --rm -v paugeran_data:/data -v $(pwd):/backup alpine \
  tar czf /backup/paugeran-backup-$(date +%Y%m%d).tar.gz /data
```

### 39.4 Recovery

**39.4.1** Recovery dilakukan dengan mengekstrak arsip ke data directory dan restart aplikasi.

### 39.5 Migration

**39.5.1** SQLite schema backward compatible melalui migrations.

**39.5.2** Legal Knowledge Base dapat di-export terpisah sebagai file JSON untuk migrasi.

---

## 40. MONITORING & OBSERVABILITAS

### 40.1 Logging

**40.1.1** Logging menggunakan tracing crate dengan format JSON structured.

**40.1.2** Log levels: DEBUG (development only), INFO, WARNING, ERROR, CRITICAL.

**40.1.3** API key tidak boleh di-log dalam bentuk apapun.

**40.1.4** PII tidak boleh di-log tanpa redaksi.

**40.1.5** Lisensi key tidak boleh di-log dalam bentuk apapun.

### 40.2 Health Check

**40.2.1** Endpoint `GET /health` mengembalikan:
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "database": "connected",
  "auth_enabled": false,
  "license_valid": true,
  "timestamp": "2026-08-28T10:00:00Z"
}
```

### 40.3 LangSmith Integration

**40.3.1** Jika LangSmith API key dikonfigurasi, semua LLM calls di-trace untuk observabilitas.

### 40.4 Metrik yang Dimonitor

**40.4.1** Request rate per endpoint

**40.4.2** Response time per endpoint

**40.4.3** LLM API call count dan latency

**40.4.4** Knowledge Base hit rate

**40.4.5** Web research success rate

**40.4.6** License validation status

**40.4.7** Error rate per kategori

---

# BAGIAN VII — SPESIFIKASI KEAMANAN & COMPLIANCE

---

## 41. KEAMANAN DATA

### 41.1 Enkripsi API Key

**41.1.1** API key dienkripsi dengan AES-256-GCM.

**41.1.2** Kunci enkripsi disimpan dengan permission 600.

### 41.2 Password Hashing

**41.2.1** Password di-hash dengan Argon2.

### 41.3 JWT Token

**41.3.1** JWT token menggunakan algoritma HS256.

**41.3.2** Access token expiry 15 menit.

**41.3.3** Refresh token expiry 24 jam.

### 41.4 Lisensi Key

**41.4.1** Lisensi key dienkripsi dengan AES-256-GCM.

### 41.5 PII Protection

**41.5.1** PII di-redact sebelum dikirim ke LLM API.

**41.5.2** Mapping untuk de-anonymize setelah respons diterima.

**41.5.3** Plain text PII tidak di-log.

### 41.6 Session Isolation

**41.6.1** Setiap query database WAJIB filter by session_id untuk isolasi sesi.

### 41.7 Admin Protection

**41.7.1** Admin tidak bisa melihat data pribadi atau API key user lain.

### 41.8 Audit Trail

**41.8.1** Semua aksi kritis dicatat di audit_logs.

### 41.9 Rust Security

**41.9.1** Tidak ada unsafe code Rust tanpa justifikasi yang jelas.

**41.9.2** Tidak ada blocking I/O di async context.

**41.9.3** Semua dependencies diaudit dengan `cargo audit`.

**41.9.4** Tidak ada `unwrap()` di production code.

### 41.10 SQL Injection Prevention

**41.10.1** SQL injection dicegah melalui parameterized queries via SQLx.

### 41.11 XSS Prevention

**41.11.1** XSS dicegah melalui input sanitization.

### 41.12 Web Research Security

**41.12.1** Penelitian web hanya ke domain whitelist.

### 41.13 License Server Security

**41.13.1** Request ke server lisensi menggunakan HTTPS dengan payload yang di-sign.

**41.13.2** Server lisensi tidak menerima data pengguna, API key LLM, atau konten sesi.

---

## 42. ENKRIPSI & PRIVACY

### 42.1 Enkripsi at Rest

**42.1.1** API keys: AES-256-GCM encryption

**42.1.2** Lisensi keys: AES-256-GCM encryption

**42.1.3** Database: SQLite (bisa ditambahkan SQLCipher jika perlu)

**42.1.4** Dokumen: file system permission 600

### 42.2 Enkripsi in Transit

**42.2.1** HTTP untuk local access (localhost)

**42.2.2** HTTPS via reverse proxy untuk remote access

### 42.3 Privacy by Design

**42.3.1** Data pengguna tidak dikirim ke server PAUGERAN.

**42.3.2** API key hanya dikirim ke provider LLM yang dipilih pengguna.

**42.3.3** Lisensi key hanya dikirim ke server lisensi untuk validasi.

**42.3.4** Konten sesi tidak dikirim ke pihak ketiga.

---

## 43. COMPLIANCE HUKUM

### 43.1 UU PDP (Perlindungan Data Pribadi)

**43.1.1** PAUGERAN mematuhi UU PDP Indonesia.

**43.1.2** Data pengguna disimpan lokal atau di server yang dikendalikan pengguna.

**43.1.3** Pengguna memiliki hak akses, koreksi, dan penghapusan data.

### 43.2 Kerahasiaan Advokat-Klien

**43.2.1** PAUGERAN dirancang untuk menjaga kerahasiaan advokat-klien.

**43.2.2** Data sesi tidak bocor ke pihak ketiga.

**43.2.3** Admin tidak bisa melihat data sesi user lain.

---

## 44. AUDIT TRAIL

### 44.1 Audit Logs

**44.1.1** Semua aksi kritis dicatat di audit_logs.

**44.1.2** Format: JSON structured.

**44.1.3** Retention: 2 tahun.

### 44.2 Events yang Di-log

**44.2.1** User registration/login

**44.2.2** Session created/updated/deleted

**44.2.3** Message sent

**44.2.4** Document uploaded/deleted

**44.2.5** Analysis started/completed

**44.2.6** Report exported

**44.2.7** API key changed/deleted

**44.2.8** Admin actions (user management, config changes)

**44.2.9** License validation events

---

# BAGIAN VIII — KRITERIA PENERIMAAN & TESTING

---

## 45. KRITERIA KEBERHASILAN

### 45.1 Definisi "Jawaban Berhasil"

**45.1.1** Sebuah analisis dianggap berhasil apabila pengguna dapat menjawab lima pertanyaan berikut hanya dengan membaca output PAUGERAN:

**45.1.2** Apa sebenarnya masalah hukum saya?
→ Dijawab oleh: Poin 3 (Rekonstruksi Masalah) dan Poin 7 (Masalah Hukum)

**45.1.3** Hukum apa yang mengatur masalah tersebut?
→ Dijawab oleh: Poin 8 (Dasar Hukum) dan Poin 10 (Kaidah Hukum)

**45.1.4** Bagaimana hukum tersebut diterapkan terhadap fakta saya?
→ Dijawab oleh: Poin 12 (Penerapan terhadap Fakta)

**45.1.5** Apa alasan yang mendukung dan menentang kesimpulan tersebut?
→ Dijawab oleh: Poin 13 (Argumen Pendukung) dan Poin 14 (Argumen Berlawanan)

**45.1.6** Mengapa PAUGERAN sampai pada kesimpulan akhirnya dan apa yang dapat membuat kesimpulan itu berubah?
→ Dijawab oleh: Poin 22 (Kesimpulan Hukum), Poin 24 (Peta Keterlacakan), dan Poin 21 (Informasi yang Masih Diperlukan)

**45.1.7** Jika kelima pertanyaan tersebut tidak dapat dijawab dari output, maka produk belum memenuhi standar PAUGERAN.

### 45.2 Metrik Kualitas

**45.2.1** Akurasi citation: lebih dari 95%

**45.2.2** Kelengkapan keterlacakan: 100%

**45.2.3** Keberimbangan: 100% selalu ada kontraargumentasi

**45.2.4** Waktu respons chat: kurang dari 2 detik streaming

**45.2.5** Waktu generasi laporan: kurang dari 5 menit

**45.2.6** Startup time: kurang dari 5 detik

**45.2.7** Memory usage: kurang dari 500 MB typical

**45.2.8** Binary size: kurang dari 30 MB

**45.2.9** Inline citation coverage: 100% klaim hukum memiliki citation

**45.2.10** Knowledge Base hit rate: minimal 30% untuk pertanyaan umum setelah penggunaan 1 bulan

**45.2.11** Web research success rate: lebih dari 90% untuk domain whitelist

**45.2.12** License validation success rate: lebih dari 99% saat online

**45.2.13** Export PDF/DOCX quality: siap pakai tanpa formatting ulang

---

## 46. KRITERIA PENERIMAAN

Produk dinyatakan ready dan siap digunakan apabila memenuhi SEMUA kriteria berikut:

### 46.1 Kriteria Fungsional

**46.1.1** Pengguna dapat menjalankan dengan satu perintah

**46.1.2** Setup wizard muncul saat pertama kali dijalankan

**46.1.3** Pengguna dapat mengaktivasi lisensi melalui antarmuka web

**46.1.4** Lisensi divalidasi secara berkala tanpa mengganggu pengguna

**46.1.5** Grace period berfungsi saat offline

**46.1.6** Pengguna dapat mengonfigurasi berbagai LLM providers (tidak hanya model ternama)

**46.1.7** Pengguna dapat memasukkan API key melalui antarmuka web

**46.1.8** Pengguna dapat mengubah API key dan provider kapan saja

**46.1.9** Auth opsional berdasarkan AUTH_ENABLED

**46.1.10** User pertama otomatis jadi admin jika AUTH_ENABLED=true

**46.1.11** Admin dapat invite, kelola, dan monitor user

**46.1.12** Admin tidak bisa lihat data atau API key user lain

**46.1.13** Pengguna dapat membuat sesi obrolan baru kapan saja

**46.1.14** Sesi lama dapat dibuka kembali kapan saja tanpa batas waktu

**46.1.15** Sesi dapat dihapus permanen dengan konfirmasi

**46.1.16** Setiap sesi terisolasi dari sesi lain

**46.1.17** PAUGERAN melakukan wawancara adaptif

**46.1.18** Pengguna dapat mengunggah dokumen PDF, DOCX, TXT

**46.1.19** PAUGERAN melakukan penelitian web ke situs whitelist

**46.1.20** PAUGERAN menyimpan peraturan yang diteliti ke Legal Knowledge Base

**46.1.21** PAUGERAN menggunakan Legal Knowledge Base untuk analisis berikutnya

**46.1.22** Setiap klaim hukum memiliki inline citation yang detail

**46.1.23** Log reasoning dapat diakses dan menampilkan sumber detail

**46.1.24** PAUGERAN menghasilkan laporan 24 poin

**46.1.25** PAUGERAN menampilkan peta keterlacakan interaktif

**46.1.26** Pengguna dapat mengekspor laporan ke PDF profesional dan DOCX profesional

**46.1.27** Template export dapat dikustomisasi

**46.1.28** Pengguna dapat mengustomisasi antarmuka

**46.1.29** Preferensi UI tersimpan persisten

**46.1.30** Command palette berfungsi dengan semua perintah

**46.1.31** Keyboard shortcuts lengkap berfungsi

**46.1.32** Screen reader support berfungsi

**46.1.33** Mode aksesibilitas (High Contrast, Reduced Motion) berfungsi

**46.1.34** Custom Graph Engine berjalan dengan benar untuk semua 11 fase

**46.1.35** Streaming SSE berfungsi dengan baik

**46.1.36** Cancellation support berfungsi

**46.1.37** Multi-provider LLM berfungsi dengan fallback

### 46.2 Kriteria Non-Fungsional

**46.2.1** Respons chat kurang dari 2 detik

**46.2.2** Laporan lengkap kurang dari 5 menit

**46.2.3** Binary size kurang dari 30 MB

**46.2.4** Startup time kurang dari 5 detik

**46.2.5** Memory usage kurang dari 500 MB

**46.2.6** Data terenkripsi at rest

**46.2.7** PII diredaksi sebelum dikirim ke API model

**46.2.8** Semua klaim hukum memiliki citation yang valid

**46.2.9** API key dan lisensi key tidak pernah di-log dalam plain text

**46.2.10** UI responsif di desktop, tablet, dan mobile

**46.2.11** Tidak ada memory leaks

**46.2.12** Tidak ada data races

**46.2.13** WCAG 2.1 AA compliance untuk aksesibilitas

### 46.3 Kriteria Kepatuhan PRD

**46.3.1** Tidak ada halusinasi pasal atau putusan dalam 100 uji kasus

**46.3.2** Semua kesimpulan memiliki peta keterlacakan lengkap

**46.3.3** Kontraargumentasi selalu ditampilkan

**46.3.4** Ketidakpastian dinyatakan secara eksplisit

**46.3.5** Bahasa analisis sesuai standar profesional

**46.3.6** Isolasi sesi terjamin melalui uji penetrasi

**46.3.7** Audit log mencatat semua aksi kritis

**46.3.8** Penelitian web hanya ke domain whitelist

**46.3.9** Inline citation detail di semua output

**46.3.10** Legal Knowledge Base dapat dibangun dari waktu ke waktu

**46.3.11** Lisensi validasi bekerja dengan grace period

### 46.4 Kriteria Rust

**46.4.1** Semua kode compile tanpa warnings

**46.4.2** cargo clippy clean

**46.4.3** cargo fmt applied

**46.4.4** cargo audit clean

**46.4.5** Test coverage lebih dari 80%

**46.4.6** Documentation coverage lebih dari 70%

**46.4.7** No unwrap() in production code

### 46.5 Kriteria Deployment

**46.5.1** Binary dapat dijalankan di Windows, macOS, Linux

**46.5.2** Docker image build berhasil

**46.5.3** Dapat di-deploy ke Railway

**46.5.4** Dapat di-deploy ke VPS dengan Caddy

**46.5.5** Dapat di-deploy ke homelab

**46.5.6** Volume persistence berfungsi

**46.5.7** Health check endpoint berfungsi

**46.5.8** Backup dan restore teruji

**46.5.9** Deployment air-gapped dengan lisensi offline berfungsi

### 46.6 Kriteria Keamanan

**46.6.1** API key terenkripsi dengan AES-256-GCM

**46.6.2** Lisensi key terenkripsi dengan AES-256-GCM

**46.6.3** Encryption key disimpan dengan permission 600

**46.6.4** Session isolation verified

**46.6.5** OWASP Top 10 vulnerabilities tested dan fixed

**46.6.6** SQL injection prevented

**46.6.7** XSS prevented

**46.6.8** Admin tidak bisa akses data user lain

**46.6.9** Penelitian web hanya ke whitelist

**46.6.10** Server lisensi tidak menerima data sensitif

### 46.7 Kriteria Kustomisasi & Aksesibilitas

**46.7.1** Minimal 5 tema warna tersedia

**46.7.2** Minimal 4 ukuran font tersedia

**46.7.3** Minimal 2 bahasa UI tersedia

**46.7.4** Preferensi tersimpan di database

**46.7.5** Perubahan preferensi berlaku instan

**46.7.6** Reset ke default berfungsi

**46.7.7** Command palette mencakup semua perintah

**46.7.8** Keyboard shortcuts lengkap dan berfungsi

**46.7.9** Screen reader dapat mengakses semua fitur

**46.7.10** Mode aksesibilitas berfungsi dengan baik

### 46.8 Kriteria Legal Knowledge Base

**46.8.1** Peraturan dari web dapat disimpan ke Knowledge Base

**46.8.2** Pencarian semantik di Knowledge Base berfungsi

**46.8.3** Knowledge Base digunakan otomatis dalam analisis

**46.8.4** Admin dapat mengelola Knowledge Base global

**46.8.5** User dapat mengelola Knowledge Base pribadi

**46.8.6** Bulk import berfungsi

**46.8.7** Status keberlakuan peraturan dapat di-update

### 46.9 Kriteria Lisensi

**46.9.1** Aktivasi lisensi berfungsi

**46.9.2** Validasi berkala berfungsi

**46.9.3** Grace period berfungsi

**46.9.4** Agen dikunci saat lisensi tidak valid

**46.9.5** Data tetap dapat diakses saat lisensi tidak valid

**46.9.6** Lisensi offline untuk air-gapped deployment berfungsi

**46.9.7** Lisensi key terenkripsi di database

### 46.10 Kriteria Export

**46.10.1** PDF export menghasilkan dokumen profesional

**46.10.2** DOCX export menghasilkan dokumen profesional

**46.10.3** Template dapat dikustomisasi

**46.10.4** Inline citation ter-embed di dokumen

**46.10.5** Daftar Isi otomatis berfungsi

**46.10.6** Bookmark PDF berfungsi

**46.10.7** Metadata PDF terisi dengan benar

---

## 47. TESTING STRATEGY

### 47.1 Testing Pyramid

**47.1.1** Unit Tests (70%):
- Setiap fungsi Rust
- Setiap komponen SolidJS
- Coverage target: 80%

**47.1.2** Integration Tests (20%):
- API endpoints
- Database operations
- LLM provider integration
- Knowledge Base operations
- License validation

**47.1.3** E2E Tests (10%):
- Full user flows
- Multi-session scenarios
- Admin workflows
- Export workflows

### 47.2 Test Categories

**47.2.1** Functional Tests:
- Setiap fitur di PRD
- Setiap endpoint API
- Setiap fase agen

**47.2.2** Security Tests:
- OWASP Top 10
- Session isolation
- API key isolation
- License validation bypass attempts
- SQL injection
- XSS

**47.2.3** Performance Tests:
- Load testing (100 concurrent users)
- Stress testing
- Endurance testing (24+ jam)
- Memory leak detection

**47.2.4** Accessibility Tests:
- WCAG 2.1 AA compliance
- Keyboard navigation
- Screen reader compatibility
- Color contrast

**47.2.5** Hallucination Tests:
- 100+ kasus hukum
- Verifikasi tidak ada pasal/putusan karangan
- Verifikasi semua citation valid

### 47.3 Test Data

**47.3.1** Dataset kasus hukum untuk pengujian

**47.3.2** Mock LLM responses

**47.3.3** Sample documents (PDF, DOCX, TXT)

**47.3.4** Sample regulations untuk Knowledge Base

---

## 48. QUALITY GATES

### 48.1 Pre-Development Gate

**48.1.1** PRD final dan disetujui

**48.1.2** Arsitektur teknis disetujui

**48.1.3** Database schema disetujui

**48.1.4** API specification disetujui

### 48.2 Pre-Alpha Gate

**48.2.1** Core features implemented (minimal 60% dari kriteria fungsional)

**48.2.2** Backend Rust berjalan tanpa crash

**48.2.3** Frontend SolidJS render dengan benar

**48.2.4** Database migrations berfungsi

**48.2.5** Setup wizard dapat dilalui

**48.2.6** Lisensi aktivasi dasar berfungsi

**48.2.7** Minimal satu LLM provider dapat dipanggil

### 48.3 Alpha Gate

**48.3.1** Semua 11 fase agen berjalan dengan benar

**48.3.2** Streaming SSE berfungsi untuk semua fase

**48.3.3** Multi-provider LLM berfungsi dengan fallback

**48.3.4** Legal Knowledge Base dapat menyimpan dan mencari entri

**48.3.5** Penelitian web berfungsi untuk minimal 5 domain whitelist

**48.3.6** Inline citation muncul di semua output

**48.3.7** Peta keterlacakan dapat dirender

**48.3.8** Export PDF dan DOCX berfungsi

**48.3.9** Kustomisasi UI berfungsi untuk semua opsi

**48.3.10** Test coverage >60%

**48.3.11** Tidak ada crash dalam 24 jam pengujian

### 48.4 Beta Gate

**48.4.1** Semua kriteria fungsional terpenuhi

**48.4.2** Semua kriteria non-fungsional terpenuhi

**48.4.3** Test coverage >80%

**48.4.4** Tidak ada halusinasi dalam 50 uji kasus

**48.4.5** Security audit awal selesai (OWASP Top 10)

**48.4.6** Accessibility audit awal selesai (WCAG 2.1 AA)

**48.4.7** Performance benchmarks memenuhi target

**48.4.8** Deployment ke semua skenario teruji

**48.4.9** Backup dan restore teruji

**48.4.10** Dokumentasi pengguna draft selesai

### 48.5 Release Candidate (RC) Gate

**48.5.1** Semua kriteria penerimaan terpenuhi 100%

**48.5.2** Tidak ada halusinasi dalam 100 uji kasus

**48.5.3** Security audit final selesai dan semua temuan critical/high fixed

**48.5.4** Accessibility audit final selesai

**48.5.5** Performance audit final selesai

**48.5.6** Legal review (ToS, Privacy Policy) selesai

**48.5.7** Dokumentasi final selesai

**48.5.8** Tidak ada bug critical atau high

**48.5.9** Bug medium dan low terdokumentasi dan memiliki rencana mitigasi

**48.5.10** Load test 100 concurrent users lulus

**48.5.11** Endurance test 72 jam lulus tanpa memory leak

### 48.6 General Availability (GA) Gate

**48.6.1** Semua RC gate terpenuhi

**48.6.2** Beta testing dengan minimal 10 pengguna eksternal selesai

**48.6.3** Feedback beta di-review dan ditindaklanjuti

**48.6.4** Code signing untuk semua platform selesai

**48.6.5** Docker image di-scan dan tidak ada critical vulnerabilities

**48.6.6** Server lisensi production siap

**48.6.7** Monitoring dan alerting production siap

**48.6.8** Runbook operasional selesai

**48.6.9** Support channels siap

**48.6.10** Go/No-Go decision dibuat dan disetujui

---

# BAGIAN IX — TATA KELOLA & DOKUMEN TURUNAN

---

## 49. LARANGAN PRODUK

PAUGERAN tidak boleh melakukan hal-hal berikut. Setiap pelanggaran terhadap larangan ini merupakan pelanggaran kontrak yang harus segera diperbaiki.

### 49.1 Larangan Terkait Halusinasi & Akurasi

**49.1.1** Mengarang pasal, nomor peraturan, atau tahun peraturan.

**49.1.2** Mengarang putusan, nomor perkara, atau tanggal putusan.

**49.1.3** Mengarang sumber hukum yang tidak ada.

**49.1.4** Menyatakan sumber masih berlaku tanpa pemeriksaan yang memadai.

**49.1.5** Menyatakan fakta pengguna sebagai fakta terbukti tanpa dasar verifikasi.

**49.1.6** Memberikan kesimpulan hanya berdasarkan kemiripan kata kunci.

**49.1.7** Mengklaim telah melakukan penelitian yang sebenarnya tidak dilakukan.

**49.1.8** Mengklaim telah memeriksa dokumen yang tidak tersedia.

**49.1.9** Mengubah fakta agar cocok dengan norma.

### 49.2 Larangan Terkait Penalaran

**49.2.1** Menyembunyikan ketidakpastian atau ambiguitas hukum.

**49.2.2** Hanya mencari sumber yang mendukung kesimpulan (confirmation bias).

**49.2.3** Menghapus atau mengabaikan argumen pihak lawan.

**49.2.4** Memberikan kepastian yang tidak didukung data.

**49.2.5** Memberikan jawaban tanpa melalui siklus pemahaman.

**49.2.6** Melewatkan fase kontraargumentasi.

**49.2.7** Memalsukan kepastian ketika kesimpulan tidak dapat ditentukan.

### 49.3 Larangan Terkait Data & Privasi

**49.3.1** Mengirim API key pengguna ke server PAUGERAN atau pihak ketiga.

**49.3.2** Menyimpan API key dalam plain text di database atau log.

**49.3.3** Mengakses data dari sesi obrolan lain.

**49.3.4** Membocorkan data sesi ke sesi lain.

**49.3.5** Menampilkan error message yang mengekspos API key, lisensi key, atau data sensitif.

**49.3.6** Memproses data setelah sesi dihapus.

**49.3.7** Menggunakan data pengguna untuk melatih model AI.

**49.3.8** Mengirim data preferensi ke pihak ketiga.

**49.3.9** Mengirim lisensi key ke server lisensi dalam plain text.

**49.3.10** Mengirim data pengguna, API key LLM, atau konten sesi ke server lisensi.

### 49.4 Larangan Terkait Penelitian Web

**49.4.1** Mengakses situs di luar daftar putih untuk penelitian.

**49.4.2** Tidak menghormati robots.txt dari situs yang diakses.

**49.4.3** Menyembunyikan kegagalan akses situs dari pengguna.

### 49.5 Larangan Terkait Autentikasi & Admin

**49.5.1** Mewajibkan login atau autentikasi untuk penggunaan pribadi (saat AUTH_ENABLED=false).

**49.5.2** Menghapus data sesi tanpa konfirmasi eksplisit dari pengguna.

**49.5.3** Mengubah preferensi UI tanpa persetujuan pengguna.

**49.5.4** Admin melihat data pribadi atau API key user lain.

**49.5.5** Admin menurunkan diri sendiri dari role admin.

**49.5.6** Menghapus admin terakhir di sistem.

**49.5.7** Mengunci pengguna dari data mereka sendiri saat lisensi tidak valid (data harus tetap dapat diakses untuk backup/migrasi).

**49.5.8** Mengunci pengguna tanpa grace period saat validasi lisensi gagal karena masalah jaringan.

### 49.6 Larangan Terkait Lisensi

**49.6.1** Menyembunyikan status lisensi dari pengguna.

**49.6.2** Mematikan akses data saat lisensi tidak valid (hanya agen yang dikunci).

**49.6.3** Mengirim informasi sensitif ke server lisensi.

### 49.7 Larangan Terkait Kode & Teknis

**49.7.1** Menggunakan unsafe code Rust tanpa justifikasi yang jelas.

**49.7.2** Menyimpan plain text API key di memori lebih dari yang diperlukan.

**49.7.3** Melakukan blocking I/O di async context.

**49.7.4** Menggunakan `unwrap()` di production code tanpa penanganan error yang tepat.

**49.7.5** Mengklaim klaim hukum tanpa inline citation yang detail.

**49.7.6** Menyimpan dokumen pengguna (non-hukum) ke Legal Knowledge Base.

**49.7.7** Mengunci pengguna pada satu penyedia atau model LLM tertentu.

**49.7.8** Mengabaikan error LLM provider tanpa fallback.

---

## 50. DOKUMEN TURUNAN

### 50.1 Kedudukan Dokumen Turunan

**50.1.1** Dokumen ini (PAUGERAN Contract Baseline) adalah sumber kebenaran mutlak bagi seluruh proyek PAUGERAN.

**50.1.2** Dokumen turunan adalah dokumen-dokumen yang diturunkan dari Contract Baseline ini untuk memberikan detail teknis, operasional, atau prosedural yang lebih spesifik.

**50.1.3** Dokumen turunan tidak boleh bertentangan dengan Contract Baseline. Jika terjadi pertentangan, Contract Baseline yang berlaku dan dokumen turunan harus direvisi.

### 50.2 Format Referensi Silang

**50.2.1** Setiap dokumen turunan wajib mencantumkan referensi ke pasal dan ayat Contract Baseline menggunakan format: `[CB §X.Y]` di mana X adalah nomor bab dan Y adalah nomor sub-bab.

**50.2.2** Contoh referensi:
- `[CB §6.2]` merujuk ke Bab 6 (Arsitektur Sistem), sub-bab 6.2
- `[CB §23.1.12]` merujuk ke Bab 23 (Database Schema), sub-bab 23.1, poin 23.1.12

### 50.3 Daftar Dokumen Turunan Wajib

**50.3.1 Kategori Arsitektur & Teknis Inti:**

**a) SPEC-ARCH — Spesifikasi Arsitektur Teknis**
- Referensi: [CB §6], [CB §7]
- Format: Markdown + diagram (Mermaid/PlantUML)
- Isi: Diagram arsitektur C4 Model, alur data, sequence diagrams, Architecture Decision Records (ADRs)
- Target: Developer backend & frontend

**b) SPEC-API — OpenAPI Specification**
- Referensi: [CB §24]
- Format: YAML (OpenAPI 3.1)
- Isi: Semua endpoint dengan schema, authentication schemes, error codes, contoh request/response
- Target: Developer, QA, integrator

**c) SPEC-DB — Spesifikasi Database & Migrasi**
- Referensi: [CB §23]
- Format: SQL migration files + Markdown
- Isi: Migration files, ERD, index strategy, query performance notes
- Target: Developer backend, DBA

**d) SPEC-REPO — Spesifikasi Struktur Repositori**
- Referensi: [CB §37]
- Format: Markdown + tree structure
- Isi: Struktur direktori, konvensi penamaan, workspace configuration
- Target: Semua developer

**50.3.2 Kategori Spesifikasi Fitur:**

**e) SPEC-GRAPH — Spesifikasi Custom Graph Engine**
- Referensi: [CB §22], [CB §26], [CB §27]
- Format: Markdown + state diagram
- Isi: State diagram, spesifikasi setiap node, conditional edges, error handling
- Target: Developer backend

**f) SPEC-LLM — Spesifikasi Multi-Provider LLM**
- Referensi: [CB §14]
- Format: Markdown + interface definitions
- Isi: Trait LlmProvider, adapter untuk setiap provider, model routing, fallback mechanism
- Target: Developer backend

**g) SPEC-KB — Spesifikasi Legal Knowledge Base**
- Referensi: [CB §12]
- Format: Markdown + algoritma
- Isi: Skema penyimpanan, algoritma embedding, pencarian semantik, chunking strategy
- Target: Developer backend

**h) SPEC-LICENSE — Spesifikasi Sistem Lisensi**
- Referensi: [CB §15]
- Format: Markdown + protocol definitions
- Isi: Protokol validasi, grace period logic, lisensi offline, cryptographic signing
- Target: Developer backend, security team

**i) SPEC-WEB — Spesifikasi Penelitian Web**
- Referensi: [CB §13]
- Format: Markdown + whitelist config
- Isi: Daftar whitelist, HTTP client config, HTML parsing, etika scraping
- Target: Developer backend, legal advisor

**j) SPEC-CITATION — Spesifikasi Inline Citation**
- Referensi: [CB §29.5]
- Format: Markdown + format definitions
- Isi: Format citation, visualisasi, konsistensi, export format
- Target: Developer frontend & backend

**k) SPEC-EXPORT — Spesifikasi Export Dokumen Profesional**
- Referensi: [CB §19]
- Format: Markdown + template files
- Isi: Template PDF/DOCX, tipografi, metadata, bookmark, custom template
- Target: Developer backend, designer

**l) SPEC-A11Y — Spesifikasi Aksesibilitas**
- Referensi: [CB §18]
- Format: Markdown + WCAG checklist
- Isi: WCAG 2.1 AA compliance, keyboard navigation, ARIA labels, screen reader testing
- Target: Developer frontend, QA, accessibility specialist

**m) SPEC-AUTH — Spesifikasi Autentikasi & Otorisasi**
- Referensi: [CB §16]
- Format: Markdown + flow diagrams
- Isi: JWT structure, password hashing, RBAC matrix, invitation system
- Target: Developer backend, security team

**n) SPEC-UI — Spesifikasi Antarmuka Pengguna**
- Referensi: [CB §31], [CB §32], [CB §33]
- Format: Markdown + wireframes + design tokens
- Isi: Design tokens, component library, layout rules, responsive breakpoints
- Target: Designer, developer frontend

**50.3.3 Kategori Pengujian & Kualitas:**

**o) TEST-PLAN — Rencana Pengujian Keseluruhan**
- Referensi: [CB §45], [CB §46], [CB §47]
- Format: Markdown + test matrix
- Isi: Testing pyramid, coverage targets, test environment, acceptance criteria
- Target: QA team, developer

**p) TEST-CASES — Kumpulan Test Cases**
- Referensi: [CB §49], [CB §46]
- Format: Markdown atau test management tool
- Isi: Test cases untuk setiap endpoint, fase agen, isolasi sesi, lisensi, multi-provider, Knowledge Base, web research, export, aksesibilitas
- Target: QA team

**q) TEST-HALLUCINATION — Protokol Uji Anti-Halusinasi**
- Referensi: [CB §49.1], [CB §46.3.1]
- Format: Markdown + dataset
- Isi: Dataset 100+ kasus hukum, kriteria penilaian, prosedur pengujian
- Target: QA team, legal advisor

**r) TEST-PERFORMANCE — Protokol Uji Performa**
- Referensi: [CB §45.2], [CB §46.2]
- Format: Markdown + benchmark scripts
- Isi: Load testing, stress testing, endurance testing, performance benchmarks
- Target: QA team, DevOps

**s) TEST-SECURITY — Protokol Uji Keamanan**
- Referensi: [CB §41], [CB §46.6]
- Format: Markdown + security checklist
- Isi: OWASP Top 10, penetration testing, API key isolation, license validation bypass
- Target: Security team, QA

**50.3.4 Kategori Deployment & Operasional:**

**t) DEPLOY-GUIDE — Panduan Deployment**
- Referensi: [CB §36], [CB §37], [CB §38]
- Format: Markdown + scripts
- Isi: Deployment untuk setiap skenario (laptop, Docker, Railway, VPS, homelab, Tauri, air-gapped)
- Target: DevOps, sysadmin, pengguna advanced

**u) OPS-RUNBOOK — Runbook Operasional**
- Referensi: [CB §40]
- Format: Markdown + decision trees
- Isi: Monitoring setup, alert definitions, common incidents, log analysis
- Target: DevOps, sysadmin

**v) OPS-BACKUP — Prosedur Backup & Recovery**
- Referensi: [CB §39]
- Format: Markdown + scripts
- Isi: Backup strategy, schedule, verification, recovery procedures, disaster recovery plan
- Target: DevOps, sysadmin

**w) OPS-CICD — Pipeline CI/CD**
- Referensi: [CB §37]
- Format: YAML (GitHub Actions) + Markdown
- Isi: Pipeline definition, build stages, test stages, release automation
- Target: DevOps, developer

**50.3.5 Kategori Pengguna & Desain:**

**x) USER-GUIDE — Panduan Pengguna**
- Referensi: [CB §31], [CB §18], [CB §17]
- Format: Markdown atau documentation site
- Isi: Quick start, instalasi, aktivasi lisensi, konfigurasi provider, fitur-fitur, FAQ, troubleshooting
- Target: Pengguna akhir

**y) ADMIN-GUIDE — Panduan Administrator**
- Referensi: [CB §16], [CB §7]
- Format: Markdown
- Isi: Setup admin, kelola tim, undang user, kelola Knowledge Base global, monitoring
- Target: Administrator tim

**z) DESIGN-SYSTEM — Design System**
- Referensi: [CB §33]
- Format: Storybook + Markdown
- Isi: Color palette, typography, spacing, component library, icon library, motion guidelines
- Target: Designer, developer frontend

**aa) COPY-STANDARDS — Standar Penulisan Konten**
- Referensi: [CB §29], [CB §28]
- Format: Markdown
- Isi: Tone of voice, istilah baku, format penulisan, error message guidelines
- Target: Content writer, developer, designer

**50.3.6 Kategori Bisnis & Legal:**

**bb) LEGAL-TOS — Terms of Service**
- Referensi: [CB §1], [CB §5]
- Format: Dokumen hukum
- Isi: Acceptance of terms, description of service, user responsibilities, limitation of liability
- Target: Pengguna akhir

**cc) LEGAL-PRIVACY — Privacy Policy**
- Referensi: [CB §5.11], [CB §5.12], [CB §5.13]
- Format: Dokumen hukum
- Isi: Data yang dikumpulkan, tujuan, data yang tidak dikumpulkan, user rights, security measures
- Target: Pengguna akhir

**dd) LEGAL-LICENSE-AGREEMENT — Perjanjian Lisensi**
- Referensi: [CB §15]
- Format: Dokumen hukum
- Isi: Tipe lisensi, hak dan kewajiban, restrictions, warranty disclaimer
- Target: Pembeli lisensi

**ee) BUSINESS-PRICING — Strategi Harga**
- Referensi: [CB §15.2]
- Format: Internal document
- Isi: Pricing tiers, feature gating, discount strategies, payment methods
- Target: Management, sales (internal)

**50.3.7 Kategori AI Agent Development:**

**ff) agen.md — Kontrak Perilaku AI Agen Pengembangan**
- Referensi: Seluruh dokumen CB
- Format: Markdown
- Isi: Identitas agen, hierarki kebenaran, aturan emas perilaku, protokol implementasi, protokol update ceklist
- Target: AI agent (Claude, GPT, dll)

**gg) IMPLEMENTASI-STATUS.md — Status Implementasi**
- Referensi: Seluruh dokumen CB
- Format: Markdown (living document)
- Isi: Status per fase, status per checklist item, update log, blockers, next actions
- Target: Developer, project manager, AI agent

### 50.4 Metadata Dokumen Turunan

**50.4.1** Setiap dokumen turunan harus memiliki metadata di bagian awal:

```markdown
---
title: [Nama Dokumen]
document_id: [SPEC-ARCH, SPEC-API, dll]
version: 1.0
cb_reference: [CB §X.Y, CB §A.B]
status: DRAFT | REVIEW | FINAL
owner: [Tim/individu yang bertanggung jawab]
last_updated: [YYYY-MM-DD]
---
```

### 50.5 Versi Dokumen Turunan

**50.5.1** Versi dokumen turunan harus sinkron dengan versi Contract Baseline.

**50.5.2** Skema versi: `MAJOR.MINOR.PATCH`
- MAJOR: Perubahan yang tidak backward compatible dengan CB
- MINOR: Penambahan fitur atau spesifikasi baru
- PATCH: Koreksi typo, klarifikasi, perbaikan kecil

**50.5.3** Contoh: `SPEC-ARCH v1.0` mengikuti `CB v1.0`

### 50.6 Prinsip Dokumen Turunan

**50.6.1** Self-contained: Pembaca tidak harus membuka CB untuk memahami dokumen turunan, tetapi referensi ke CB harus tetap dicantumkan untuk traceability.

**50.6.2** Konsisten: Format, terminologi, dan gaya harus konsisten dengan CB dan antar dokumen turunan.

**50.6.3** Terpelihara: Setiap perubahan pada CB harus diikuti dengan update dokumen turunan yang relevan.

**50.6.4** Traceable: Setiap keputusan dalam dokumen turunan harus dapat ditelusuri kembali ke pasal CB yang menjadi dasarnya.

### 50.7 Prioritas Pembuatan Dokumen Turunan

**50.7.1** Prioritas Tinggi (sebelum development dimulai):
1. SPEC-REPO
2. SPEC-ARCH
3. SPEC-DB
4. SPEC-API
5. agen.md
6. IMPLEMENTASI-STATUS.md

**50.7.2** Prioritas Sedang (saat development):
7. SPEC-GRAPH
8. SPEC-LLM
9. SPEC-KB
10. SPEC-LICENSE
11. SPEC-AUTH
12. SPEC-UI
13. DESIGN-SYSTEM
14. SPEC-WEB
15. SPEC-CITATION
16. SPEC-EXPORT
17. SPEC-A11Y

**50.7.3** Prioritas Pengujian (saat development):
18. TEST-PLAN
19. TEST-CASES
20. TEST-HALLUCINATION
21. TEST-PERFORMANCE
22. TEST-SECURITY

**50.7.4** Prioritas Deployment (sebelum launch):
23. DEPLOY-GUIDE
24. OPS-BACKUP
25. OPS-RUNBOOK
26. OPS-CICD

**50.7.5** Prioritas Dokumentasi (sebelum launch):
27. USER-GUIDE
28. ADMIN-GUIDE
29. LEGAL-TOS
30. LEGAL-PRIVACY
31. LEGAL-LICENSE-AGREEMENT

**50.7.6** Prioritas Pendukung (setelah launch):
32. COPY-STANDARDS
33. BUSINESS-PRICING

---

## 51. PERUBAHAN & VERSI

### 51.1 Prinsip Perubahan

**51.1.1** Contract Baseline ini adalah dokumen hidup yang dapat berkembang seiring dengan pemahaman yang lebih dalam tentang produk dan kebutuhan pengguna.

**51.1.2** Namun, perubahan tidak boleh dilakukan secara sembarangan. Setiap perubahan harus melalui proses yang terstruktur untuk menjaga integritas dokumen sebagai sumber kebenaran mutlak.

### 51.2 Jenis Perubahan

**51.2.1** Perubahan Mayor:
- Menambah atau menghapus prinsip produk
- Mengubah arsitektur fundamental
- Mengubah siklus agen
- Mengubah standar keterlacakan
- Mengubah kriteria penerimaan

**51.2.2** Perubahan Minor:
- Menambah fitur baru yang tidak bertentangan dengan prinsip
- Memperjelas spesifikasi yang ambigu
- Menambah opsi kustomisasi
- Menambah endpoint API

**51.2.3** Perubahan Patch:
- Koreksi typo
- Klarifikasi bahasa
- Perbaikan format
- Update referensi

### 51.3 Proses Perubahan

**51.3.1** Setiap perubahan harus melalui tahapan berikut:

**Tahap 1 — Proposal**
- Proposal perubahan tertulis yang menjelaskan:
  - Pasal yang diusulkan untuk diubah
  - Alasan perubahan
  - Dampak terhadap dokumen turunan
  - Alternatif yang dipertimbangkan

**Tahap 2 — Review**
- Review oleh seluruh stakeholder
- Diskusi dan klarifikasi
- Revisi proposal jika perlu

**Tahap 3 — Persetujuan**
- Persetujuan tertulis dari seluruh stakeholder
- Dokumentasi keputusan

**Tahap 4 — Implementasi**
- Update Contract Baseline
- Update dokumen turunan yang terpengaruh
- Komunikasi ke seluruh tim pengembangan

**Tahap 5 — Verifikasi**
- Verifikasi bahwa semua dokumen turunan konsisten dengan CB baru
- Verifikasi bahwa tidak ada pertentangan

### 51.4 Skema Versi

**51.4.1** Contract Baseline menggunakan skema versi `MAJOR.MINOR`:
- MAJOR: Perubahan yang tidak backward compatible
- MINOR: Penambahan fitur atau klarifikasi

**51.4.2** Contoh:
- `v1.0` — Versi awal
- `v1.1` — Penambahan fitur lisensi offline
- `v1.2` — Penambahan fitur multi-bahasa
- `v2.0` — Perubahan arsitektur fundamental

### 51.5 Changelog

**51.5.1** Setiap versi harus memiliki changelog yang mencatat:
- Tanggal rilis
- Jenis perubahan (major/minor/patch)
- Daftar perubahan
- Referensi ke dokumen turunan yang terpengaruh

**51.5.2** Format changelog:

```markdown
# Changelog

## [1.1] - 2026-09-15

### Added
- [CB §15.9] Lisensi offline untuk deployment air-gapped
- [CB §23.1.15] Tabel license untuk menyimpan lisen
