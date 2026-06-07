# Catatan Penting Arah Pengembangan EduFlow

Dokumen ini mencantumkan batasan dan keputusan strategis mengenai arah pengembangan produk dan model bisnis EduFlow. Informasi di bawah ini menjadi acuan utama dalam menyelaraskan seluruh proposal proyek.

---

## 📌 Batasan & Keputusan Strategis Utama

### 1. Segmen Pengguna Tunggal: Khusus Mahasiswa (B2C)
* EduFlow dikembangkan **khusus untuk mahasiswa**. Seluruh materi, pendekatan, dan gaya interaksi di dalam aplikasi disesuaikan dengan kebutuhan akademik mahasiswa perguruan tinggi (misalnya pengerjaan tugas kuliah, pemahaman silabus mata kuliah, dan persiapan ujian).

### 2. Model Pembelajaran Mandiri (Self-Directed Learning)
* Mahasiswa belajar secara **mandiri sepenuhnya** di dalam platform.
* Platform memanfaatkan asisten AI personal berbasis **Teknik Feynman** dan **Active Recall** untuk memvalidasi pemahaman mahasiswa secara asinkron.
* **Tidak melibatkan dosen/pengajar** dalam alur pembelajaran harian mahasiswa. Mahasiswa mendapatkan umpan balik, evaluasi kognitif, dan motivasi langsung dari sistem AI, bukan dari dosen.

### 3. Model Bisnis Murni B2C (Business-to-Consumer)
* EduFlow dipasarkan dan dijual langsung kepada mahasiswa sebagai konsumen akhir (**murni B2C**).
* **Tidak ada model bisnis B2B** maupun rencana kemitraan resmi dengan institusi perguruan tinggi/kampus.
* Tidak ada skema lisensi kampus, kode undangan dari dosen, atau integrasi dengan sistem administrasi kampus.

### 4. Pilihan API AI: Gemini 2.5 Flash & Biaya Token
* **Model AI Utama:** Menggunakan model **Gemini 2.5 Flash** (via Google AI Studio atau OpenRouter) untuk memproses penilaian kognitif dan analisis miskonsepsi.
* **Tarif Biaya Token (Resmi):**
  - **Input Tokens:** **$0,075 per 1 juta tokens** (dengan prompt caching; jika tanpa cache tarifnya **$0,15 - $0,30 per 1 juta tokens** atau sekitar **Rp2.400 - Rp4.900**).
  - **Output Tokens:** **$0,30 per 1 juta tokens** (konteks ≤128k; jika di atas tarifnya **$0,60 - $2,50 per 1 juta tokens** atau sekitar **Rp9.800 - Rp40.750**).
  - *Free Tier:* Tersedia gratis untuk kuota harian terbatas guna meminimalkan biaya selama fase pengembangan awal.

### 5. Payment Gateway B2C: DOKU (Metode Pembayaran Terbatas)
* **Penyedia Layanan:** Menggunakan payment gateway **DOKU** untuk mengelola transaksi pembayaran langganan premium B2C mahasiswa.
* **Metode Pembayaran Terbatas:** Pembayaran **hanya didukung via E-Wallet** (GoPay, OVO, Dana, LinkAja, ShopeePay) dan **QRIS** demi mempermudah akses pembayaran yang umum digunakan di kalangan mahasiswa tanpa memerlukan kartu kredit atau akun bank konvensional.


---

## 🛠️ Implikasi Terhadap Fitur & Teknologi (Perlu Penyesuaian Dokumen)

Berdasarkan keputusan strategis di atas, fitur-fitur berikut yang sebelumnya direncanakan harus **dihapus** atau **dialihkan fungsinya**:

| Fitur Lama (B2B / Involusi Dosen) | Status | Tindakan Lanjutan & Alihan Fitur |
| :--- | :---: | :--- |
| **Antarmuka Dosen (Mobile App)** | ❌ Dihapus | Dihapus sepenuhnya dari menu aplikasi mobile. |
| **Dashboard Perguruan Tinggi & Dosen (Web Admin)** | ❌ Dihapus | Dihapus sepenuhnya dari Website Admin. Website Admin hanya digunakan oleh Developer/Administrator internal untuk manajemen kurikulum, konten kreator, dan moderasi. |
| **Laporan Kognitif Kolektif (Rapor)** | ❌ Dihapus | Diubah menjadi **Dashboard Analitik Kognitif Personal** yang diakses langsung oleh mahasiswa sendiri untuk melihat grafik perkembangan belajar mereka secara mandiri. |
| **Sistem Kode Undangan Kampus** | ❌ Dihapus | Diubah menjadi skema promosi B2C umum (misalnya kode *referral* antar-mahasiswa atau uji coba gratis secara individu). |

---

## 📊 Estimasi Ukuran Pasar (TAM, SAM, SOM) EduFlow B2C

Sesuai dengan pivot model bisnis ke arah **B2C Murni untuk Mahasiswa secara Mandiri**, berikut adalah target cakupan pasar (*market sizing*) yang ditetapkan untuk fase awal peluncuran:

### 1. TAM (Total Addressable Market) - Seluruh Mahasiswa Indonesia
* **Definisi:** Total mahasiswa aktif secara nasional di seluruh perguruan tinggi di Indonesia.
* **Volume Sasaran:** **8.467.714 mahasiswa aktif** (merujuk data resmi [Badan Pusat Statistik / *Statistical Yearbook of Indonesia 2025*](https://www.bps.go.id)).

### 2. SAM (Serviceable Addressable Market) - Mahasiswa Telkom University (Seluruh Cabang)
* **Definisi:** Total mahasiswa aktif yang menempuh pendidikan di seluruh jaringan kampus nasional Telkom University (mencakup Kampus Bandung, Jakarta, Purwokerto, dan Surabaya).
* **Volume Sasaran:** **46.246 mahasiswa aktif** (merujuk data resmi [Fakta dan Angka - Telkom University](https://telkomuniversity.ac.id/fakta-dan-angka/)).

### 3. SOM (Serviceable Obtainable Market) - Mahasiswa Telkom University Kampus Surabaya
* **Definisi:** Target pangsa pasar awal (*beachhead*) yang realistis untuk diakuisisi pada awal peluncuran aplikasi, yaitu berfokus pada mahasiswa di Telkom University Kampus Surabaya (dahulu ITTelkom Surabaya).
* **Volume Sasaran:** **~3.500 mahasiswa aktif** secara keseluruhan (dengan pendaftaran mahasiswa baru tahun akademik 2024/2025 saja mencapai **1.056 mahasiswa baru** sebagaimana tercatat di [Portal Penerimaan Kampus Surabaya](https://surabaya.telkomuniversity.ac.id)).

## 🧮 Kalkulasi Finansial AI Token & Monetisasi B2C (No B2B)

Untuk menjaga keberlangsungan finansial startup tanpa adanya model bisnis B2B (sekolah/kampus), berikut adalah detail perhitungan biaya API kognitif (*Feynman AI*) per sesi, bagi hasil kreator, serta struktur OpEx bulanan yang diselaraskan dengan [FINANCIAL_PLAN.md](file:///c:/laragon/www/project1/FINANCIAL_PLAN.md):

### 1. Tarif & Perhitungan Biaya API Gemini 2.5 Flash per Sesi

Untuk memberikan proyeksi yang komprehensif, kami membandingkan tarif resmi dari dua provider (OpenRouter dan Google AI Studio langsung) menggunakan asumsi kurs stabil **Rp16.000 per USD**.

* **A. Tarif Token API Resmi (Per 1 Juta Tokens):**
  - **OpenRouter (Skema Conservatif / Lama):**
    - **Input Tokens:** **$0,35 per 1 juta tokens** (atau sekitar **Rp5,60 per 1.000 tokens**).
    - **Output Tokens:** **$1,05 per 1 juta tokens** (atau sekitar **Rp16,80 per 1.000 tokens**).
  - **Google AI Studio (Skema Efisiensi Tinggi / Baru):**
    - **Input Tokens (≤128k context):** **$0,075 per 1 juta tokens** (dengan prompt caching; jika tanpa cache tarifnya **$0,15 per 1 juta tokens** atau sekitar **Rp2,40 per 1.000 tokens**).
    - **Output Tokens (≤128k context):** **$0,30 per 1 juta tokens** (atau sekitar **Rp4,80 per 1.000 tokens**).

* **B. Asumsi Konsumsi Token per Sesi Evaluasi Feynman AI:**
  - **Input:** ~1.000 tokens (mencakup System Prompt Teknik Feynman, silabus mata kuliah, dan transkripsi suara mahasiswa).
  - **Output:** ~300 tokens (mencakup feedback kualitatif AI dan skor pemahaman kognitif).

* **C. Matematika Biaya Sesi Riil:**
  - **Skenario OpenRouter (Conservatif):**
    $$\text{Biaya Input} = \frac{1.000}{1.000.000} \times \$0,35 \times \text{Rp16.000} = \text{Rp5,60}$$
    $$\text{Biaya Output} = \frac{300}{1.000.000} \times \$1,05 \times \text{Rp16.000} = \text{Rp5,04}$$
    $$\text{Total HPP Sesi Riil} = \text{Rp5,60} + \text{Rp5,04} = \mathbf{\text{Rp10,64 / sesi}}$$
  - **Skenario Google AI Studio (Efisiensi Tinggi):**
    $$\text{Biaya Input (dengan cache)} = \frac{1.000}{1.000.000} \times \$0,075 \times \text{Rp16.000} = \text{Rp1,20}$$
    $$\text{Biaya Output} = \frac{300}{1.000.000} \times \$0,30 \times \text{Rp16.000} = \text{Rp1,44}$$
    $$\text{Total HPP Sesi Riil} = \text{Rp1,20} + \text{Rp1,44} = \mathbf{\text{Rp2,64 / sesi}}$$
  - **Biaya HPP API Perencanaan (Buffer Pengaman):** Kami menggunakan **Rp120 / sesi** di dalam seluruh model proyeksi keuangan. Angka ini bertindak sebagai buffer pengaman yang sangat konservatif untuk mengantisipasi kegagalan caching, input prompt yang lebih panjang pada bab mata kuliah rumit, serta fluktuasi kurs mata uang asing.

### 2. Akumulasi Biaya HPP API Bulanan per Jalur Pengguna

Berdasarkan biaya buffer **Rp120/sesi**, akumulasi biaya API bulanan untuk tiap mahasiswa adalah sebagai berikut:

* **A. Jalur Pengguna Gratis (Limit Kuota Standar / Rata-rata Penggunaan Organik):**
  - Mengasumsikan rata-rata penggunaan normal mahasiswa aktif secara berkala adalah **60 sesi evaluasi per bulan** (3x sesi per hari selama 20 hari aktif belajar).
  - **HPP API Bulanan:** 60 sesi × Rp120/sesi = **Rp7.200 / mahasiswa / bulan** (inilah mengapa HPP API standard dapat mencapai **Rp7.000 lebih** per pengguna aktif).
* **B. Jalur Pengguna Premium (Limit Kuota Maksimum):**
  - Pelanggan Premium mendapatkan hak evaluasi mandiri hingga batas maksimum **100 sesi evaluasi per bulan**.
  - **HPP API Bulanan:** 100 sesi × Rp120/sesi = **Rp12.000 / mahasiswa / bulan** .

### 3. Struktur HPP & Margin Bersih Pengguna B2C Premium (Limit 100 Sesi)

Setiap pelanggan Premium membayar biaya langganan bulanan sebesar **Rp19.999 / bulan (dibulatkan menjadi Rp20.000)**. Berikut adalah rincian lengkap HPP variabel langsung bulanan yang menjelaskan mengapa biaya totalnya mencapai **Rp18.000 lebih**:

* **Payment Gateway (PG) DOKU (Metode QRIS & E-Wallet):** **Rp300 / transaksi** (Ditetapkan flat rate 0,7% dari biaya langganan, dibulatkan ke atas sebagai buffer biaya administrasi).
* **Bagi Hasil Konten Kreator (30%):** **Rp6.000 / bulan** (Dihitung dari 30% × Rp19.999 untuk mendanai Premium Royalty Pool bagi dosen, asdos, dan mahasiswa kreator konten video pembelajaran vertikal pendek).
* **Gemini 2.5 Flash API HPP (100 Sesi):** **Rp12.000 / bulan** (100 sesi × Rp120/sesi buffer pengaman).
* **Bunny Stream Video (Bandwidth):** **Rp30 / bulan** (Estimasi konsumsi bandwidth rata-rata 0,35 GB per pengguna per bulan untuk memutar video pembelajaran micro-learning).
* **Total HPP B2C Premium (Worst-Case / Penggunaan Maksimal):**
  $$\text{Total HPP Premium} = \text{Rp300 (PG)} + \text{Rp6.000 (Kreator)} + \text{Rp12.000 (Gemini)} + \text{Rp30 (Video)} = \mathbf{\text{Rp18.330 / bulan}}$$
* **Margin Keuntungan Bersih Platform per Pengguna Premium:**
  $$\text{Laba Bersih} = \text{Rp20.000 (Biaya Langganan)} - \text{Rp18.330 (HPP)} = \mathbf{+\text{Rp1.670 / bulan}}$$
  *(Catatan: Apabila pengguna melewati batas 100 sesi, sistem mengaktifkan Ad-Lock di mana setiap sesi tambahan didanai melalui rewarded video ads dengan pendapatan iklan Rp150 per tayangan, menghasilkan surplus bersih +Rp30 s.d. +Rp134 per sesi).*

### 4. Struktur Biaya Operasional Bulanan (OpEx Pentahapan - Staged Scaling)

Untuk mencegah risiko kegagalan arus kas (*cash flow bleeding*) di fase awal, pengeluaran operasional dibagi menjadi 3 tahapan realistis berbasis volume pengguna:

* **TAHAP 1: Prototype & Beta Testing (Bulan 1 - 2) — Rp800.000 / bulan**
  - **Infrastruktur Server:** **Rp150.000** (Biznet Gio / Host.id VPS 2-Core, RAM 4GB untuk Laravel API & DB lokal).
  - **SaaS Pendukung:** **Rp150.000** (Domain `.id` / `.co.id`, DNS, email transaksional awal).
  - **Pemasaran & Komunitas Awal:** **Rp500.000** (Penyebaran brosur digital dan promosi langsung di kampus beachhead).
  - **Tim CS & Operasional:** **Rp0** (Ditangani langsung secara kolektif oleh tim founder / *developer-run*).
* **TAHAP 2: Peluncuran Awal & Kemitraan (Bulan 3 - 5) — Rp4.550.000 / bulan**
  - **Infrastruktur Server:** **Rp750.000** (Upgrade ke Main VPS IDCloudHost + DB Managed eksternal untuk menampung ratusan pengguna aktif).
  - **SaaS Pendukung:** **Rp300.000** (Upgrade kuota SaaS email marketing & monitoring server).
  - **Pemasaran & Komunitas:** **Rp1.500.000** (Penyelenggaraan event kampus dan insentif cetak materi promosi).
  - **Tim CS Part-Time:** **Rp2.000.000** (Perekrutan 1 orang mahasiswa magang sebagai staf Customer Support paruh waktu).
* **TAHAP 3: Skala Menengah & Growth (Bulan 6+) — Rp9.900.000 / bulan**
  - **Infrastruktur Server:** **Rp1.800.000** (App Server Dedicated + Managed Redis & DB + dedicated serverless GPU RunPod untuk memproses Whisper STT secara paralel).
  - **SaaS Pendukung:** **Rp500.000** (SaaS tools skala enterprise).
  - **Pemasaran & Komunitas:** **Rp2.000.000** (Iklan digital media sosial terarah dan program Campus Ambassador).
  - **Tim CS Full-Time:** **Rp5.600.000** (Gaji untuk 2 orang CS junior purna waktu @ Rp2.800.000/bulan sesuai standar kelayakan UMK kota lapis kedua).




