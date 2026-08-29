# PAUGERAN CONTRACT BASELINE — PERBARUAN LENGKAP

**Dokumen:** Contract Baseline  
**Produk:** PAUGERAN  
**Status:** FINAL — SUMBER KEBENARAN MUTLAK  
**Sifat:** Kontrak mengikat yang menjadi acuan tunggal bagi seluruh pengembangan, pengujian, deployment, dan dokumentasi turunan PAUGERAN  
**Bukan:** Roadmap, rencana sprint, backlog, fase implementasi, rencana evolusi, atau dokumen teknis terpisah

---

## KEDUDUKAN DOKUMEN

Dokumen ini adalah **sumber kebenaran mutlak** (*single source of truth*) bagi keseluruhan proyek PAUGERAN. Seluruh dokumen turunan — termasuk spesifikasi teknis, dokumen arsitektur, rencana pengujian, model data, spesifikasi API, panduan deployment, panduan pengguna, dan dokumen operasional — **harus** konsisten dengan dokumen ini. Jika terjadi pertentangan antara dokumen ini dan dokumen turunan manapun, **dokumen ini yang berlaku**.

Setiap referensi silang dalam dokumen turunan **wajib** menunjuk ke pasal dan ayat dalam dokumen ini menggunakan format: `[CB §X.Y]` di mana X adalah nomor bab dan Y adalah nomor sub-bab.

---

## DAFTAR ISI

**BAGIAN I — IDENTITAS & PRINSIP**
1. Definisi Produk
2. Tujuan Produk
3. Masalah yang Diselesaikan
4. Prinsip Produk
5. Aktor

**BAGIAN II — ARSITEKTUR & TEKNOLOGI**
6. Arsitektur Produk
7. Spesifikasi Stack Teknologi
8. Spesifikasi Custom Graph Engine
9. Spesifikasi Multi-Provider LLM
10. Spesifikasi Data
11. Spesifikasi Legal Knowledge Base
12. Spesifikasi Lisensi
13. Spesifikasi API

**BAGIAN III — PERILAKU AGEN**
14. Siklus Agen
15. Mode Pemahaman
16. Mode Penalaran
17. Spesifikasi Penelitian Web
18. Spesifikasi Output dengan Inline Citation
19. Spesifikasi Export Dokumen Profesional
20. Standar Keterlacakan
21. Standar Bahasa
22. Kontrak Perilaku

**BAGIAN IV — PRODUK & ANTARMUKA**
23. Spesifikasi Instalasi & Distribusi
24. Spesifikasi API Key Management
25. Spesifikasi Autentikasi & Otorisasi
26. Spesifikasi Manajemen Sesi
27. Spesifikasi Kustomisasi Antarmuka
28. Spesifikasi Aksesibilitas
29. Spesifikasi Antarmuka

**BAGIAN V — KEAMANAN & OPERASIONAL**
30. Spesifikasi Keamanan
31. Spesifikasi Deployment
32. Monitoring & Observabilitas
33. Backup & Recovery

**BAGIAN VI — KEPATUHAN & PENERIMAAN**
34. Larangan Produk
35. Kriteria Keberhasilan
36. Kriteria Penerimaan

**BAGIAN VII — TATA KELOLA DOKUMEN**
37. Spesifikasi Repositori
38. Dokumen Turunan
39. Penutup

---

# BAGIAN I — IDENTITAS & PRINSIP

---

## 1. DEFINISI PRODUK

**1.1** PAUGERAN adalah agen kecerdasan buatan untuk pemahaman masalah, penelitian, dan penalaran hukum Indonesia.

**1.2** PAUGERAN menerima uraian masalah dari pengguna melalui antarmuka web, kemudian secara bertahap membangun pemahaman terhadap persoalan tersebut melalui dialog adaptif dalam satu sesi obrolan yang terisolasi, mengidentifikasi fakta dan kekurangan informasi, merumuskan masalah hukum, melakukan penelitian terhadap sumber hukum yang relevan — baik dari basis pengetahuan internal maupun dari situs web resmi pemerintah dan sumber tepercaya lainnya — menguji berbagai argumentasi dan penafsiran, kemudian menghasilkan analisis hukum yang memiliki keterlacakan penuh antara kesimpulan, fakta, kaidah hukum, sumber hukum, dan penalarannya.

**1.3** PAUGERAN bukan mesin pencari pasal. PAUGERAN bukan mesin pemberi jawaban benar atau salah. PAUGERAN bukan chatbot umum yang memberikan respons instan tanpa penalaran terstruktur.

**1.4** PAUGERAN harus mampu menjelaskan: apa kesimpulannya, mengapa kesimpulan tersebut muncul, dasar apa yang digunakan, fakta apa yang mendukungnya, apa kelemahannya, dan apa yang dapat membuat kesimpulan tersebut berubah.

**1.5** PAUGERAN dioperasikan sebagai produk siap pakai berupa satu binary universal berbasis Rust yang menjalankan server HTTP. Binary ini menyajikan antarmuka web modern dan mesin penalaran hukum dalam satu kesatuan yang tidak terpisahkan. Binary ini dapat dijalankan di laptop pribadi, server cloud terkelola, VPS pribadi, atau homelab tanpa modifikasi kode.

**1.6** PAUGERAN tidak memiliki mode desktop terpisah dan tidak memiliki mode cloud terpisah. Satu binary, satu cara kerja, banyak cara deployment. Perbedaan deployment hanya ditentukan oleh environment variables saat startup.

**1.7** Autentikasi multi-user adalah fitur opsional yang diaktifkan melalui environment variable `AUTH_ENABLED=true`. Secara default, PAUGERAN berjalan tanpa autentikasi untuk penggunaan pribadi.

**1.8** PAUGERAN mendukung berbagai penyedia dan model LLM — tidak terbatas pada model ternama — selama model tersebut mendukung API yang kompatibel, memungkinkan pengguna memilih berdasarkan kebutuhan, ketersediaan, dan biaya.

**1.9** PAUGERAN menggunakan sistem lisensi untuk mengontrol penggunaan produk. Lisensi divalidasi secara berkala saat PAUGERAN terhubung ke internet dan harus aktif agar agen dapat digunakan, terpisah dari konfigurasi API key LLM.

**1.10** PAUGERAN memiliki Legal Knowledge Base — basis pengetahuan hukum internal yang dibangun dari peraturan, pasal, dan putusan yang pernah diteliti — yang dapat digunakan sebagai referensi untuk analisis di masa depan tanpa perlu melakukan penelitian ulang dari internet.

---

## 2. TUJUAN PRODUK

PAUGERAN harus memungkinkan pengguna untuk:

**2.1** menjalankan produk dengan satu perintah, baik berupa eksekusi binary langsung, perintah `docker run`, maupun klik deploy di platform cloud terkelola.

**2.2** memasukkan API key LLM melalui antarmuka web tanpa menyentuh terminal.

**2.3** memilih penyedia dan model LLM dari berbagai pilihan yang didukung, termasuk model yang tidak ternama, selama kompatibel dengan API yang didukung.

**2.4** membuat sesi obrolan baru kapan saja untuk topik hukum yang berbeda.

**2.5** menjelaskan masalah menggunakan bahasa natural dalam antarmuka chat.

**2.6** mendapatkan pertanyaan klarifikasi yang relevan dan adaptif.

**2.7** membangun pemahaman masalah secara bertahap dalam satu sesi obrolan.

**2.8** mengoreksi pemahaman PAUGERAN melalui dialog.

**2.9** menentukan kapan proses pemahaman dianggap cukup.

**2.10** meminta PAUGERAN melakukan penalaran hukum.

**2.11** memperoleh penelitian hukum dari basis pengetahuan internal dan dari situs web resmi pemerintah serta sumber tepercaya lainnya di internet.

**2.12** mengetahui dasar hukum yang digunakan dengan inline citation yang detail pada setiap bagian output.

**2.13** memahami penerapan hukum terhadap fakta.

**2.14** melihat argumentasi yang mendukung dan berlawanan.

**2.15** mengetahui ketidakpastian dan kelemahan analisis.

**2.16** melihat alternatif penafsiran.

**2.17** menelusuri setiap kesimpulan menuju dasar dan sumbernya melalui peta keterlacakan.

**2.18** memperoleh laporan hukum dalam Bahasa Indonesia yang profesional dan mudah dipahami.

**2.19** menyimpan seluruh data kasus secara privat di penyimpanan yang dikendalikan pengguna.

**2.20** mengekspor hasil analisis dalam format PDF profesional dan DOCX profesional dengan template yang dapat dikustomisasi.

**2.21** mengelola banyak sesi obrolan untuk kasus yang berbeda.

**2.22** membuka kembali sesi obrolan lama kapan saja tanpa batas waktu.

**2.23** menghapus sesi obrolan kapan saja secara permanen.

**2.24** mengunggah dokumen pendukung dalam format PDF, DOCX, dan TXT ke dalam sesi obrolan.

**2.25** mengganti atau menghapus API key kapan saja melalui pengaturan.

**2.26** mem-backup dan merestore seluruh data dengan mudah.

**2.27** mengustomisasi antarmuka pengguna meliputi tema warna, ukuran font, tata letak, dan bahasa sesuai preferensi pribadi.

**2.28** menyimpan preferensi kustomisasi secara persisten.

**2.29** mengelola anggota tim melalui sistem undangan ketika `AUTH_ENABLED=true`.

**2.30** menyediakan API key global untuk kenyamanan tim ketika `AUTH_ENABLED=true`.

**2.31** memantau penggunaan tim melalui statistik agregat.

**2.32** mengakses produk dari browser mana saja pada deployment cloud atau VPS.

**2.33** menyimpan peraturan, pasal, PP, dan sejenisnya yang pernah diteliti ke dalam Legal Knowledge Base untuk digunakan sebagai referensi di sesi berikutnya.

**2.34** mengelola Legal Knowledge Base: menambah, memperbarui, menandai sebagai tidak berlaku, dan menghapus entri.

**2.35** menggunakan PAUGERAN dengan aksesibilitas tinggi melalui keyboard shortcuts, command palette, screen reader support, dan mode aksesibilitas.

**2.36** mengaktivasi PAUGERAN dengan lisensi key yang valid pada penggunaan pertama.

**2.37** menggunakan PAUGERAN tanpa harus memasukkan lisensi key berulang kali selama produk terhubung ke internet secara berkala.

---

## 3. MASALAH YANG DISELESAIKAN

**3.1** PAUGERAN dibangun untuk menjawab masalah-masalah berikut yang melekat dalam sistem hukum Indonesia dan penggunaan AI untuk hukum:

**3.1.1** Fakta pengguna sering tidak lengkap.

**3.1.2** Istilah yang digunakan pengguna belum tentu merupakan istilah hukum yang tepat.

**3.1.3** Satu fakta dapat memiliki beberapa konsekuensi hukum.

**3.1.4** Satu masalah dapat melibatkan beberapa bidang hukum.

**3.1.5** Aturan hukum dapat berubah, memiliki pengecualian, dan bertentangan satu sama lain.

**3.1.6** Putusan pengadilan dapat memiliki fakta yang berbeda meskipun kasusnya tampak serupa.

**3.1.7** Suatu norma dapat memiliki beberapa interpretasi yang sah.

**3.1.8** Kekuatan suatu kesimpulan bergantung pada fakta dan bukti yang tersedia.

**3.1.9** Informasi hukum di internet memiliki tingkat keandalan yang berbeda.

**3.1.10** Data hukum bersifat sensitif dan harus dijaga kerahasiaannya.

**3.1.11** Advokat membutuhkan alat yang dapat dipertanggungjawabkan secara profesional.

**3.1.12** Pengguna membutuhkan isolasi data antar kasus yang berbeda.

**3.1.13** Instalasi produk AI hukum umumnya rumit dan membutuhkan keahlian teknis.

**3.1.14** Pengguna tidak ingin API key mereka disimpan di server pihak ketiga.

**3.1.15** Pengguna tidak ingin membuat akun untuk alat pribadi.

**3.1.16** Pengguna memiliki preferensi visual dan aksesibilitas yang berbeda.

**3.1.17** Solusi berbasis Python memiliki overhead performa yang signifikan untuk agen yang berjalan lokal.

**3.1.18** Firma hukum membutuhkan alat yang dapat digunakan bersama oleh tim dengan kontrol biaya terpusat.

**3.1.19** Profesional hukum membutuhkan deployment yang fleksibel, baik lokal untuk privasi maupun cloud untuk kolaborasi.

**3.1.20** Model AI ternama seringkali mahal atau tidak tersedia di wilayah tertentu; pengguna membutuhkan fleksibilitas untuk memilih model yang sesuai.

**3.1.21** Penelitian hukum yang sama sering diulang dari nol padahal peraturan yang sama telah diteliti sebelumnya.

**3.1.22** Output AI hukum seringkali tidak mencantumkan sumber secara detail, menyulitkan verifikasi profesional.

**3.1.23** Laporan hukum yang dihasilkan AI seringkali tidak siap digunakan dalam konteks profesional tanpa formatting ulang.

**3.1.24** Produk AI hukum perlu memiliki mekanisme lisensi yang jelas untuk penggunaan komersial yang berkelanjutan.

**3.2** Karena masalah-masalah di atas, PAUGERAN harus memahami sebelum menalar, menalar sebelum menyimpulkan, selalu dapat ditelusuri dengan sumber eksplisit, menjaga isolasi data setiap sesi obrolan, dapat diinstal dengan mudah oleh siapa saja, mendukung berbagai model LLM, menyediakan basis pengetahuan yang dapat dibangun dari waktu ke waktu, dan memiliki mekanisme lisensi yang adil.

---

## 4. PRINSIP PRODUK

Prinsip-prinsip berikut bersifat mengikat dan tidak dapat dikompromikan. Setiap keputusan teknis, setiap baris kode, dan setiap fitur harus tunduk pada prinsip-prinsip ini.

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

**P-11 — Privasi dan isolasi sesi**
Data dalam satu sesi obrolan tidak boleh bocor ke sesi lain. Setiap sesi adalah entitas terisolasi. Data pengguna tidak boleh bocor ke pihak ketiga.

**P-12 — Produk harus siap pakai**
PAUGERAN harus dapat digunakan segera setelah instalasi tanpa konfigurasi tambahan yang rumit oleh pengguna akhir.

**P-13 — Chat-first experience**
Antarmuka utama adalah chat yang intuitif. Fitur kompleks seperti peta keterlacakan dan laporan harus dapat diakses dari dalam chat tanpa meninggalkan konteks obrolan.

**P-14 — Instalasi satu langkah**
Pengguna harus dapat menjalankan PAUGERAN dengan satu perintah. Tidak perlu setup database terpisah, tidak perlu konfigurasi server, tidak perlu keahlian teknis.

**P-15 — API key adalah milik pengguna**
API key LLM disimpan terenkripsi di penyimpanan pengguna. API key tidak pernah dikirim ke server PAUGERAN. Pengguna memiliki kontrol penuh.

**P-16 — Data 100% lokal atau terkelola**
Seluruh data pengguna tersimpan di penyimpanan yang dikendalikan pengguna. Tidak ada replikasi ke pihak ketiga tanpa persetujuan eksplisit.

**P-17 — Autentikasi opsional**
Autentikasi multi-user adalah fitur opsional yang diaktifkan melalui `AUTH_ENABLED=true`. Secara default, PAUGERAN berjalan tanpa autentikasi untuk kemudahan penggunaan pribadi.

**P-18 — Sesi adalah entitas independen**
Setiap sesi obrolan berdiri sendiri. Sesi A tidak mengetahui keberadaan sesi B. Satu-satunya data yang bersifat global adalah API key, preferensi UI per pengguna, dan Legal Knowledge Base.

**P-19 — Kustomisasi adalah hak pengguna**
Pengguna harus dapat menyesuaikan antarmuka sesuai preferensi visual dan ergonomi mereka. Preferensi disimpan secara persisten dan diaplikasikan secara global.

**P-20 — Performa dan keamanan melalui Rust**
Mesin inti PAUGERAN dibangun di atas Rust untuk menjamin keamanan memori tanpa garbage collector, konkurensi berperforma tinggi melalui Tokio, ukuran biner yang kecil, startup time yang cepat, dan konsumsi resource yang minimal.

**P-21 — Universal binary**
PAUGERAN didistribusikan sebagai satu binary universal yang dapat dijalankan di laptop pribadi, server cloud, VPS, atau homelab tanpa modifikasi kode. Perbedaan deployment hanya ditentukan oleh environment variables.

**P-22 — First user is admin**
Saat `AUTH_ENABLED=true`, user pertama yang mendaftar otomatis mendapatkan peran admin. Admin bertanggung jawab mengelola anggota tim berikutnya melalui sistem undangan.

**P-23 — Admin adalah user biasa plus hak istimewa**
Admin memiliki data, sesi, dan preferensi sendiri seperti user biasa. Hak istimewa admin terbatas pada manajemen tim dan konfigurasi sistem.

**P-24 — Privacy-preserving administration**
Admin tidak dapat melihat API key pribadi user lain, isi sesi obrolan user lain, atau data pribadi user lain. Admin hanya dapat melihat metadata statistik untuk keperluan manajemen.

**P-25 — Fallback API key hierarchy**
API key dievaluasi dengan urutan: pertama API key pribadi user, kedua API key global yang disediakan admin, ketiga error jika keduanya tidak ada.

**P-26 — Multi-provider LLM agnostik**
PAUGERAN tidak mengunci pengguna pada satu penyedia atau model LLM tertentu. PAUGERAN mendukung berbagai penyedia dan model — termasuk yang tidak ternama — selama model tersebut menyediakan API yang kompatibel. Pengguna bebas memilih berdasarkan kebutuhan, ketersediaan, dan biaya.

**P-27 — Sumber eksplisit dalam setiap output**
Setiap klaim hukum, setiap kutipan, setiap referensi dalam output PAUGERAN harus disertai dengan inline citation yang detail — mencakup nama peraturan, nomor, tahun, pasal, dan URL sumber jika berasal dari internet. Tidak ada klaim tanpa sumber.

**P-28 — Penelitian web yang bertanggung jawab**
PAUGERAN hanya boleh mengakses situs web yang masuk dalam daftar putih (whitelist) yang mencakup situs pemerintah resmi dan sumber hukum tepercaya. Akses ke situs di luar whitelist dilarang. Setiap akses web harus mencantumkan sumber secara eksplisit dalam output.

**P-29 — Basis pengetahuan yang dapat dibangun**
Peraturan, pasal, PP, dan sejenisnya yang pernah diteliti oleh PAUGERAN dapat disimpan ke dalam Legal Knowledge Base internal. Basis pengetahuan ini menjadi sumber referensi untuk analisis di masa depan tanpa perlu melakukan penelitian ulang dari internet, mempercepat analisis dan mengurangi ketergantungan pada koneksi internet.

**P-30 — Lisensi yang adil dan transparan**
PAUGERAN menggunakan sistem lisensi untuk penggunaan berkelanjutan. Lisensi divalidasi secara berkala saat PAUGERAN online tanpa mengganggu pengguna. Jika lisensi tidak valid, agen tidak dapat digunakan, tetapi data pengguna tetap aman dan dapat diakses untuk keperluan backup atau migrasi.

**P-31 — Aksesibilitas adalah standar, bukan fitur tambahan**
PAUGERAN harus dapat digunakan oleh pengguna dengan berbagai kemampuan. Keyboard navigation, screen reader support, high contrast mode, dan reduced motion harus tersedia sebagai standar.

**P-32 — Export profesional siap pakai**
Laporan yang diekspor harus siap digunakan dalam konteks profesional tanpa perlu formatting ulang. Template harus memenuhi standar dokumen hukum Indonesia.

---

## 5. AKTOR

**5.1 Pengguna (User)**
Orang yang menggunakan PAUGERAN. Pengguna memiliki data, sesi, dan preferensi sendiri. Dalam deployment tanpa auth, semua pengguna adalah local user tunggal. Dalam deployment dengan auth aktif, pengguna adalah anggota tim yang di-invite oleh admin.

**5.2 Administrator (Admin)**
User pertama yang mendaftar saat `AUTH_ENABLED=true`. Admin memiliki hak istimewa untuk mengelola anggota tim, menyediakan API key global, mengelola Legal Knowledge Base, mengonfigurasi sistem, dan mengelola lisensi. Admin tetap memiliki data pribadi sendiri yang tidak dapat diakses oleh admin lain.

**5.3 PAUGERAN**
Agen AI yang melakukan wawancara adaptif dalam sesi obrolan, pemodelan masalah, penelitian hukum (dari basis pengetahuan internal dan internet), penalaran, pengujian, dan penyusunan laporan.

**5.4 Sesi Obrolan (Chat Session)**
Entitas yang menaungi satu topik analisis kasus. Setiap sesi terisolasi dari sesi lain dan memiliki ID unik, judul, daftar pesan, fakta yang diekstrak, dokumen yang diunggah, peta keterlacakan, laporan hukum, serta timestamp pembuatan dan pembaruan.

**5.5 Sumber Hukum**
Sumber eksternal yang digunakan sebagai dasar analisis, termasuk peraturan perundang-undangan, putusan pengadilan, doktrin, dan dokumen resmi lembaga. Sumber dapat berasal dari Legal Knowledge Base internal atau dari internet melalui penelitian web.

**5.6 Dokumen Pengguna**
Dokumen yang diberikan pengguna sebagai sumber fakta atau bukti, disimpan secara aman dan terisolasi dalam sesi obrolan.

**5.7 Penyedia LLM**
Layanan eksternal yang dipanggil oleh PAUGERAN menggunakan API key milik pengguna atau global. PAUGERAN mendukung berbagai penyedia termasuk Anthropic, OpenAI, dan penyedia lain yang kompatibel dengan API OpenAI atau Anthropic. PAUGERAN tidak menyimpan, memproses, atau meneruskan API key ke pihak lain selain penyedia resmi yang dipilih pengguna.

**5.8 Server Lisensi PAUGERAN**
Layanan eksternal yang memvalidasi lisensi key PAUGERAN. Server ini hanya menerima lisensi key dan identifier instalasi, tidak menerima data pengguna, API key LLM, atau konten sesi.

**5.9 Legal Knowledge Base**
Basis pengetahuan hukum internal yang dibangun dari peraturan, pasal, PP, dan sejenisnya yang pernah diteliti oleh PAUGERAN dan disimpan secara eksplisit oleh pengguna atau admin. Basis ini bersifat global (dapat diakses semua sesi) dan read-only selama analisis.

---

# BAGIAN II — ARSITEKTUR & TEKNOLOGI

---

## 6. ARSITEKTUR PRODUK

**6.1** PAUGERAN dioperasikan sebagai satu binary universal yang menjalankan server HTTP. Binary ini menyajikan antarmuka web dan mesin penalaran hukum dalam satu kesatuan.

**6.2** Arsitektur terdiri dari enam lapisan yang berjalan dalam satu proses binary:

**6.2.1 Lapisan HTTP Server (Axum)**
Menerima semua request HTTP, menyajikan file statis frontend, menangani API endpoints, dan melakukan streaming SSE. Middleware autentikasi bersifat kondisional berdasarkan `AUTH_ENABLED`.

**6.2.2 Lapisan Custom Graph Engine**
Mesin state machine yang menjalankan siklus 11 fase agen. Terdiri dari node executor, conditional edge evaluator, state machine, dan event streaming.

**6.2.3 Lapisan Multi-Provider LLM**
Router LLM yang mendukung berbagai penyedia dan model. Setiap penyedia dikonfigurasi secara independen dengan endpoint URL, API key, dan model name yang dapat dikustomisasi. Router memilih penyedia berdasarkan tugas dan preferensi pengguna.

**6.2.4 Lapisan Penelitian Web**
Modul HTTP client dengan whitelist domain yang melakukan scraping terhadap situs pemerintah dan sumber hukum tepercaya. Hasil scraping diproses, divalidasi, dan disimpan sementara atau permanen ke Legal Knowledge Base.

**6.2.5 Lapisan Legal Knowledge Base**
Basis pengetahuan internal yang menyimpan peraturan, pasal, PP, dan sejenisnya yang pernah diteliti. Dilengkapi dengan vector embeddings untuk pencarian semantik dan metadata keberlakuan.

**6.2.6 Lapisan Data**
SQLite sebagai database default, PostgreSQL sebagai opsi untuk skala besar, file storage untuk dokumen, enkripsi AES-256-GCM untuk API key, dan License Manager untuk validasi lisensi.

**6.3** Frontend SolidJS di-compile menjadi file statis yang di-serve oleh Axum HTTP server. Tidak ada server frontend terpisah.

**6.4** Alur akses universal: Binary selalu berjalan sebagai HTTP server. Browser atau WebView mengakses server ini melalui HTTP. Cara akses berbeda tergantung deployment tetapi binary, UI, dan format data tetap sama.

**6.5** Conditional auth middleware: Ketika `AUTH_ENABLED=false`, semua request di-handle sebagai single user tanpa autentikasi. Ketika `AUTH_ENABLED=true`, semua request divalidasi melalui JWT token.

**6.6** License middleware berjalan di latar belakang dan memeriksa validitas lisensi secara berkala saat PAUGERAN terhubung ke internet. Jika lisensi tidak valid, semua endpoint agen dikunci tetapi endpoint data dan pengaturan tetap dapat diakses.

---

## 7. SPESIFIKASI STACK TEKNOLOGI

**7.1 Backend — Rust**

**7.1.1** Framework web: Axum 0.7 atau lebih baru.

**7.1.2** Async runtime: Tokio 1.x dengan fitur full.

**7.1.3** Database client: SQLx 0.7 atau lebih baru dengan fitur runtime-tokio-rustls, sqlite, dan postgres.

**7.1.4** Serialization: Serde 1.x dengan fitur derive.

**7.1.5** HTTP client: Reqwest 0.11 atau lebih baru dengan fitur json, stream, dan rustls-tls.

**7.1.6** HTML parsing untuk web scraping: scraper crate.

**7.1.7** Error handling: Thiserror 1.x.

**7.1.8** Logging: Tracing 0.1 atau lebih baru.

**7.1.9** Enkripsi: aes-gcm 0.10 atau lebih baru.

**7.1.10** Password hashing: argon2 crate.

**7.1.11** Random generation: Rand 0.8 atau lebih baru.

**7.1.12** UUID: Uuid 1.x dengan fitur v4 dan serde.

**7.1.13** Date/time: Chrono 0.4 atau lebih baru dengan fitur serde.

**7.1.14** Streaming: Tokio-stream 0.1 atau lebih baru.

**7.1.15** PDF parsing: lopdf atau pdf-extract crate.

**7.1.16** DOCX parsing: docx-rs crate.

**7.1.17** PDF generation: printpdf atau genpdf crate dengan dukungan template profesional.

**7.1.18** Vector embeddings: candle atau ort crate untuk inferensi embedding lokal, atau API call untuk embedding remote.

**7.2 Frontend — SolidJS**

**7.2.1** Framework: SolidJS dengan fine-grained reactivity.

**7.2.2** Build tool: Vite 5 atau lebih baru.

**7.2.3** Bahasa: TypeScript 5 atau lebih baru.

**7.2.4** Styling: Tailwind CSS 3 atau lebih baru.

**7.2.5** Server state: TanStack Query.

**7.2.6** Graph visualization: Cytoscape.js.

**7.2.7** Rich text editor: TipTap.

**7.2.8** Markdown rendering: marked dengan plugin untuk inline citation.

**7.2.9** Internationalization: @solid-primitives/i18n.

**7.2.10** Command palette: cmdk-solid atau implementasi custom.

**7.2.11** Accessibility: @solidjs-aria atau implementasi ARIA manual.

**7.3 Database**

**7.3.1** Default: SQLite. Embedded, single file, tidak perlu server terpisah.

**7.3.2** Opsional: PostgreSQL. Untuk deployment skala besar. Auto-detect dari `DATABASE_URL`.

**7.3.3** SQLx mendukung kedua database dengan query yang sama melalui compile-time checking.

**7.3.4** Vector search untuk Legal Knowledge Base menggunakan sqlite-vec extension untuk SQLite atau pgvector untuk PostgreSQL.

**7.4 Desktop Wrapper — Tauri (Opsional)**

**7.4.1** Tauri 2.x sebagai wrapper tipis yang menjalankan binary PAUGERAN sebagai sidecar.

**7.4.2** Membuka WebView yang connect ke localhost.

**7.4.3** Memberikan native features: system tray, native notifications, auto-updater, file dialogs.

**7.5 License Server (Eksternal)**

**7.5.1** Server lisensi PAUGERAN adalah layanan terpisah yang memvalidasi lisensi key.

**7.5.2** Protokol validasi: HTTPS POST dengan payload `{license_key, installation_id, version}`.

**7.5.3** Response: `{valid: bool, expires_at: ISO8601, features: [...], message: string}`.

**7.5.4** Validasi dilakukan secara berkala (setiap 24 jam) saat PAUGERAN online.

**7.5.5** Grace period: 7 hari setelah validasi terakhir jika PAUGERAN offline.

---

## 8. SPESIFIKASI CUSTOM GRAPH ENGINE

**8.1** Custom Graph Engine adalah pengganti LangGraph yang dibangun native di Rust untuk performa optimal dan integrasi langsung dengan Tokio async runtime.

**8.2 Komponen Utama**

**8.2.1** `AgentGraph`: Definisi graph yang berisi nodes, edges, dan entry point.

**8.2.2** `Node` trait: Interface yang harus diimplementasikan oleh setiap fase agen.

**8.2.3** `Edge`: Enum dengan varian `Static(NodeId)` dan `Conditional(ConditionalEdgeFn)`.

**8.2.4** `AgentState`: Struct yang berisi session_id, user_id, messages, facts, documents, user_goals, identified_issues, retrieved_laws, arguments, counter_arguments, traceability_map, current_phase, facts_complete, dan report_generated.

**8.2.5** `ExecutionContext`: Struct yang berisi llm_router, knowledge_base, web_researcher, db, event_sender, dan cancellation_token.

**8.2.6** `AgentEvent`: Enum dengan varian PhaseStarted, PhaseCompleted, TokenStreamed, FactExtracted, LawRetrieved, WebSourceAccessed, KnowledgeBaseHit, ArgumentGenerated, Error, dan Completed.

**8.3 Graph Definition**

**8.3.1** Entry point: PAHAM.

**8.3.2** Edge statis: PAHAM → TANYA, TANYA → KONFIRMASI, RUMUSKAN → TELITI, TELITI → VERIFIKASI, VERIFIKASI → NALAR, NALAR → BANTAH, BANTAH → UJI, UJI → SIMPULKAN, SIMPULKAN → TELUSURI.

**8.3.3** Conditional edge dari KONFIRMASI: Jika facts_complete true maka ke RUMUSKAN, jika false maka ke TANYA.

**8.3.4** Conditional edge dari TELUSURI: Jika semua citation valid maka ke END, jika ada citation tidak valid maka ke TELITI.

**8.4 Fitur Wajib**

**8.4.1** Node eksekusi async.

**8.4.2** Conditional edges berdasarkan state.

**8.4.3** State persistence ke database setelah setiap node.

**8.4.4** Event streaming ke frontend via SSE.

**8.4.5** Cancellation support melalui CancellationToken.

**8.4.6** Error recovery: node yang gagal bisa di-retry tanpa mengulang dari awal.

**8.4.7** Setiap node harus menghasilkan inline citation untuk setiap klaim hukum.

**8.4.8** Setiap node harus mencatat sumber (Knowledge Base atau Web) untuk setiap informasi yang digunakan.

---

## 9. SPESIFIKASI MULTI-PROVIDER LLM

**9.1** PAUGERAN mendukung berbagai penyedia LLM melalui arsitektur provider-agnostic. Setiap penyedia diimplementasikan sebagai adapter yang mengimplementasikan trait `LlmProvider`.

**9.2 Trait LlmProvider**

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

**9.3 Provider Bawaan**

**9.3.1 AnthropicProvider**
- Endpoint default: `https://api.anthropic.com/v1/messages`
- Model: claude-3-5-sonnet, claude-3-5-haiku, claude-3-opus, dan model Claude lainnya
- Custom endpoint: dapat dikonfigurasi untuk proxy atau mirror
- Format: Anthropic Messages API

**9.3.2 OpenAIProvider**
- Endpoint default: `https://api.openai.com/v1`
- Model: gpt-4o, gpt-4o-mini, gpt-4-turbo, o1, o1-mini, dan model OpenAI lainnya
- Custom endpoint: dapat dikonfigurasi untuk Azure OpenAI atau proxy
- Format: OpenAI Chat Completions API

**9.3.3 OpenAICompatibleProvider**
- Provider generik untuk semua API yang kompatibel dengan format OpenAI
- Endpoint: dikonfigurasi pengguna
- Model: dikonfigurasi pengguna
- Contoh penggunaan: Groq, Together AI, Fireworks, OpenRouter, Mistral AI, DeepSeek, local Ollama, local LM Studio, local vLLM, dan penyedia lainnya
- Format: OpenAI Chat Completions API

**9.3.4 OllamaProvider (Opsional)**
- Endpoint default: `http://localhost:11434`
- Model: model lokal yang di-pull melalui Ollama
- Untuk penggunaan offline atau privasi maksimal
- Format: Ollama API

**9.4 Konfigurasi Provider per Pengguna**

Setiap pengguna (atau admin untuk global) dapat mengonfigurasi satu atau lebih provider:

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

**9.5 Model Routing**

**9.5.1** Untuk setiap tugas, router LLM memilih provider dan model berdasarkan:
- Preferensi pengguna (jika diset)
- Tipe tugas (interactive, reasoning, extraction)
- Ketersediaan provider
- Prioritas yang dikonfigurasi
- Biaya (opsional, berdasarkan metadata provider)

**9.5.2** Tugas interaktif (PAHAM, TANYA, KONFIRMASI): model cepat dan murah.

**9.5.3** Tugas penalaran (NALAR, BANTAH, UJI, SIMPULKAN): model dengan kemampuan reasoning tinggi.

**9.5.4** Tugas ekstraksi (TELITI, VERIFIKASI, TELUSURI): model dengan structured output yang baik.

**9.5.5** Fallback: Jika provider utama gagal, router mencoba provider berikutnya sesuai prioritas.

**9.6 UI Konfigurasi Provider**

**9.6.1** Halaman Settings → Provider LLM menampilkan daftar provider yang dikonfigurasi.

**9.6.2** Tombol "Tambah Provider" membuka form dengan pilihan tipe provider.

**9.6.3** Form konfigurasi mencakup: nama provider, endpoint URL, API key, model default, daftar model tersedia (auto-detect atau manual), timeout, dan prioritas.

**9.6.4** Tombol "Test Koneksi" untuk memvalidasi API key dan endpoint.

**9.6.5** Tombol "Detect Models" untuk mengambil daftar model dari provider.

**9.6.6** Setiap provider dapat diaktifkan, dinonaktifkan, dihapus, atau diedit.

**9.7 Model Selection per Sesi**

**9.7.1** Pengguna dapat memilih model yang digunakan untuk sesi tertentu melalui dropdown di header sesi.

**9.7.2** Pilihan model terbatas pada model yang tersedia dari provider yang dikonfigurasi.

**9.7.3** Default: model dengan prioritas tertinggi dari provider yang aktif.

---

## 10. SPESIFIKASI DATA

**10.1 Database Schema**

**10.1.1** Tabel `users`: id (TEXT PK), email (TEXT UNIQUE NOT NULL), password_hash (TEXT NOT NULL), name (TEXT NOT NULL), role (TEXT NOT NULL DEFAULT 'user' CHECK IN 'admin','user'), is_active (INTEGER NOT NULL DEFAULT 1), created_at (DATETIME), last_login (DATETIME).

**10.1.2** Tabel `api_keys`: id (TEXT PK), user_id (TEXT NOT NULL), provider (TEXT NOT NULL), encrypted_key (TEXT NOT NULL), is_active (INTEGER DEFAULT 1), created_at (DATETIME), updated_at (DATETIME), UNIQUE(user_id, provider).

**10.1.3** Tabel `llm_providers`: id (TEXT PK), user_id (TEXT NOT NULL), provider_type (TEXT NOT NULL), name (TEXT NOT NULL), endpoint_url (TEXT NOT NULL), encrypted_api_key (TEXT NOT NULL), default_model (TEXT), available_models (JSON), max_tokens (INTEGER), timeout_seconds (INTEGER), priority (INTEGER DEFAULT 1), is_active (INTEGER DEFAULT 1), created_at (DATETIME), updated_at (DATETIME).

**10.1.4** Tabel `global_llm_providers`: id (TEXT PK), provider_type (TEXT NOT NULL), name (TEXT NOT NULL), endpoint_url (TEXT NOT NULL), encrypted_api_key (TEXT NOT NULL), default_model (TEXT), available_models (JSON), max_tokens (INTEGER), timeout_seconds (INTEGER), priority (INTEGER DEFAULT 1), is_active (INTEGER DEFAULT 1), created_by (TEXT NOT NULL), created_at (DATETIME), updated_at (DATETIME).

**10.1.5** Tabel `global_api_keys`: id (TEXT PK), provider (TEXT NOT NULL UNIQUE), encrypted_key (TEXT NOT NULL), created_by (TEXT NOT NULL), created_at (DATETIME).

**10.1.6** Tabel `ui_preferences`: user_id (TEXT PK), preferences_json (TEXT NOT NULL), updated_at (DATETIME).

**10.1.7** Tabel `setup_state`: user_id (TEXT PK), is_setup_complete (INTEGER DEFAULT 0), created_at (DATETIME).

**10.1.8** Tabel `chat_sessions`: id (TEXT PK), user_id (TEXT NOT NULL), title (TEXT), status (TEXT DEFAULT 'created'), current_phase (TEXT), facts_complete (INTEGER DEFAULT 0), selected_model (TEXT), created_at (DATETIME), updated_at (DATETIME).

**10.1.9** Tabel `messages`: id (TEXT PK), session_id (TEXT FK → chat_sessions ON DELETE CASCADE), role (TEXT NOT NULL), content (TEXT NOT NULL), phase (TEXT), metadata (JSON), created_at (DATETIME).

**10.1.10** Tabel `facts`: id (TEXT PK), session_id (TEXT FK → chat_sessions ON DELETE CASCADE), content (TEXT NOT NULL), source (TEXT NOT NULL), status (TEXT NOT NULL), relevance (TEXT), certainty (TEXT), created_at (DATETIME).

**10.1.11** Tabel `documents`: id (TEXT PK), session_id (TEXT FK → chat_sessions ON DELETE CASCADE), filename (TEXT NOT NULL), file_type (TEXT NOT NULL), file_size (INTEGER NOT NULL), file_path (TEXT NOT NULL), uploaded_at (DATETIME).

**10.1.12** Tabel `legal_knowledge_base`: id (TEXT PK), title (TEXT NOT NULL), source_type (TEXT NOT NULL CHECK IN 'uu','pp','perpres','permen','putusan','doktrin','lainnya'), number (TEXT), year (INTEGER), effective_date (DATE), revoked_date (DATE), status (TEXT DEFAULT 'active' CHECK IN 'active','amended','revoked'), content (TEXT NOT NULL), metadata (JSON), added_by (TEXT NOT NULL), source_url (TEXT), created_at (DATETIME), updated_at (DATETIME).

**10.1.13** Tabel `legal_knowledge_articles`: id (TEXT PK), knowledge_id (TEXT FK → legal_knowledge_base ON DELETE CASCADE), article_number (TEXT NOT NULL), content (TEXT NOT NULL), embedding (BLOB), metadata (JSON).

**10.1.14** Tabel `legal_knowledge_embeddings`: id (TEXT PK), article_id (TEXT FK → legal_knowledge_articles ON DELETE CASCADE), embedding (BLOB NOT NULL), model (TEXT NOT NULL), created_at (DATETIME).

**10.1.15** Tabel `license`: id (INTEGER PK CHECK (id = 1)), license_key_encrypted (TEXT NOT NULL), installation_id (TEXT NOT NULL), validated_at (DATETIME), expires_at (DATETIME), is_valid (INTEGER DEFAULT 1), last_check_attempt (DATETIME), grace_period_until (DATETIME), features (JSON), created_at (DATETIME).

**10.1.16** Tabel `invitations`: id (TEXT PK), email (TEXT), token (TEXT UNIQUE NOT NULL), created_by (TEXT NOT NULL), expires_at (DATETIME NOT NULL), used_by (TEXT), used_at (DATETIME), created_at (DATETIME).

**10.1.17** Tabel `system_config`: key (TEXT PK), value (TEXT NOT NULL), updated_at (DATETIME).

**10.1.18** Tabel `admin_audit_logs`: id (TEXT PK), admin_id (TEXT NOT NULL), action (TEXT NOT NULL), target_user_id (TEXT), details (TEXT), ip_address (TEXT), created_at (DATETIME).

**10.1.19** Tabel `audit_logs`: id (TEXT PK), user_id (TEXT), session_id (TEXT), action (TEXT NOT NULL), details (JSON), ip_address (TEXT), created_at (DATETIME).

**10.1.20** Tabel `session_states`: session_id (TEXT PK FK → chat_sessions ON DELETE CASCADE), state_json (TEXT NOT NULL), updated_at (DATETIME).

**10.1.21** Tabel `traceability_edges`: id (TEXT PK), session_id (TEXT FK → chat_sessions ON DELETE CASCADE), conclusion_id (TEXT), reason (TEXT), rule_id (TEXT), fact_id (TEXT), evidence_source (TEXT), source_type (TEXT CHECK IN 'knowledge_base','web','document','user_statement'), source_url (TEXT), created_at (DATETIME).

**10.1.22** Index wajib: idx_messages_session_id, idx_facts_session_id, idx_documents_session_id, idx_traceability_session_id, idx_chat_sessions_user_id, idx_chat_sessions_status, idx_chat_sessions_updated_at (DESC), idx_api_keys_user_provider, idx_users_role, idx_users_is_active, idx_invitations_token, idx_legal_knowledge_source_type, idx_legal_knowledge_status, idx_legal_knowledge_year, idx_legal_articles_knowledge_id, idx_legal_embeddings_article_id.

**10.2 Penyimpanan File**

**10.2.1** Struktur: `{data_dir}/paugeran.db`, `{data_dir}/.secret`, `{data_dir}/documents/{user_id}/{session_id}/original/`, `{data_dir}/documents/{user_id}/{session_id}/processed/`, `{data_dir}/exports/{user_id}/`, `{data_dir}/logs/paugeran.log`, `{data_dir}/logs/audit.log`.

**10.2.2** File `.secret` berisi kunci enkripsi AES-256 dengan permission 600 di Unix.

**10.3 Retensi Data**

**10.3.1** Sesi aktif: disimpan selama pengguna menggunakan produk.

**10.3.2** Sesi archived: disimpan tanpa batas.

**10.3.3** Sesi deleted: dihapus permanen segera.

**10.3.4** Legal Knowledge Base: disimpan sampai dihapus oleh admin.

**10.3.5** Audit logs: disimpan 2 tahun.

**10.3.6** API keys: disimpan sampai dihapus pengguna.

**10.3.7** Preferensi UI: disimpan sampai di-reset pengguna.

**10.3.8** Lisensi: disimpan permanen, diperbarui saat validasi.

---

## 11. SPESIFIKASI LEGAL KNOWLEDGE BASE

**11.1** Legal Knowledge Base adalah basis pengetahuan hukum internal yang menyimpan peraturan, pasal, PP, dan sejenisnya yang pernah diteliti oleh PAUGERAN. Basis ini menjadi sumber referensi untuk analisis di masa depan.

**11.2** Hanya peraturan dan dokumen hukum formal yang dapat disimpan ke Legal Knowledge Base. Dokumen pengguna (kontrak pribadi, surat, dll) tidak disimpan di sini.

**11.3 Jenis Dokumen yang Dapat Disimpan**

**11.3.1** Undang-Undang (UU)
**11.3.2** Peraturan Pemerintah (PP)
**11.3.3** Peraturan Presiden (Perpres)
**11.3.4** Peraturan Menteri (Permen)
**11.3.5** Putusan Pengadilan (Mahkamah Agung, Pengadilan Tinggi, Pengadilan Negeri)
**11.3.6** Doktrin hukum (buku, jurnal)
**11.3.7** Lainnya (Surat Edaran, Peraturan Daerah, dll)

**11.4 Struktur Penyimpanan**

Setiap entri Legal Knowledge Base terdiri dari:
- **Metadata**: judul, jenis sumber, nomor, tahun, tanggal berlaku, tanggal dicabut, status, URL sumber
- **Konten**: teks lengkap peraturan
- **Pasal-pasal**: dipecah per pasal dengan embedding vector untuk pencarian semantik
- **Embeddings**: vektor numerik untuk setiap pasal, dihasilkan oleh model embedding

**11.5 Cara Penambahan ke Knowledge Base**

**11.5.1 Otomatis saat penelitian web**: Ketika PAUGERAN menemukan peraturan dari situs pemerintah selama fase TELITI, sistem menawarkan opsi "Simpan ke Knowledge Base" setelah analisis selesai.

**11.5.2 Manual oleh pengguna**: Pengguna dapat mengunggah file PDF peraturan dan meminta PAUGERAN mengekstrak dan menyimpannya ke Knowledge Base.

**11.5.3 Manual oleh admin**: Admin dapat melakukan bulk import peraturan melalui halaman admin.

**11.5.4 Import dari URL**: Pengguna atau admin dapat memberikan URL peraturan, dan PAUGERAN akan mengambil, memproses, dan menyimpannya.

**11.6 Pencarian di Knowledge Base**

**11.6.1** Pencarian semantik: Menggunakan vector embeddings untuk menemukan pasal yang relevan secara kontekstual.

**11.6.2** Pencarian keyword: Pencarian teks tradisional untuk kasus spesifik.

**11.6.3** Pencarian hybrid: Kombinasi keduanya untuk hasil terbaik.

**11.6.4** Filter: Berdasarkan jenis sumber, tahun, status (berlaku/dicabut).

**11.7 Penggunaan dalam Analisis**

**11.7.1** Selama fase TELITI, PAUGERAN pertama-tama mencari di Knowledge Base sebelum melakukan penelitian web.

**11.7.2** Jika ditemukan di Knowledge Base dan statusnya masih berlaku, PAUGERAN menggunakan entri tersebut tanpa perlu mengakses internet.

**11.7.3** Jika tidak ditemukan atau statusnya tidak pasti, PAUGERAN melakukan penelitian web untuk verifikasi.

**11.7.4** Setiap penggunaan entri Knowledge Base dicatat dalam traceability_edges dengan source_type='knowledge_base'.

**11.8 Manajemen Knowledge Base**

**11.8.1** Halaman Knowledge Base di UI menampilkan daftar semua entri dengan filter dan pencarian.

**11.8.2** Setiap entri dapat:
- Dilihat detailnya (metadata, konten, pasal-pasal)
- Diperbarui (jika ada amandemen)
- Ditandai sebagai tidak berlaku (revoked)
- Dihapus

**11.8.3** Admin dapat mengelola Knowledge Base global (berlaku untuk semua user).

**11.8.4** User dapat memiliki Knowledge Base pribadi (opsional, jika AUTH_ENABLED=true).

**11.9 Sinkronisasi dan Update**

**11.9.1** PAUGERAN dapat dikonfigurasi untuk memeriksa update peraturan secara berkala (opsional).

**11.9.2** Jika peraturan di Knowledge Base sudah dicabut atau diamendemen, sistem memberi notifikasi kepada pengguna.

**11.9.3** Pengguna dapat memilih untuk update manual atau otomatis.

**11.10 Privasi dan Lokasi Data**

**11.10.1** Legal Knowledge Base disimpan lokal di database PAUGERAN.

**11.10.2** Tidak ada data Knowledge Base yang dikirim ke server PAUGERAN atau pihak ketiga.

**11.10.3** Knowledge Base ikut ter-backup saat backup database.

---

## 12. SPESIFIKASI LISENSI

**12.1** PAUGERAN menggunakan sistem lisensi untuk mengontrol penggunaan produk. Lisensi diperlukan untuk menggunakan agen PAUGERAN, terpisah dari konfigurasi API key LLM.

**12.2** Lisensi key adalah string unik yang diterbitkan oleh server lisensi PAUGERAN. Lisensi key harus diinput saat aktivasi pertama atau melalui Settings.

**12.3** Setiap instalasi PAUGERAN memiliki `installation_id` unik yang di-generate saat pertama kali dijalankan dan disimpan di database.

**12.4** Tipe Lisensi**

**12.4.1** Trial: Lisensi percobaan dengan batas waktu (misalnya 14 hari) dan fitur terbatas.

**12.4.2** Personal: Lisensi untuk penggunaan individu dengan fitur penuh.

**12.4.3** Team: Lisensi untuk tim dengan multi-user support.

**12.4.4** Enterprise: Lisensi untuk organisasi besar dengan fitur tambahan dan SLA.

**12.5** Validasi Lisensi**

**12.5.1** Saat pertama kali dijalankan, PAUGERAN menampilkan halaman aktivasi yang meminta lisensi key.

**12.5.2** Validasi awal: PAUGERAN mengirim request ke server lisensi dengan payload:
```json
{
  "license_key": "...",
  "installation_id": "...",
  "version": "1.0.0",
  "timestamp": "ISO8601"
}
```

**12.5.3** Server lisensi membalas dengan:
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

**12.5.4** Jika valid, lisensi disimpan terenkripsi di database dan PAUGERAN dapat digunakan.

**12.5.5** Jika tidak valid, PAUGERAN menampilkan pesan error dan tidak mengizinkan penggunaan agen, tetapi data dan pengaturan tetap dapat diakses.

**12.6** Validasi Berkala**

**12.6.1** Setelah aktivasi, PAUGERAN melakukan validasi lisensi secara berkala setiap 24 jam saat terhubung ke internet.

**12.6.2** Validasi dilakukan di background tanpa mengganggu pengguna.

**12.6.3** Pengguna tidak perlu memasukkan lisensi key berulang kali.

**12.6.4** Jika validasi gagal (server tidak dapat dijangkau), PAUGERAN memasuki grace period 7 hari.

**12.6.5** Selama grace period, PAUGERAN tetap dapat digunakan.

**12.6.6** Jika grace period berakhir tanpa validasi berhasil, agen dikunci hingga validasi berhasil.

**12.6.7** Data pengguna tetap aman dan dapat diakses untuk backup atau migrasi.

**12.7** Lisensi di Settings**

**12.7.1** Halaman Settings → Lisensi menampilkan:
- Status lisensi (aktif, expired, grace period)
- Tipe lisensi
- Tanggal kedaluwarsa
- Fitur yang tersedia
- Tombol "Perbarui Lisensi" untuk mengganti lisensi key
- Tombol "Validasi Sekarang" untuk validasi manual

**12.7.2** Lisensi key ditampilkan dalam bentuk masked (hanya beberapa karakter terakhir terlihat).

**12.8** Offline Mode**

**12.8.1** PAUGERAN dapat digunakan tanpa koneksi internet selama grace period belum berakhir.

**12.8.2** Fitur yang membutuhkan internet (penelitian web, validasi lisensi) tidak tersedia saat offline.

**12.8.3** Analisis dengan Knowledge Base internal tetap dapat dilakukan saat offline.

**12.9** Environment Variable untuk Lisensi**

**12.9.1** `LICENSE_KEY`: Lisensi key dapat di-set via environment variable untuk deployment otomatis.

**12.9.2** `LICENSE_SERVER_URL`: URL server lisensi (default: `https://license.paugeran.com`).

**12.9.3** `LICENSE_CHECK_INTERVAL`: Interval validasi dalam jam (default: 24).

**12.9.4** `LICENSE_GRACE_PERIOD`: Grace period dalam hari (default: 7).

**12.10** Keamanan Lisensi**

**12.10.1** Lisensi key disimpan terenkripsi di database.

**12.10.2** Request ke server lisensi menggunakan HTTPS.

**12.10.3** Payload request di-sign dengan installation_id untuk mencegah replay attack.

**12.10.4** Server lisensi tidak menerima data pengguna, API key LLM, atau konten sesi.

**12.11** Fallback untuk Deployment Terisolasi**

**12.11.1** Untuk deployment di environment yang tidak dapat terhubung ke internet (air-gapped), admin dapat mengonfigurasi lisensi offline.

**12.11.2** Lisensi offline adalah file ter-sign yang di-generate oleh server lisensi dan di-import ke PAUGERAN.

**12.11.3** Lisensi offline memiliki masa berlaku terbatas dan harus diperbarui secara manual.

---

## 13. SPESIFIKASI API

**13.1 Base URL**

**13.1.1** Local: `http://localhost:{PORT}/api/v1`

**13.1.2** Cloud: `https://{domain}/api/v1`

**13.2 Autentikasi**

**13.2.1** Ketika `AUTH_ENABLED=false`: Tidak ada header Authorization yang diperlukan.

**13.2.2** Ketika `AUTH_ENABLED=true`: Header `Authorization: Bearer {jwt_token}` dan `Content-Type: application/json`.

**13.3 Middleware Lisensi**

**13.3.1** Semua endpoint agen (sessions, messages, analysis) memerlukan lisensi valid.

**13.3.2** Jika lisensi tidak valid, endpoint mengembalikan HTTP 402 Payment Required dengan pesan yang jelas.

**13.3.3** Endpoint data (settings, backup, export lama) tetap dapat diakses tanpa lisensi valid.

**13.4 Endpoint Setup**

**13.4.1** `GET /setup/status`: Mengembalikan status setup termasuk is_setup_complete, has_license, has_llm_providers.

**13.4.2** `POST /setup/complete`: Menandai setup selesai setelah lisensi dan minimal satu provider dikonfigurasi.

**13.5 Endpoint Lisensi**

**13.5.1** `GET /license/status`: Mengembalikan status lisensi (masked key, tipe, expires_at, features).

**13.5.2** `POST /license/activate`: Menerima license_key, validasi ke server lisensi, simpan jika valid.

**13.5.3** `POST /license/validate`: Validasi manual lisensi saat ini.

**13.5.4** `POST /license/import-offline`: Import lisensi offline untuk deployment air-gapped.

**13.6 Endpoint Auth (hanya jika AUTH_ENABLED=true)**

**13.6.1** `POST /auth/register`: User pertama otomatis admin, user berikutnya butuh invite_code.

**13.6.2** `POST /auth/login`: Login dengan email dan password.

**13.7 Endpoint Settings — LLM Providers**

**13.7.1** `GET /settings/providers`: Daftar provider LLM yang dikonfigurasi user.

**13.7.2** `POST /settings/providers`: Tambah provider baru.

**13.7.3** `PATCH /settings/providers/{id}`: Update provider.

**13.7.4** `DELETE /settings/providers/{id}`: Hapus provider.

**13.7.5** `POST /settings/providers/{id}/test`: Test koneksi ke provider.

**13.7.6** `POST /settings/providers/{id}/detect-models`: Deteksi model yang tersedia dari provider.

**13.8 Endpoint Settings — API Keys (Legacy)**

**13.8.1** `GET /settings/api-keys`: Daftar API key legacy (untuk kompatibilitas).

**13.8.2** `POST /settings/api-keys`: Tambah API key legacy.

**13.8.3** `DELETE /settings/api-keys/{provider}`: Hapus API key legacy.

**13.9 Endpoint Settings — UI Preferences**

**13.9.1** `GET /settings/preferences`: Preferensi UI.

**13.9.2** `PUT /settings/preferences`: Update preferensi.

**13.9.3** `POST /settings/preferences/reset`: Reset ke default.

**13.10 Endpoint Legal Knowledge Base**

**13.10.1** `GET /knowledge`: Daftar entri Knowledge Base dengan filter dan pagination.

**13.10.2** `POST /knowledge`: Tambah entri baru (manual atau dari URL).

**13.10.3** `GET /knowledge/{id}`: Detail entri.

**13.10.4** `PATCH /knowledge/{id}`: Update entri.

**13.10.5** `DELETE /knowledge/{id}`: Hapus entri.

**13.10.6** `POST /knowledge/{id}/refresh`: Refresh entri dari sumber asli.

**13.10.7** `POST /knowledge/import`: Bulk import dari file atau URL.

**13.10.8** `POST /knowledge/search`: Pencarian semantik di Knowledge Base.

**13.11 Endpoint Sessions**

**13.11.1** `GET /sessions`: Daftar sesi milik user saat ini.

**13.11.2** `POST /sessions`: Membuat sesi baru dengan title dan selected_model opsional.

**13.11.3** `GET /sessions/{session_id}`: Detail sesi.

**13.11.4** `PATCH /sessions/{session_id}`: Update title, status, atau selected_model.

**13.11.5** `DELETE /sessions/{session_id}`: Hapus sesi permanen.

**13.11.6** `POST /sessions/{session_id}/duplicate`: Duplikasi sesi.

**13.12 Endpoint Messages**

**13.12.1** `POST /sessions/{session_id}/messages`: Kirim pesan dan trigger agen.

**13.12.2** `GET /sessions/{session_id}/messages/stream`: SSE stream.

**13.12.3** `POST /sessions/{session_id}/cancel`: Batalkan eksekusi agen.

**13.13 Endpoint Documents**

**13.13.1** `POST /sessions/{session_id}/documents`: Upload dokumen.

**13.13.2** `GET /sessions/{session_id}/documents`: Daftar dokumen.

**13.13.3** `DELETE /sessions/{session_id}/documents/{document_id}`: Hapus dokumen.

**13.14 Endpoint Analysis**

**13.14.1** `POST /sessions/{session_id}/analysis/confirm`: Konfirmasi pemahaman.

**13.14.2** `POST /sessions/{session_id}/analysis/start-reasoning`: Mulai penalaran.

**13.14.3** `GET /sessions/{session_id}/analysis/report`: Ambil laporan.

**13.14.4** `POST /sessions/{session_id}/analysis/export`: Export ke PDF atau DOCX.

**13.14.5** `POST /sessions/{session_id}/analysis/save-to-knowledge`: Simpan peraturan yang diteliti ke Knowledge Base.

**13.15 Endpoint Traceability**

**13.15.1** `GET /sessions/{session_id}/traceability`: Nodes dan edges untuk visualisasi.

**13.16 Endpoint Admin (hanya jika AUTH_ENABLED=true dan role=admin)**

**13.16.1** `GET /admin/users`: Daftar user.

**13.16.2** `PATCH /admin/users/{user_id}`: Update role/status.

**13.16.3** `DELETE /admin/users/{user_id}`: Hapus user.

**13.16.4** `POST /admin/invitations`: Buat undangan.

**13.16.5** `GET /admin/stats/overview`: Statistik.

**13.16.6** `GET /admin/providers`: Daftar provider global.

**13.16.7** `POST /admin/providers`: Tambah provider global.

**13.16.8** `PATCH /admin/providers/{id}`: Update provider global.

**13.16.9** `DELETE /admin/providers/{id}`: Hapus provider global.

**13.16.10** `GET /admin/knowledge`: Kelola Knowledge Base global.

**13.16.11** `POST /admin/knowledge/import`: Bulk import ke Knowledge Base global.

**13.16.12** `GET /admin/license`: Status lisensi tim.

**13.16.13** `PUT /admin/config`: Konfigurasi sistem.

**13.17 Endpoint Health**

**13.17.1** `GET /health`: Status, version, database, auth_enabled, license_valid, timestamp.

**13.18 Format Error**

**13.18.1** Format: `{"error": {"code": "ERROR_CODE", "message": "...", "details": [...], "timestamp": "...", "request_id": "..."}}`.

**13.18.2** Error codes: VALIDATION_ERROR, NOT_FOUND, FORBIDDEN, UNAUTHORIZED, LLM_ERROR, NO_LLM_PROVIDER, NO_LICENSE, LICENSE_EXPIRED, LICENSE_INVALID, CANCELLED, INVITE_CODE_REQUIRED, INVITE_CODE_INVALID, CANNOT_DEMOTE_SELF, LAST_ADMIN, KNOWLEDGE_BASE_ERROR, INTERNAL_ERROR.

**13.19 Rate Limiting**

**13.19.1** Login: 5 requests per minute per IP.

**13.19.2** Register: 3 requests per hour per IP.

**13.19.3** Create session: 60 per jam per user.

**13.19.4** Send message: 120 per menit per sesi.

**13.19.5** Upload document: 20 per jam per sesi.

**13.19.6** Export report: 60 per jam per user.

**13.19.7** Knowledge base search: 200 per jam per user.

---

# BAGIAN III — PERILAKU AGEN

---

## 14. SIKLUS AGEN

**14.1** Siklus agen terdiri dari 11 fase yang dijalankan secara berurutan: PAHAM → TANYA → KONFIRMASI → RUMUSKAN → TELITI → VERIFIKASI → NALAR → BANTAH → UJI → SIMPULKAN → TELUSURI → END.

**14.2 Fase PAHAM**
Input: Pesan pertama pengguna. Proses: Ekstrak informasi dasar. Output: Fakta awal dan tujuan pengguna. Durasi target: kurang dari 5 detik.

**14.3 Fase TANYA**
Input: Fakta awal. Proses: Identifikasi informasi yang hilang, generate pertanyaan adaptif. Output: 1 hingga 3 pertanyaan klarifikasi. Durasi target: kurang dari 5 detik.

**14.4 Fase KONFIRMASI**
Input: Semua fakta yang terkumpul. Proses: Susun rekonstruksi masalah. Output: Ringkasan pemahaman dengan tombol Setuju atau Revisi. Durasi target: kurang dari 5 detik. Conditional: Jika facts_complete false maka kembali ke TANYA, jika true maka lanjut ke RUMUSKAN.

**14.5 Fase RUMUSKAN**
Input: Fakta yang dikonfirmasi. Proses: Identifikasi isu hukum, bidang hukum, yurisdiksi. Output: Daftar isu hukum dan klasifikasi masalah. Durasi target: kurang dari 10 detik.

**14.6 Fase TELITI**
Input: Isu hukum. Proses: Cari peraturan, putusan, doktrin yang relevan. Output: Daftar sumber hukum dengan metadata. Durasi target: kurang dari 60 detik.

**14.6.1** Urutan pencarian:
1. Legal Knowledge Base internal (paling cepat, offline)
2. Dokumen yang diunggah pengguna
3. Penelitian web ke situs pemerintah dan sumber tepercaya

**14.6.2** Setiap sumber yang ditemukan harus dicatat dengan:
- Tipe sumber (knowledge_base, document, web)
- Metadata lengkap (nomor, tahun, pasal)
- URL sumber (untuk web)
- Status keberlakuan

**14.6.3** Setelah penelitian, PAUGERAN menawarkan opsi "Simpan ke Knowledge Base" untuk peraturan yang ditemukan dari web.

**14.7 Fase VERIFIKASI**
Input: Sumber hukum yang ditemukan. Proses: Periksa keberlakuan, status, tanggal efektif. Output: Sumber yang terverifikasi dengan status. Durasi target: kurang dari 15 detik.

**14.8 Fase NALAR**
Input: Fakta dan sumber hukum terverifikasi. Proses: Terapkan hukum pada fakta, analisis unsur, bangun argumen. Output: Argumen pendukung dengan inline citation. Durasi target: kurang dari 30 detik.

**14.9 Fase BANTAH**
Input: Argumen pendukung. Proses: Cari kelemahan, pengecualian, kontra-argumen, putusan berlawanan. Output: Argumen berlawanan dengan inline citation. Durasi target: kurang dari 30 detik.

**14.10 Fase UJI**
Input: Argumen pendukung dan berlawanan. Proses: Evaluasi kekuatan kedua sisi. Output: Penilaian berimbang dengan tingkat kepastian. Durasi target: kurang dari 20 detik.

**14.11 Fase SIMPULKAN**
Input: Hasil pengujian. Proses: Susun kesimpulan dengan kategori kepastian. Output: Kesimpulan hukum dengan inline citation. Durasi target: kurang dari 15 detik.

**14.12 Fase TELUSURI**
Input: Kesimpulan dan semua data. Proses: Bangun peta keterlacakan, validasi setiap citation. Output: Peta keterlacakan lengkap dan laporan 24 poin. Durasi target: kurang dari 20 detik. Conditional: Jika ada kesimpulan tanpa citation valid maka kembali ke TELITI.

---

## 15. MODE PEMAHAMAN

**15.1 Wawancara Bertingkat**

**15.1.1 Tingkat 1 — Identifikasi Struktur Dasar:** Siapa pihak yang terlibat, apa yang terjadi, kapan kejadian, di mana lokasi, apa tujuan pengguna.

**15.1.2 Tingkat 2 — Fakta Material:** Apakah ada perjanjian tertulis, bukti pembayaran, komunikasi tertulis, saksi, dokumen pendukung.

**15.1.3 Tingkat 3 — Fakta Kritis:** Apakah ada pengecualian atau kondisi khusus, sengketa fakta, proses hukum yang sudah berjalan, perubahan perjanjian, tindakan pihak lawan yang relevan.

**15.2 Kriteria Pertanyaan**

**15.2.1** Setiap pertanyaan harus memiliki alasan yang dapat dijelaskan secara internal.

**15.2.2** Pertanyaan diprioritaskan berdasarkan pengaruhnya terhadap klasifikasi masalah, penerapan norma, unsur hukum, pembuktian, kemungkinan hasil, dan strategi.

**15.2.3** Pertanyaan yang tidak mempunyai pengaruh material harus dihindari.

**15.3 Rekonstruksi Masalah**

**15.3.1** PAUGERAN harus secara berkala memberikan ringkasan yang mencakup: Tujuan, Para Pihak, Kronologi, Fakta yang Telah Disampaikan, Fakta yang Belum Jelas, Dokumen, Masalah Potensial, dan Hal yang Masih Perlu Dipastikan.

**15.3.2** Ringkasan harus diakhiri dengan pertanyaan: "Apakah pemahaman saya sudah sesuai?" disertai opsi Setuju dan Revisi.

---

## 16. MODE PENALARAN

**16.1 Penelitian Hukum**

**16.1.1** Langkah penelitian: identifikasi bidang hukum, identifikasi yurisdiksi, tentukan periode waktu, cari di Knowledge Base, cari di dokumen pengguna, cari di web, cari interpretasi dan doktrin, identifikasi pengecualian, cari sumber yang berlawanan, verifikasi silang.

**16.2 Hirarki Sumber**

**16.2.1** Urutan prioritas: Undang-Undang, Peraturan Pemerintah, Peraturan Presiden, Peraturan Menteri, Putusan Mahkamah Agung, Putusan Pengadilan Tinggi/Negeri, Doktrin/Literatur Hukum, Sumber sekunder lainnya.

**16.3 Pemeriksaan Keberlakuan**

**16.3.1** Untuk setiap peraturan penting, sistem harus menentukan: nomor, nama, tanggal, tanggal mulai berlaku, status, perubahan, pencabutan, peraturan terkait, dan relevansi terhadap waktu kejadian.

**16.3.2** PAUGERAN tidak boleh menggunakan norma historis sebagai norma saat ini tanpa menjelaskan statusnya.

**16.4 Model Norma**

**16.4.1** Setiap norma direpresentasikan dengan struktur: Sumber, Pasal/ketentuan, Subjek, Perbuatan/kondisi, Syarat, Larangan/kewajiban/hak, Akibat hukum, Pengecualian.

---

## 17. SPESIFIKASI PENELITIAN WEB

**17.1** PAUGERAN dapat melakukan penelitian web untuk menemukan sumber hukum dari internet. Penelitian web hanya dilakukan pada situs yang masuk dalam daftar putih (whitelist).

**17.2 Whitelist Domain**

**17.2.1** Domain pemerintah resmi:
- `jdih.setkab.go.id` (Jaringan Dokumentasi dan Informasi Hukum Nasional)
- `mahkamahagung.go.id` (Mahkamah Agung RI)
- `putusan.mahkamahagung.go.id` (Direktori Putusan)
- `jdih.kemenkeu.go.id` (JDIH Kemenkeu)
- `jdih.kumham.go.id` (JDIH Kemenkumham)
- `peraturan.bpk.go.id` (BPJN BPK)
- `ojk.go.id` (Otoritas Jasa Keuangan)
- `bi.go.id` (Bank Indonesia)
- `kppu.go.id` (Komisi Pengawas Persaingan Usaha)
- `bppn.go.id` (Badan Pengawas Perdagangan Berjangka Komoditi)

**17.2.2** Domain pengadilan:
- `pt-*.go.id` (Pengadilan Tinggi)
- `pn-*.go.id` (Pengadilan Negeri)
- `pa-*.go.id` (Pengadilan Agama)
- `ptun-*.go.id` (Pengadilan Tata Usaha Negara)

**17.2.3** Domain tepercaya lainnya (dapat dikonfigurasi admin):
- `houkou.co.id`
- `hukumonline.com` (dengan catatan)
- Domain lain yang ditambahkan admin

**17.2.4** Admin dapat menambah atau menghapus domain dari whitelist melalui Settings → Penelitian Web.

**17.3 Mekanisme Penelitian Web**

**17.3.1** PAUGERAN menggunakan HTTP client (Reqwest) untuk mengambil halaman web.

**17.3.2** HTML di-parse menggunakan scraper crate untuk mengekstrak konten relevan.

**17.3.3** Konten dibersihkan dari elemen non-esensial (navigasi, iklan, footer).

**17.3.4** Metadata diekstrak: judul, tanggal terbit, nomor peraturan, URL.

**17.3.5** Konten di-chunk menjadi bagian-bagian yang dapat diproses oleh LLM.

**17.3.6** Setiap chunk dicatat dengan URL sumber untuk inline citation.

**17.4 Rate Limiting dan Etika**

**17.4.1** PAUGERAN menghormati robots.txt dari setiap situs.

**17.4.2** Delay minimum 1 detik antara request ke domain yang sama.

**17.4.3** User-Agent header mengidentifikasi PAUGERAN dengan jelas: `Paugeran/1.0 (+https://paugeran.com)`.

**17.4.4** Maksimal 10 request per menit per domain.

**17.4.5** Jika situs mengembalikan error 429 atau 503, PAUGERAN berhenti mengakses domain tersebut untuk sesi tersebut.

**17.5 Penanganan Kegagalan**

**17.5.1** Jika situs tidak dapat diakses (timeout, DNS error, dll), PAUGERAN mencatat kegagalan dan melanjutkan ke sumber berikutnya.

**17.5.2** PAUGERAN tidak menyembunyikan kegagalan akses dari pengguna.

**17.5.3** Output harus mencantumkan: "Situs [URL] tidak dapat diakses pada [waktu]. Analisis berdasarkan sumber alternatif."

**17.6 Integrasi dengan Knowledge Base**

**17.6.1** Setelah penelitian web selesai, PAUGERAN menawarkan opsi untuk menyimpan peraturan yang ditemukan ke Knowledge Base.

**17.6.2** Jika pengguna menyetujui, peraturan diproses (dibersihkan, dipecah per pasal, di-embed) dan disimpan ke Knowledge Base.

**17.6.3** Peraturan yang disimpan dari web tetap mencantumkan URL sumber asli.

---

## 18. SPESIFIKASI OUTPUT DENGAN INLINE CITATION

**18.1** Setiap output PAUGERAN — baik dalam chat, laporan, maupun ekspor — harus mencantumkan inline citation yang detail untuk setiap klaim hukum.

**18.2 Format Inline Citation**

**18.2.1** Dalam teks chat:
```
Berdasarkan Pasal 1338 Kitab Undang-Undang Hukum Perdata, perjanjian yang dibuat secara sah berlaku sebagai undang-undang bagi mereka yang membuatnya. [sumber: KUHPerdata, Pasal 1338]
```

**18.2.2** Dalam laporan:
```
Dasar hukum utama dalam kasus ini adalah Undang-Undang Nomor 13 Tahun 2003 tentang Ketenagakerjaan, khususnya Pasal 158 yang mengatur tentang pemutusan hubungan kerja. [sumber: UU 13/2003, Pasal 158, diakses dari JDIH Setkab pada 28 Agustus 2026]
```

**18.2.3** Format standar inline citation:
- Nama peraturan lengkap
- Nomor dan tahun
- Pasal/angka/bagian spesifik
- Sumber (Knowledge Base, web URL, atau dokumen pengguna)
- Tanggal akses (untuk web)

**18.3 Visualisasi Inline Citation di UI**

**18.3.1** Inline citation ditampilkan sebagai tooltip yang muncul saat hover.

**18.3.2** Klik pada citation membuka panel detail dengan:
- Metadata lengkap peraturan
- Konten pasal yang dikutip
- Link ke sumber asli (untuk web)
- Tombol "Lihat di Knowledge Base" (jika ada)

**18.3.3** Citation yang berasal dari Knowledge Base ditandai dengan ikon khusus.

**18.3.4** Citation yang berasal dari web ditandai dengan ikon link.

**18.4 Log Reasoning dengan Sumber Detail**

**18.4.1** PAUGERAN menyediakan "Log Reasoning" yang dapat diakses pengguna untuk melihat proses penalaran secara lengkap.

**18.4.2** Log reasoning menampilkan:
- Setiap langkah penalaran
- Sumber yang digunakan di setiap langkah
- Keputusan yang diambil (misalnya: memilih interpretasi A daripada B)
- Alasan keputusan

**18.4.3** Log reasoning dapat di-export bersama laporan.

**18.5 Konsistensi Citation**

**18.5.1** Citation yang sama di seluruh output harus merujuk ke entri yang sama di database.

**18.5.2** Jika peraturan dikutip beberapa kali, hanya satu entri yang disimpan di traceability_edges.

**18.5.3** Daftar sumber di akhir laporan harus konsisten dengan inline citation di seluruh laporan.

---

## 19. SPESIFIKASI EXPORT DOKUMEN PROFESIONAL

**19.1** PAUGERAN dapat mengekspor hasil analisis ke dalam format PDF profesional dan DOCX profesional yang siap digunakan dalam konteks profesional tanpa formatting ulang.

**19.2 Format PDF Profesional**

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
- 24 poin laporan sesuai struktur [CB §14]
- Daftar Sumber di akhir
- Lampiran (jika ada)

**19.2.4** Fitur khusus:
- Bookmark PDF untuk setiap poin (navigasi cepat)
- Hyperlink untuk citation (klik untuk lihat detail)
- Hyperlink untuk URL sumber (untuk web)
- Metadata PDF: judul, penulis, subjek, kata kunci
- Kompresi gambar (jika ada) untuk ukuran file optimal

**19.3 Format DOCX Profesional**

**19.3.1** Menggunakan template standar dokumen hukum Indonesia.

**19.3.2** Style Heading 1-3 untuk navigasi.

**19.3.3** Table of Contents otomatis.

**19.3.4** Editable: Pengguna dapat mengedit setelah ekspor.

**19.3.5** Support Track Changes untuk kolaborasi.

**19.3.6** Page numbering dan header/footer.

**19.4 Template Kustomisasi**

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

**19.5 Opsi Export**

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

**19.6 Implementasi Teknis**

**19.6.1** PDF generation menggunakan printpdf atau genpdf crate di Rust.

**19.6.2** DOCX generation menggunakan docx-rs crate di Rust.

**19.6.3** File disimpan sementara di `{data_dir}/exports/{user_id}/` dan dihapus setelah 24 jam.

**19.6.4** Download URL memiliki expiry time 1 jam.

---

## 20. STANDAR KETERLACAKAN

**20.1** Setiap kesimpulan material harus mempunyai struktur: Kesimpulan → Alasan → Kaidah hukum → Sumber → Fakta → Sumber fakta → Bukti → Kontraargumentasi.

**20.2** Setiap edge dalam traceability_edges harus mencantumkan:
- `source_type`: 'knowledge_base', 'web', 'document', atau 'user_statement'
- `source_url`: URL sumber (untuk web)
- `rule_id`: ID entri di Knowledge Base atau legal_rules
- `fact_id`: ID fakta terkait

**20.3** Jika salah satu elemen tidak tersedia, sistem harus menandainya secara eksplisit dengan penanda "KETERLACAKAN TIDAK LENGKAP" beserta elemen yang hilang dan rekomendasi.

**20.4 Checklist Validasi Wajib:**
- Setiap kesimpulan memiliki minimal 1 alasan
- Setiap alasan memiliki minimal 1 kaidah hukum
- Setiap kaidah memiliki sumber yang valid
- Setiap sumber memiliki status keberlakuan
- Setiap kaidah terhubung ke minimal 1 fakta
- Setiap fakta memiliki status verifikasi
- Setiap kesimpulan memiliki kontraargumentasi

---

## 21. STANDAR BAHASA

**21.1** PAUGERAN harus menggunakan Bahasa Indonesia yang baku untuk analisis hukum.

**21.2** PAUGERAN harus menggunakan istilah hukum yang benar sesuai KUHP, KUHPerdata, dan UU terkait.

**21.3** PAUGERAN harus menjelaskan istilah teknis ketika pertama kali digunakan.

**21.4** PAUGERAN harus menghindari jargon yang tidak perlu.

**21.5** PAUGERAN harus membedakan dengan tegas antara fakta, hukum, interpretasi, dan opini.

**21.6** PAUGERAN tidak boleh menggunakan bahasa yang terlalu informal atau sengaja sulit dipahami.

**21.7** Standar gaya: Bahasa ahli hukum yang dapat dipahami orang awam.

**21.8** Istilah hukum baku yang wajib digunakan: Wanprestasi (bukan ingkar janji), Perjanjian (bukan kesepakatan untuk konteks formal), Para pihak (bukan mereka), Gugatan (bukan tuntutan untuk perdata), Tergugat/Penggugat, Eksepsi (bukan keberatan), Rekonvensi (bukan gugatan balik).

**21.9** Format penulisan peraturan: "Pasal 1338 Kitab Undang-Undang Hukum Perdata", "Undang-Undang Nomor 5 Tahun 1986".

**21.10** Format penulisan putusan: "Putusan Mahkamah Agung Nomor 123 K/Pdt/2020".

**21.11** Format tanggal default: "28 Agustus 2026".

**21.12** Standar bahasa ini berlaku untuk analisis hukum. Antarmuka bisa dalam Bahasa Indonesia atau English sesuai preferensi pengguna.

---

## 22. KONTRAK PERILAKU

**22.1 Formula Perilaku Inti**

PAUGERAN harus berperilaku menurut formula: PAHAM → TANYA → KONFIRMASI → RUMUSKAN → TELITI → VERIFIKASI → NALAR → BANTAH → UJI → SIMPULKAN → TELUSURI.

PAUGERAN tidak boleh berperilaku menurut formula: PERTANYAAN → CARI PASAL → JAWAB.

**22.2 Karakter Perilaku**

**22.2.1** PAUGERAN adalah: Sabar, Teliti, Jujur, Berimbang, Transparan, Dapat Dipertanggungjawabkan, Hormat terhadap preferensi pengguna, Cepat melalui Rust, Aman secara memory-safe.

**22.2.2** PAUGERAN bukan: Otoriter, Spekulatif, Bias, Hitam-putih, Tertutup, Intrusif, Lambat.

**22.3 Respons terhadap Situasi**

**22.3.1** Jika informasi tidak lengkap: PAUGERAN harus menyatakan informasi apa yang masih kurang dan mengapa informasi tersebut diperlukan.

**22.3.2** Jika norma konflik: PAUGERAN harus menampilkan konflik, menjelaskan kedua norma, dan menjelaskan bagaimana konflik diselesaikan dalam praktik.

**22.3.3** Jika kepastian rendah: PAUGERAN harus menyatakan tingkat kepastian, alasan ketidakpastian, dan kondisi yang dapat mengubah kesimpulan.

**22.3.4** Jika tidak ada sumber: PAUGERAN harus menyatakan bahwa sumber tidak ditemukan, menjelaskan kemungkinan penyebab, dan menyarankan pendekatan alternatif.

**22.3.5** Jika API key tidak dikonfigurasi: PAUGERAN harus meminta pengguna memasukkan API key melalui halaman Pengaturan.

**22.3.6** Jika lisensi tidak valid: PAUGERAN harus menyatakan bahwa lisensi tidak valid dan meminta pengguna mengaktivasi lisensi melalui halaman Pengaturan.

**22.3.7** Jika eksekusi dibatalkan: PAUGERAN harus menyatakan bahwa analisis dibatalkan dan data yang sudah terkumpul tetap tersimpan.

**22.3.8** Jika situs web tidak dapat diakses: PAUGERAN harus menyatakan situs mana yang tidak dapat diakses dan melanjutkan dengan sumber alternatif.

**22.3.9** Jika provider LLM gagal: PAUGERAN harus mencoba provider fallback dan menyatakan kepada pengguna jika semua provider gagal.

---

# BAGIAN IV — PRODUK & ANTARMUKA

---

## 23. SPESIFIKASI INSTALASI & DISTRIBUSI

**23.1** PAUGERAN didistribusikan sebagai satu binary universal yang dapat dijalankan di berbagai environment tanpa modifikasi kode.

**23.2 Binary Standalone**

**23.2.1** Linux: `paugeran-linux` untuk x86_64.

**23.2.2** macOS: `paugeran-macos` universal untuk Intel dan Apple Silicon.

**23.2.3** Windows: `paugeran-windows.exe` untuk x86_64.

**23.3 Docker**

**23.3.1** Image: `ghcr.io/paugeran/paugeran:latest`

**23.3.2** Perintah: `docker run -d -p 3000:3000 -v paugeran_data:/data ghcr.io/paugeran/paugeran`

**23.4 Railway**

**23.4.1** One-click deploy melalui tombol "Deploy to Railway" di repository.

**23.4.2** Volume `/data` wajib ditambahkan untuk persistent storage.

**23.5 VPS**

**23.5.1** Quick install script: `curl -fsSL https://get.paugeran.com/install.sh | bash`

**23.5.2** Docker Compose dengan Caddy sebagai reverse proxy untuk auto-SSL.

**23.6 Homelab**

**23.6.1** Docker run dengan akses via Tailscale atau Cloudflare Tunnel.

**23.7 Tauri Desktop (Opsional)**

**23.7.1** Installer: `.msi` untuk Windows, `.dmg` untuk macOS, `.appimage` untuk Linux.

**23.7.2** Tauri menjalankan binary PAUGERAN sebagai sidecar dan membuka WebView ke localhost.

**23.8 Environment Variables**

**23.8.1** `AUTH_ENABLED`: Default false. Set true untuk mengaktifkan autentikasi multi-user.

**23.8.2** `JWT_SECRET`: Wajib jika AUTH_ENABLED=true. Random string 256-bit.

**23.8.3** `DATABASE_URL`: Default `sqlite:///data/paugeran.db`. Bisa diganti ke PostgreSQL.

**23.8.4** `PORT`: Default 3000.

**23.8.5** `DATA_DIR`: Default `/data`.

**23.8.6** `CORS_ORIGINS`: Default `http://localhost:3000`.

**23.8.7** `LOG_LEVEL`: Default `info`.

**23.8.8** `MAX_UPLOAD_SIZE`: Default 10485760 (10MB).

**23.8.9** `LICENSE_KEY`: Lisensi key untuk aktivasi otomatis.

**23.8.10** `LICENSE_SERVER_URL`: URL server lisensi (default: `https://license.paugeran.com`).

**23.8.11** `LICENSE_CHECK_INTERVAL`: Interval validasi dalam jam (default: 24).

**23.8.12** `LICENSE_GRACE_PERIOD`: Grace period dalam hari (default: 7).

**23.9 Data Location**

**23.9.1** Binary mode: Linux `~/.local/share/paugeran/`, macOS `~/Library/Application Support/paugeran/`, Windows `%APPDATA%\paugeran\`.

**23.9.2** Docker mode: Volume `paugeran_data` mounted ke `/data`.

---

## 24. SPESIFIKASI API KEY MANAGEMENT

**24.1** API key LLM adalah milik pengguna. PAUGERAN hanya menyimpan, menggunakan, dan menghapusnya sesuai instruksi pengguna. API key tidak pernah dikirim ke server PAUGERAN atau pihak ketiga manapun.

**24.2** API key disimpan terenkripsi dengan AES-256-GCM di database.

**24.3** Kunci enkripsi disimpan di `{data_dir}/.secret` dengan permission 600 di Unix.

**24.4** Kunci enkripsi di-generate otomatis saat pertama kali aplikasi dijalankan.

**24.5** Frontend tidak pernah menerima plain text API key. Frontend hanya menerima masked key untuk display.

**24.6** Setup wizard muncul saat pertama kali diakses ketika belum ada lisensi dan provider. Minimal satu lisensi dan satu provider harus dikonfigurasi.

**24.7** Fallback API key hierarchy: API key pribadi user → API key global admin → error.

**24.8** Konfigurasi provider mendukung berbagai penyedia dan model, tidak terbatas pada model ternama.

---

## 25. SPESIFIKASI AUTENTIKASI & OTORISASI

**25.1** Autentikasi adalah fitur opsional yang diaktifkan melalui `AUTH_ENABLED=true`.

**25.2** Ketika `AUTH_ENABLED=false`: Tidak ada halaman login. Semua data milik local user tunggal.

**25.3** Ketika `AUTH_ENABLED=true`: Halaman login dan register aktif. JWT token digunakan untuk session.

**25.4** User pertama yang mendaftar saat `AUTH_ENABLED=true` otomatis mendapatkan peran admin.

**25.5** User kedua dan seterusnya wajib menyertakan invite_code yang valid saat registrasi.

**25.6 Role Admin**

**25.6.1** Admin memiliki semua hak user biasa.

**25.6.2** Admin dapat mengelola user: invite, deactivate, promote, demote, delete.

**25.6.3** Admin dapat melihat statistik usage tim.

**25.6.4** Admin dapat menyediakan global API keys dan global LLM providers.

**25.6.5** Admin dapat mengelola Legal Knowledge Base global.

**25.6.6** Admin dapat mengonfigurasi sistem termasuk whitelist domain penelitian web.

**25.6.7** Admin dapat mengelola lisensi tim.

**25.7 Role User**

**25.7.1** User dapat mengelola sesi obrolan sendiri.

**25.7.2** User dapat mengelola LLM providers sendiri.

**25.7.3** User dapat mengustomisasi UI sendiri.

**25.7.4** User dapat mengekspor laporan sendiri.

**25.7.5** User dapat mengelola Legal Knowledge Base pribadi (opsional).

**25.8 Proteksi Admin**

**25.8.1** Admin tidak bisa menurunkan diri sendiri dari role admin.

**25.8.2** Admin terakhir di sistem tidak bisa diturunkan atau dihapus.

**25.8.3** Admin tidak bisa melihat API key pribadi user lain.

**25.8.4** Admin tidak bisa melihat isi sesi obrolan user lain.

**25.8.5** Admin tidak bisa melihat data pribadi user lain.

**25.8.6** Semua aksi admin dicatat di admin_audit_logs.

**25.9 Invitation System**

**25.9.1** Admin dapat membuat undangan via email atau link.

**25.9.2** Undangan memiliki expiry date dan maximum usage.

**25.9.3** Undangan yang sudah dipakai ditandai dengan used_by dan used_at.

---

## 26. SPESIFIKASI MANAJEMEN SESI

**26.1** Setiap sesi obrolan adalah entitas independen yang berdiri sendiri. Sesi A tidak mengetahui keberadaan sesi B.

**26.2** Data yang bersifat global hanya API key, preferensi UI, Legal Knowledge Base, dan lisensi.

**26.3** Sesi baru dapat dibuat kapan saja melalui tombol "+ Sesi Baru", shortcut Ctrl+N, command palette, atau otomatis saat mengirim pesan pertama di halaman kosong.

**26.4** Sidebar menampilkan daftar sesi dikelompokkan berdasarkan waktu: Hari Ini, Kemarin, 7 Hari Terakhir, 30 Hari Terakhir, Lebih Lama.

**26.5** Sesi lama dapat dibuka kembali kapan saja tanpa batas waktu.

**26.6** Sesi dapat dihapus permanen kapan saja dengan konfirmasi dialog.

**26.7** Penghapusan sesi menghapus semua pesan, fakta, dokumen, peta keterlacakan, dan file fisik terkait. Penghapusan tidak mempengaruhi API key, preferensi UI, Legal Knowledge Base, atau sesi lain.

**26.8** Query database WAJIB filter by session_id. File storage menggunakan path per session. Tidak ada endpoint yang bisa mengakses data sesi lain.

**26.9** Siklus hidup sesi: CREATED → ACTIVE → COMPLETED → ARCHIVED → DELETED.

**26.10** Tidak ada batasan jumlah sesi, jumlah pesan per sesi, atau usia sesi. Batasan teknis: ukuran file max 10MB, jumlah file per sesi max 50.

**26.11** Setiap sesi dapat memiliki model LLM yang dipilih sendiri melalui dropdown di header sesi.

---

## 27. SPESIFIKASI KUSTOMISASI ANTARMUKA

**27.1** Antarmuka adalah milik pengguna. Pengguna harus dapat menyesuaikan tampilan sesuai preferensi visual dan ergonomi mereka.

**27.2 Opsi Kustomisasi**

**27.2.1** Tema Warna: Light (default), Dark, System, Sepia, High Contrast.

**27.2.2** Ukuran Font: Small (14px), Medium (16px, default), Large (18px), Extra Large (20px).

**27.2.3** Lebar Konten: Narrow (700px), Medium (900px, default), Wide (1200px), Full (100%).

**27.2.4** Posisi Sidebar: Kiri (default), Kanan, Tersembunyi.

**27.2.5** Bahasa Antarmuka: Bahasa Indonesia (default), English.

**27.2.6** Font Family: Sans-serif (default), Serif, Monospace.

**27.2.7** Animasi: Full (default), Reduced, None.

**27.2.8** Format Tanggal: Indonesia formal (default), Pendek, English, ISO.

**27.2.9** Notifikasi Suara: Aktif (default), Senyap.

**27.2.10** Density: Comfortable (default), Compact, Spacious.

**27.3** Preferensi disimpan di tabel `ui_preferences` sebagai JSON.

**27.4** Preferensi di-load saat aplikasi start dan diaplikasikan secara instan saat diubah.

**27.5** Tombol "Reset ke Default" tersedia di bagian bawah settings.

---

## 28. SPESIFIKASI AKSESIBILITAS

**28.1** PAUGERAN harus dapat digunakan oleh pengguna dengan berbagai kemampuan. Aksesibilitas adalah standar, bukan fitur tambahan.

**28.2 Keyboard Navigation**

**28.2.1** Semua interaksi dapat dilakukan melalui keyboard.

**28.2.2** Focus order harus logis dan dapat diprediksi.

**28.2.3** Focus indicator harus terlihat jelas.

**28.2.4** Tab, Shift+Tab, Enter, Space, Arrow keys, Escape harus berfungsi sesuai konvensi.

**28.3 Keyboard Shortcuts Lengkap**

**28.3.1** Navigasi:
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

**28.3.2** Dalam chat:
- `Enter`: Kirim pesan
- `Shift+Enter`: Baris baru
- `Ctrl+↑`: Edit pesan terakhir
- `Ctrl+↓`: Batal edit
- `Ctrl+C`: Salin pesan terpilih
- `Ctrl+Shift+C`: Salin dengan citation

**28.3.3** Dalam dokumen/laporan:
- `Ctrl+F`: Cari dalam dokumen
- `Ctrl+G`: Cari berikutnya
- `Ctrl+Shift+G`: Cari sebelumnya
- `Ctrl+P`: Print / Export
- `Ctrl+S`: Simpan perubahan

**28.4 Command Palette**

**28.4.1** Command palette (Ctrl+K) adalah cara cepat untuk mengakses semua fitur tanpa navigasi menu.

**28.4.2** Command palette mendukung:
- Pencarian fuzzy untuk semua perintah
- Recent commands
- Keyboard navigation
- Preview untuk beberapa perintah

**28.4.3** Perintah yang tersedia di command palette:
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

**28.5 Screen Reader Support**

**28.5.1** Semua elemen interaktif memiliki ARIA labels yang sesuai.

**28.5.2** Perubahan status diumumkan melalui ARIA live regions.

**28.5.3** Streaming teks di chat diumumkan secara bertahap.

**28.5.4** Error dan notifikasi diumumkan dengan jelas.

**28.6 Mode Aksesibilitas**

**28.6.1** High Contrast Mode: Kontras warna ditingkatkan untuk pengguna dengan gangguan penglihatan.

**28.6.2** Reduced Motion Mode: Animasi diminimalkan untuk pengguna yang sensitif terhadap gerakan.

**28.6.3** Large Text Mode: Font diperbesar secara default.

**28.6.4** Screen Reader Optimized Mode: Layout dioptimalkan untuk screen reader.

**28.7 Warna dan Kontras**

**28.7.1** Semua teks harus memiliki rasio kontras minimal 4.5:1 terhadap latar belakang (WCAG 2.1 AA).

**28.7.2** Teks besar (18pt atau 14pt bold) harus memiliki rasio kontras minimal 3:1.

**28.7.3** Informasi tidak boleh disampaikan hanya melalui warna.

**28.8 Fokus dan Interaksi**

**28.8.1** Focus indicator harus memiliki rasio kontras minimal 3:1.

**28.8.2** Target klik minimal 44x44 piksel.

**28.8.3** Hover states harus jelas terlihat.

**28.9 Toast Notifications**

**28.9.1** Notifikasi non-modal untuk aksi yang berhasil.

**28.9.2** Toast otomatis hilang setelah 5 detik.

**28.9.3** Toast dapat ditutup manual.

**28.9.4** Toast tidak mengganggu fokus keyboard.

**28.10 Breadcrumb Navigation**

**28.10.1** Breadcrumb ditampilkan di atas konten untuk navigasi yang jelas.

**28.10.2** Contoh: `Beranda > Sesi > Sengketa Tanah > Laporan`

**28.10.3** Setiap item breadcrumb dapat diklik.

**28.11 Quick Actions**

**28.11.1** Tombol aksi cepat di header sesi:
- 📊 Peta Keterlacakan
- 📄 Laporan
- 📚 Simpan ke Knowledge Base
- 📥 Export
- ⋯ Menu lainnya

**28.11.2** Setiap quick action memiliki tooltip dan keyboard shortcut.

**28.12 Undo/Redo**

**28.12.1** Aksi yang dapat di-undo:
- Edit judul sesi
- Edit preferensi
- Hapus pesan (dalam batas waktu)
- Hapus dokumen (dalam batas waktu)

**28.12.2** Keyboard shortcut: `Ctrl+Z` untuk undo, `Ctrl+Shift+Z` atau `Ctrl+Y` untuk redo.

---

## 29. SPESIFIKASI ANTARMUKA

**29.1 Layout Utama**

**29.1.1** Header global: Logo PAUGERAN, breadcrumb, tombol pencarian (command palette), tombol quick theme toggle, tombol notifikasi, tombol pengaturan.

**29.1.2** Sidebar kiri: Tombol "+ Sesi Baru", daftar sesi dikelompokkan berdasarkan waktu, status lisensi di bagian bawah.

**29.1.3** Area chat utama: Header sesi dengan judul editable, indikator fase, dropdown model, quick actions, area pesan, input area.

**29.2 Komponen Chat**

**29.2.1** Pesan user: aligned right dengan warna aksen.

**29.2.2** Pesan PAUGERAN: aligned left dengan warna netral, indikator fase di header, inline citation yang dapat di-hover.

**29.2.3** Streaming text dengan efek typing.

**29.2.4** Tombol aksi kontekstual berdasarkan fase.

**29.2.5** Input area: textarea multi-line auto-resize, tombol upload dokumen, tombol pilih model, tombol kirim.

**29.3 Panel Samping**

**29.3.1** Slide dari kanan.

**29.3.2** Mode Peta Keterlacakan: graf interaktif dengan zoom, pan, click untuk detail, export PNG/SVG.

**29.3.3** Mode Laporan: 24 poin dengan collapsible sections dan tombol export.

**29.3.4** Mode Log Reasoning: Detail proses penalaran dengan sumber.

**29.4 Panel Pengaturan**

**29.4.1** Akses dari tombol ⚙️ atau shortcut Ctrl+,.

**29.4.2** Kategori:
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

**29.5 Setup Wizard**

**29.5.1** Tampil otomatis saat pertama kali dijalankan.

**29.5.2** Langkah-langkah:
1. Input lisensi key
2. Konfigurasi minimal satu LLM provider
3. Pilih preferensi dasar (tema, bahasa)
4. Selesai

**29.6 Halaman Aktivasi Lisensi**

**29.6.1** Tampil saat lisensi tidak valid atau belum diaktivasi.

**29.6.2** Input lisensi key dengan tombol "Aktivasi".

**29.6.3** Status validasi ditampilkan secara real-time.

**29.6.4** Link ke halaman pembelian lisensi.

**29.7 Halaman Legal Knowledge Base**

**29.7.1** Daftar entri dengan filter (jenis, tahun, status) dan pencarian.

**29.7.2** Tombol "Tambah" untuk menambah entri baru.

**29.7.3** Tombol "Import" untuk bulk import.

**29.7.4** Setiap entri memiliki menu aksi: Lihat, Edit, Refresh, Hapus.

**29.8 Command Palette**

**29.8.1** Modal di tengah layar dengan input pencarian.

**29.8.2** Hasil pencarian ditampilkan secara real-time.

**29.8.3** Keyboard navigation dengan arrow keys.

**29.8.4** Preview untuk beberapa perintah.

**29.9 Responsivitas**

**29.9.1** Desktop (>1024px): Sidebar dan chat area berdampingan.

**29.9.2** Tablet (768-1024px): Sidebar collapsible.

**29.9.3** Mobile (<768px): Sidebar sebagai drawer overlay, input area fixed di bawah.

**29.10 Status Bar**

**29.10.1** Status bar di bagian bawah menampilkan:
- Status lisensi (ikon + tooltip)
- Status koneksi internet
- Provider LLM aktif
- Jumlah sesi

**29.10.2** Status bar dapat di-klik untuk detail.

---

# BAGIAN V — KEAMANAN & OPERASIONAL

---

## 30. SPESIFIKASI KEAMANAN

**30.1** API key dienkripsi dengan AES-256-GCM. Kunci enkripsi disimpan dengan permission 600.

**30.2** Password di-hash dengan Argon2.

**30.3** JWT token menggunakan algoritma HS256. Access token expiry 15 menit. Refresh token expiry 24 jam.

**30.4** Lisensi key dienkripsi dengan AES-256-GCM.

**30.5** PII di-redact sebelum dikirim ke LLM API. Mapping untuk de-anonymize setelah respons diterima. Plain text PII tidak di-log.

**30.6** Setiap query database WAJIB filter by session_id untuk isolasi sesi.

**30.7** Admin tidak bisa melihat data pribadi atau API key user lain.

**30.8** Semua aksi kritis dicatat di audit_logs.

**30.9** Tidak ada unsafe code Rust tanpa justifikasi yang jelas.

**30.10** Tidak ada blocking I/O di async context.

**30.11** Semua dependencies diaudit dengan `cargo audit`.

**30.12** Tidak ada `unwrap()` di production code.

**30.13** SQL injection dicegah melalui parameterized queries via SQLx.

**30.14** XSS dicegah melalui input sanitization.

**30.15** Penelitian web hanya ke domain whitelist.

**30.16** Request ke server lisensi menggunakan HTTPS dengan payload yang di-sign.

**30.17** Server lisensi tidak menerima data pengguna, API key LLM, atau konten sesi.

---

## 31. SPESIFIKASI DEPLOYMENT

**31.1** Binary PAUGERAN selalu berjalan sebagai HTTP server. Perbedaan deployment hanya pada cara binary dijalankan dan diakses.

**31.2** Untuk deployment cloud atau VPS, reverse proxy dengan SSL wajib digunakan (Caddy atau Nginx).

**31.3** Port tidak boleh di-expose langsung ke internet tanpa SSL.

**31.4** Docker image menggunakan multi-stage build: builder Rust, builder Node.js untuk frontend, dan runtime debian-slim.

**31.5** Docker image berjalan sebagai non-root user.

**31.6** Health check endpoint `GET /health` wajib tersedia.

**31.7** Volume persistent wajib digunakan untuk `/data` di semua deployment Docker.

**31.8** Untuk deployment air-gapped, lisensi offline harus dikonfigurasi.

---

## 32. MONITORING & OBSERVABILITAS

**32.1** Logging menggunakan tracing crate dengan format JSON structured.

**32.2** Log levels: DEBUG (development only), INFO, WARNING, ERROR, CRITICAL.

**32.3** API key tidak boleh di-log dalam bentuk apapun.

**32.4** PII tidak boleh di-log tanpa redaksi.

**32.5** Lisensi key tidak boleh di-log dalam bentuk apapun.

**32.6** Health check endpoint mengembalikan status, version, database status, auth_enabled, license_valid, timestamp.

**32.7** Jika LangSmith API key dikonfigurasi, semua LLM calls di-trace untuk observabilitas.

**32.8** Metrik yang dimonitor:
- Request rate per endpoint
- Response time per endpoint
- LLM API call count dan latency
- Knowledge Base hit rate
- Web research success rate
- License validation status
- Error rate per kategori

---

## 33. BACKUP & RECOVERY

**33.1** Backup mencakup: database, encryption key, dokumen pengguna, laporan yang diekspor, Legal Knowledge Base, dan log files.

**33.2** Lisensi key ikut ter-backup sebagai bagian dari database.

**33.3** Binary mode: Backup dilakukan dengan mengarsipkan data directory.

**33.4** Docker mode: Backup dilakukan dengan menjalankan container alpine yang mengarsipkan volume.

**33.5** Recovery dilakukan dengan mengekstrak arsip ke data directory dan restart aplikasi.

**33.6** SQLite schema backward compatible melalui migrations.

**33.7** Legal Knowledge Base dapat di-export terpisah sebagai file JSON untuk migrasi.

---

# BAGIAN VI — KEPATUHAN & PENERIMAAN

---

## 34. LARANGAN PRODUK

PAUGERAN tidak boleh:

**34.1** Mengarang pasal, nomor peraturan, atau tahun peraturan.

**34.2** Mengarang putusan, nomor perkara, atau tanggal putusan.

**34.3** Mengarang sumber hukum yang tidak ada.

**34.4** Menyatakan sumber masih berlaku tanpa pemeriksaan yang memadai.

**34.5** Menyatakan fakta pengguna sebagai fakta terbukti tanpa dasar verifikasi.

**34.6** Menyembunyikan ketidakpastian atau ambiguitas hukum.

**34.7** Hanya mencari sumber yang mendukung kesimpulan.

**34.8** Menghapus atau mengabaikan argumen pihak lawan.

**34.9** Memberikan kepastian yang tidak didukung data.

**34.10** Mengklaim telah melakukan penelitian yang sebenarnya tidak dilakukan.

**34.11** Mengklaim telah memeriksa dokumen yang tidak tersedia.

**34.12** Mengubah fakta agar cocok dengan norma.

**34.13** Memberikan kesimpulan hanya berdasarkan kemiripan kata kunci.

**34.14** Mengirim API key pengguna ke server PAUGERAN atau pihak ketiga.

**34.15** Menyimpan API key dalam plain text di database atau log.

**34.16** Mengakses data dari sesi obrolan lain.

**34.17** Membocorkan data sesi ke sesi lain.

**34.18** Menampilkan error message yang mengekspos API key, lisensi key, atau data sensitif.

**34.19** Memproses data setelah sesi dihapus.

**34.20** Mengakses situs di luar daftar putih untuk penelitian.

**34.21** Memberikan jawaban tanpa melalui siklus pemahaman.

**34.22** Melewatkan fase kontraargumentasi.

**34.23** Menggunakan data pengguna untuk melatih model AI.

**34.24** Mewajibkan login atau autentikasi untuk penggunaan pribadi.

**34.25** Menghapus data sesi tanpa konfirmasi eksplisit dari pengguna.

**34.26** Mengubah preferensi UI tanpa persetujuan pengguna.

**34.27** Mengirim data preferensi ke pihak ketiga.

**34.28** Menggunakan unsafe code tanpa justifikasi yang jelas.

**34.29** Menyimpan plain text API key di memori lebih dari yang diperlukan.

**34.30** Melakukan blocking I/O di async context.

**34.31** Admin melihat data pribadi atau API key user lain.

**34.32** Admin menurunkan diri sendiri dari role admin.

**34.33** Menghapus admin terakhir di sistem.

**34.34** Mengunci pengguna dari data mereka sendiri saat lisensi tidak valid (data harus tetap dapat diakses untuk backup/migrasi).

**34.35** Mengirim lisensi key ke server lisensi dalam plain text.

**34.36** Mengirim data pengguna, API key LLM, atau konten sesi ke server lisensi.

**34.37** Mengunci pengguna tanpa grace period saat validasi lisensi gagal karena masalah jaringan.

**34.38** Mengklaim klaim hukum tanpa inline citation yang detail.

**34.39** Menyimpan dokumen pengguna (non-hukum) ke Legal Knowledge Base.

**34.40** Mengunci pengguna pada satu penyedia atau model LLM tertentu.

---

## 35. KRITERIA KEBERHASILAN

**35.1** Sebuah analisis dianggap berhasil apabila pengguna dapat menjawab lima pertanyaan berikut hanya dengan membaca output PAUGERAN:

**35.1.1** Apa sebenarnya masalah hukum saya?

**35.1.2** Hukum apa yang mengatur masalah tersebut?

**35.1.3** Bagaimana hukum tersebut diterapkan terhadap fakta saya?

**35.1.4** Apa alasan yang mendukung dan menentang kesimpulan tersebut?

**35.1.5** Mengapa PAUGERAN sampai pada kesimpulan akhirnya dan apa yang dapat membuat kesimpulan itu berubah?

**35.2** Jika kelima pertanyaan tersebut tidak dapat dijawab dari output, maka produk belum memenuhi standar PAUGERAN.

**35.3 Metrik Kualitas**

**35.3.1** Akurasi citation: lebih dari 95%.

**35.3.2** Kelengkapan keterlacakan: 100%.

**35.3.3** Keberimbangan: 100% selalu ada kontraargumentasi.

**35.3.4** Waktu respons chat: kurang dari 2 detik streaming.

**35.3.5** Waktu generasi laporan: kurang dari 5 menit.

**35.3.6** Startup time: kurang dari 5 detik.

**35.3.7** Memory usage: kurang dari 500 MB typical.

**35.3.8** Binary size: kurang dari 30 MB.

**35.3.9** Inline citation coverage: 100% klaim hukum memiliki citation.

**35.3.10** Knowledge Base hit rate: minimal 30% untuk pertanyaan umum setelah penggunaan 1 bulan.

**35.3.11** Web research success rate: lebih dari 90% untuk domain whitelist.

**35.3.12** License validation success rate: lebih dari 99% saat online.

**35.3.13** Export PDF/DOCX quality: siap pakai tanpa formatting ulang.

---

## 36. KRITERIA PENERIMAAN

Produk dinyatakan ready dan siap digunakan apabila memenuhi SEMUA kriteria berikut:

**36.1 Kriteria Fungsional**

**36.1.1** Pengguna dapat menjalankan dengan satu perintah.

**36.1.2** Setup wizard muncul saat pertama kali dijalankan.

**36.1.3** Pengguna dapat mengaktivasi lisensi melalui antarmuka web.

**36.1.4** Lisensi divalidasi secara berkala tanpa mengganggu pengguna.

**36.1.5** Grace period berfungsi saat offline.

**36.1.6** Pengguna dapat mengonfigurasi berbagai LLM providers (tidak hanya model ternama).

**36.1.7** Pengguna dapat memasukkan API key melalui antarmuka web.

**36.1.8** Pengguna dapat mengubah API key dan provider kapan saja.

**36.1.9** Auth opsional berdasarkan AUTH_ENABLED.

**36.1.10** User pertama otomatis jadi admin jika AUTH_ENABLED=true.

**36.1.11** Admin dapat invite, kelola, dan monitor user.

**36.1.12** Admin tidak bisa lihat data atau API key user lain.

**36.1.13** Pengguna dapat membuat sesi obrolan baru kapan saja.

**36.1.14** Sesi lama dapat dibuka kembali kapan saja tanpa batas waktu.

**36.1.15** Sesi dapat dihapus permanen dengan konfirmasi.

**36.1.16** Setiap sesi terisolasi dari sesi lain.

**36.1.17** PAUGERAN melakukan wawancara adaptif.

**36.1.18** Pengguna dapat mengunggah dokumen PDF, DOCX, TXT.

**36.1.19** PAUGERAN melakukan penelitian web ke situs whitelist.

**36.1.20** PAUGERAN menyimpan peraturan yang diteliti ke Legal Knowledge Base.

**36.1.21** PAUGERAN menggunakan Legal Knowledge Base untuk analisis berikutnya.

**36.1.22** Setiap klaim hukum memiliki inline citation yang detail.

**36.1.23** Log reasoning dapat diakses dan menampilkan sumber detail.

**36.1.24** PAUGERAN menghasilkan laporan 24 poin.

**36.1.25** PAUGERAN menampilkan peta keterlacakan interaktif.

**36.1.26** Pengguna dapat mengekspor laporan ke PDF profesional dan DOCX profesional.

**36.1.27** Template export dapat dikustomisasi.

**36.1.28** Pengguna dapat mengustomisasi antarmuka.

**36.1.29** Preferensi UI tersimpan persisten.

**36.1.30** Command palette berfungsi dengan semua perintah.

**36.1.31** Keyboard shortcuts lengkap berfungsi.

**36.1.32** Screen reader support berfungsi.

**36.1.33** Mode aksesibilitas (High Contrast, Reduced Motion) berfungsi.

**36.1.34** Custom Graph Engine berjalan dengan benar untuk semua 11 fase.

**36.1.35** Streaming SSE berfungsi dengan baik.

**36.1.36** Cancellation support berfungsi.

**36.1.37** Multi-provider LLM berfungsi dengan fallback.

**36.2 Kriteria Non-Fungsional**

**36.2.1** Respons chat kurang dari 2 detik.

**36.2.2** Laporan lengkap kurang dari 5 menit.

**36.2.3** Binary size kurang dari 30 MB.

**36.2.4** Startup time kurang dari 5 detik.

**36.2.5** Memory usage kurang dari 500 MB.

**36.2.6** Data terenkripsi at rest.

**36.2.7** PII diredaksi sebelum dikirim ke API model.

**36.2.8** Semua klaim hukum memiliki citation yang valid.

**36.2.9** API key dan lisensi key tidak pernah di-log dalam plain text.

**36.2.10** UI responsif di desktop, tablet, dan mobile.

**36.2.11** Tidak ada memory leaks.

**36.2.12** Tidak ada data races.

**36.2.13** WCAG 2.1 AA compliance untuk aksesibilitas.

**36.3 Kriteria Kepatuhan PRD**

**36.3.1** Tidak ada halusinasi pasal atau putusan dalam 100 uji kasus.

**36.3.2** Semua kesimpulan memiliki peta keterlacakan lengkap.

**36.3.3** Kontraargumentasi selalu ditampilkan.

**36.3.4** Ketidakpastian dinyatakan secara eksplisit.

**36.3.5** Bahasa analisis sesuai standar profesional.

**36.3.6** Isolasi sesi terjamin melalui uji penetrasi.

**36.3.7** Audit log mencatat semua aksi kritis.

**36.3.8** Penelitian web hanya ke domain whitelist.

**36.3.9** Inline citation detail di semua output.

**36.3.10** Legal Knowledge Base dapat dibangun dari waktu ke waktu.

**36.3.11** Lisensi validasi bekerja dengan grace period.

**36.4 Kriteria Rust**

**36.4.1** Semua kode compile tanpa warnings.

**36.4.2** cargo clippy clean.

**36.4.3** cargo fmt applied.

**36.4.4** cargo audit clean.

**36.4.5** Test coverage lebih dari 80%.

**36.4.6** Documentation coverage lebih dari 70%.

**36.4.7** No unwrap() in production code.

**36.5 Kriteria Deployment**

**36.5.1** Binary dapat dijalankan di Windows, macOS, Linux.

**36.5.2** Docker image build berhasil.

**36.5.3** Dapat di-deploy ke Railway.

**36.5.4** Dapat di-deploy ke VPS dengan Caddy.

**36.5.5** Dapat di-deploy ke homelab.

**36.5.6** Volume persistence berfungsi.

**36.5.7** Health check endpoint berfungsi.

**36.5.8** Backup dan restore teruji.

**36.5.9** Deployment air-gapped dengan lisensi offline berfungsi.

**36.6 Kriteria Keamanan**

**36.6.1** API key terenkripsi dengan AES-256-GCM.

**36.6.2** Lisensi key terenkripsi dengan AES-256-GCM.

**36.6.3** Encryption key disimpan dengan permission 600.

**36.6.4** Session isolation verified.

**36.6.5** OWASP Top 10 vulnerabilities tested dan fixed.

**36.6.6** SQL injection prevented.

**36.6.7** XSS prevented.

**36.6.8** Admin tidak bisa akses data user lain.

**36.6.9** Penelitian web hanya ke whitelist.

**36.6.10** Server lisensi tidak menerima data sensitif.

**36.7 Kriteria Kustomisasi & Aksesibilitas**

**36.7.1** Minimal 5 tema warna tersedia.

**36.7.2** Minimal 4 ukuran font tersedia.

**36.7.3** Minimal 2 bahasa UI tersedia.

**36.7.4** Preferensi tersimpan di database.

**36.7.5** Perubahan preferensi berlaku instan.

**36.7.6** Reset ke default berfungsi.

**36.7.7** Command palette mencakup semua perintah.

**36.7.8** Keyboard shortcuts lengkap dan berfungsi.

**36.7.9** Screen reader dapat mengakses semua fitur.

**36.7.10** Mode aksesibilitas berfungsi dengan baik.

**36.8 Kriteria Legal Knowledge Base**

**36.8.1** Peraturan dari web dapat disimpan ke Knowledge Base.

**36.8.2** Pencarian semantik di Knowledge Base berfungsi.

**36.8.3** Knowledge Base digunakan otomatis dalam analisis.

**36.8.4** Admin dapat mengelola Knowledge Base global.

**36.8.5** User dapat mengelola Knowledge Base pribadi.

**36.8.6** Bulk import berfungsi.

**36.8.7** Status keberlakuan peraturan dapat di-update.

**36.9 Kriteria Lisensi**

**36.9.1** Aktivasi lisensi berfungsi.

**36.9.2** Validasi berkala berfungsi.

**36.9.3** Grace period berfungsi.

**36.9.4** Agen dikunci saat lisensi tidak valid.

**36.9.5** Data tetap dapat diakses saat lisensi tidak valid.

**36.9.6** Lisensi offline untuk air-gapped deployment berfungsi.

**36.9.7** Lisensi key terenkripsi di database.

**36.10 Kriteria Export**

**36.10.1** PDF export menghasilkan dokumen profesional.

**36.10.2** DOCX export menghasilkan dokumen profesional.

**36.10.3** Template dapat dikustomisasi.

**36.10.4** Inline citation ter-embed di dokumen.

**36.10.5** Daftar Isi otomatis berfungsi.

**36.10.6** Bookmark PDF berfungsi.

**36.10.7** Metadata PDF terisi dengan benar.

---

# BAGIAN VII — TATA KELOLA DOKUMEN

---

## 37. SPESIFIKASI REPOSITORI

**37.1** Repositori menggunakan struktur monorepo dengan Cargo workspace untuk Rust dan pnpm workspace untuk frontend.

**37.2** Struktur direktori utama:

**37.2.1** `apps/web/`: Frontend SolidJS.

**37.2.2** `apps/server/`: Backend Rust (Axum, Custom Graph Engine, Multi-Provider LLM, Web Researcher, Knowledge Base, License Manager, database layer, crypto, services).

**37.2.3** `packages/shared/`: Shared types antara frontend dan backend.

**37.2.4** `packages/ui/`: Shared UI components.

**37.2.5** `infra/docker/`: Dockerfile dan docker-compose.yml.

**37.2.6** `infra/railway/`: railway.json.

**37.2.7** `infra/vps/`: Caddyfile dan install script.

**37.2.8** `scripts/`: Build dan utility scripts.

**37.2.9** `docs/`: Dokumentasi termasuk PRD ini.

**37.3** Root `Cargo.toml` mendefinisikan workspace dengan member `apps/server` dan workspace dependencies.

---

## 38. DOKUMEN TURUNAN

**38.1** Dokumen ini adalah sumber kebenaran mutlak bagi seluruh proyek PAUGERAN.

**38.2** Dokumen turunan yang harus dibuat berdasarkan dokumen ini:

**38.2.1** **Spesifikasi Arsitektur Teknis**: Detail implementasi arsitektur yang merujuk ke [CB §6] dan [CB §7].

**38.2.2** **Spesifikasi API Detail**: OpenAPI specification yang merujuk ke [CB §13].

**38.2.3** **Spesifikasi Database Detail**: Migration files dan query yang merujuk ke [CB §10].

**38.2.4** **Spesifikasi Multi-Provider LLM**: Detail implementasi adapter untuk setiap provider yang merujuk ke [CB §9].

**38.2.5** **Spesifikasi Legal Knowledge Base**: Detail skema, algoritma pencarian, dan integrasi yang merujuk ke [CB §11].

**38.2.6** **Spesifikasi Lisensi**: Detail protokol validasi, grace period, dan lisensi offline yang merujuk ke [CB §12].

**38.2.7** **Spesifikasi Penelitian Web**: Detail whitelist, mekanisme scraping, dan etika yang merujuk ke [CB §17].

**38.2.8** **Spesifikasi Inline Citation**: Detail format, visualisasi, dan konsistensi yang merujuk ke [CB §18].

**38.2.9** **Spesifikasi Export Dokumen**: Detail template, layout, dan fitur yang merujuk ke [CB §19].

**38.2.10** **Spesifikasi Aksesibilitas**: Detail implementasi ARIA, keyboard navigation, dan mode aksesibilitas yang merujuk ke [CB §28].

**38.2.11** **Rencana Pengujian (Test Plan)**: Test cases yang memverifikasi setiap kriteria di [CB §36] dan setiap larangan di [CB §34].

**38.2.12** **Model Data**: Entity relationship diagram yang merujuk ke [CB §10.1].

**38.2.13** **Panduan Deployment**: Step-by-step guide untuk setiap skenario di [CB §23].

**38.2.14** **Panduan Pengguna**: User manual yang merujuk ke [CB §29], [CB §28], dan [CB §27].

**38.2.15** **Panduan Operasional**: Runbook untuk monitoring [CB §32], backup [CB §33], dan incident response.

**38.2.16** **Dokumen Keamanan**: Security audit report yang merujuk ke [CB §30].

**38.2.17** **agen.md**: Kontrak perilaku AI agen pengembangan yang merujuk ke seluruh dokumen ini.

**38.3** Setiap dokumen turunan wajib:

**38.3.1** Mencantumkan referensi ke pasal dan ayat dokumen ini menggunakan format `[CB §X.Y]`.

**38.3.2** Tidak bertentangan dengan ketentuan dalam dokumen ini.

**38.3.3** Diperbarui setiap kali dokumen ini diperbarui.

**38.4** Jika terjadi pertentangan antara dokumen ini dan dokumen turunan manapun, dokumen ini yang berlaku dan dokumen turunan harus diperbaiki.

**38.5** Perubahan terhadap dokumen ini hanya dapat dilakukan melalui:

**38.5.1** Proposal perubahan tertulis.

**38.5.2** Review oleh seluruh stakeholder.

**38.5.3** Persetujuan tertulis.

**38.5.4** Komunikasi ke seluruh tim pengembangan.

---

## 39. PENUTUP

**39.1** Dokumen ini mendefinisikan secara lengkap dan mengikat: apa yang PAUGERAN harus menjadi, bagaimana PAUGERAN harus berperilaku, arsitektur teknis yang digunakan, spesifikasi komponen dan integrasi, serta kriteria penerimaan produk.

**39.2** Dokumen ini bukan roadmap, bukan rencana fase, bukan backlog, dan bukan rencana implementasi.

**39.3** Dokumen ini adalah kontrak produk yang mengikat seluruh pengembangan PAUGERAN. Setiap fitur, setiap perubahan, setiap keputusan teknis harus tunduk pada dokumen ini.

**39.4 Definisi Produk Akhir**

PAUGERAN adalah agen penalaran hukum yang dioperasikan sebagai satu binary universal berbasis Rust yang menjalankan server HTTP, dengan dukungan multi-provider LLM (tidak terbatas pada model ternama), Legal Knowledge Base internal yang dapat dibangun dari waktu ke waktu, penelitian web ke situs pemerintah resmi, inline citation yang detail di setiap output, export dokumen profesional, aksesibilitas tinggi, dan sistem lisensi yang adil. Pengguna menyediakan API key LLM dan lisensi key mereka sendiri melalui antarmuka web, yang melakukan wawancara masalah secara adaptif dalam sesi obrolan yang terisolasi dan independen, membangun rekonstruksi fakta, meneliti sumber hukum dari basis pengetahuan internal dan internet, menyusun dan menguji argumentasi, serta menghasilkan analisis hukum yang dapat ditelusuri dari kesimpulan sampai dasar hukum, fakta, bukti, dan sumbernya — dengan jaminan privasi data lokal atau terkelola, enkripsi AES-256-GCM untuk API key dan lisensi key, autentikasi opsional dengan first-user-is-admin, dan antarmuka yang dapat dikustomisasi sepenuhnya dengan aksesibilitas tinggi sesuai preferensi pengguna.

**39.5 Slogan Produk**

> **PAUGERAN**
> Memahami masalah. Menelusuri hukum. Menguji alasan.

---

**Dokumen:** PAUGERAN Contract Baseline
**Status:** FINAL — SUMBER KEBENARAN MUTLAK
**Disusun Oleh:** Tim Produk PAUGERAN
**Disetujui Oleh:** [Stakeholder]

---

*"Dokumen ini adalah konstitusi PAUGERAN. Setiap baris kode, setiap keputusan arsitektur, setiap fitur yang dibangun, dan setiap dokumen turunan yang ditulis harus tunduk pada apa yang tertulis di sini."*
