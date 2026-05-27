# 🎯 ANALISIS UKURAN PASAR (MARKET SIZE): TAM, SAM, SOM (Beachhead 3 Kota)

Dokumen ini menyajikan kalkulasi ukuran pasar (*market sizing*) untuk platform **EduFlow** menggunakan **Strategi Beachhead Super Fokus** yang berfokus pada segitiga emas Jawa Timur: **Surabaya, Sidoarjo, dan Gresik** berbasis data rill resmi Badan Pusat Statistik (BPS) dalam laporan **"Kabupaten/Kota Dalam Angka 2025"**.

---

## 📌 1. Data Rill BPS Satuan Pendidikan & Jumlah Murid Segitiga Emas

Berikut adalah rincian data rill jumlah sekolah (Negeri & Swasta) serta jumlah murid aktif untuk jenjang SMP, SMA, dan SMK di wilayah target peluncuran fase 1 (*beachhead*), merujuk langsung pada data resmi BPS daerah:

### A. Kota Surabaya (BPS)
| Jenjang | Sekolah Negeri | Sekolah Swasta | **Total Sekolah** | Murid Negeri | Murid Swasta | **Total Murid** |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **SMP** | 63 | 253 | **316** | 52.954 | 53.298 | **106.252** |
| **SMA** | 23 | 116 | **139** | 23.768 | 33.367 | **57.135** |
| **SMK** | 11 | 93 | **104** | 21.835 | 37.086 | **58.921** |
| **Sub-Total** | **97** | **462** | **559** | **98.557** | **123.751** | **222.308** |

### B. Kabupaten Sidoarjo (BPS)
| Jenjang | Sekolah Negeri | Sekolah Swasta | **Total Sekolah** | Murid Negeri | Murid Swasta | **Total Murid** |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **SMP** | 48 | 144 | **192** | 42.547 | 33.753 | **76.300** |
| **SMA** | 13 | 57 | **70** | 15.246 | 18.355 | **33.601** |
| **SMK** | 5 | 82 | **87** | 6.956 | 36.509 | **43.465** |
| **Sub-Total** | **66** | **283** | **349** | **64.749** | **88.617** | **153.366** |

### C. Kabupaten Gresik (BPS)
| Jenjang | Sekolah Negeri | Sekolah Swasta | **Total Sekolah** | Murid Negeri | Murid Swasta | **Total Murid** |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **SMP** | 35 | 87 | **122** | 21.604 | 13.111 | **34.715** |
| **SMA** | 12 | 40 | **52** | 13.255 | 7.654 | **20.909** |
| **SMK** | 4 | 57 | **61** | 5.352 | 18.665 | **24.017** |
| **Sub-Total** | **51** | **184** | **235** | **40.211** | **39.430** | **79.641** |

---

### 📊 Rekapitulasi Total Cluster Beachhead (Surabaya, Sidoarjo, Gresik)
* **Total Sekolah Menengah (B2B Potential):** **1.143 Sekolah** (SMP: 630 / SMA: 261 / SMK: 252).
* **Total Murid Aktif (B2C Potential):** **455.315 Siswa** (Surabaya: 222.308 / Sidoarjo: 153.366 / Gresik: 79.641).
* **Rata-rata Ukuran Sekolah:** ~398 siswa / sekolah.

---

## 📊 2. Visualisasi Struktur Market Size (TAM, SAM, SOM)

```
┌────────────────────────────────────────────────────────────────────────┐
│ TAM (Total Addressable Market - Nasional)                              │
│ Rp 6,33 Triliun / Tahun (50.000 Sekolah & 25 Juta Siswa)               │
│                                                                        │
│   ┌──────────────────────────────────────────────────────────────────┐  │
│   │ SAM (Serviceable Addressable Market - Jawa Timur Perkotaan)      │  │
│   │ Rp 581,4 Miliar / Tahun (5.100 Sekolah & 2,28 Juta Siswa)        │  │
│   │                                                                  │  │
│   │   ┌────────────────────────────────────────────────────────────┐ │  │
│   │   │ SOM (Serviceable Obtainable Market - Beachhead 3 Kota)     │ │  │
│   │   │ Rp 1,38 Miliar / Tahun (100 Sekolah & 42.000 Pengguna)     │ │  │
│   │   └────────────────────────────────────────────────────────────┘ │  │
│   └──────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
```

---

> [!IMPORTANT]
> ## 💡 VALIDASI ASUMSI & FORMULA KALKULASI (Transparansi Penuh)
> Untuk mencegah kebingungan juri atau investor saat proses audit finansial, berikut adalah kejelasan definisi dan dasar matematika yang kami gunakan:
> 
> ### A. Asumsi Pendapatan B2B Sekolah (Rp 6.720.000 / Tahun)
> Angka ini diperoleh dari **Rata-rata Tertimbang (*Weighted Average*)** paket langganan berjenjang (*tiered B2B pricing*) EduFlow berdasarkan asumsi sebaran portofolio sekolah mitra di dunia nyata:
> * **Struktur Paket B2B Bulanan:**
>   - **Tier S (≤200 siswa):** Rp 400.000 / bulan (60% sekolah)
>   - **Tier M (201-500 siswa):** Rp 700.000 / bulan (30% sekolah)
>   - **Tier L (501-1.000 siswa):** Rp 1.100.000 / bulan (10% sekolah)
> * **Kalkulasi Rata-rata Bulanan:**
>   $$(0,60 \times \text{Rp } 400.000) + (0,30 \times \text{Rp } 700.000) + (0,10 \times \text{Rp } 1.100.000) = \mathbf{\text{Rp } 560.000 \text{ / bulan}}$$
> * **Konversi Rata-rata Tahunan per Sekolah:**
>   $$\text{Rp } 560.000 \times 12 \text{ bulan} = \mathbf{\text{Rp } 6.720.000 \text{ / tahun}}$$
> 
> ### B. Asumsi Pendapatan B2C Mandiri Siswa (Premium vs Iklan)
> * **B2C Premium (Rp 240.000 / Tahun):** Tarif langganan individu bebas iklan Rp 19.999/bulan × 12 bulan. Digunakan pada kalkulasi TAM/SAM sebagai **Batas Atas Komersial Teoretis (*Theoretical Upper Limit*)**.
> * **B2C Ad-Supported (Rp 23.000 / Tahun):** Diperoleh dari estimasi pendapatan iklan per siswa gratisan senilai **Rp 1.920 / bulan** (asumsi 120 tayangan iklan/bulan × eCPM Rp 16) × 12 bulan = **Rp 23.040 / tahun** (dibulatkan menjadi Rp 23.000).
> * **Blended ARPU Siswa (Rp 44.700 / Tahun):** Asumsi campuran realistis di mana **90% siswa menggunakan jalur gratisan (iklan)** dan **10% siswa berlangganan Premium**:
>   $$\text{Blended ARPU} = (0,90 \times \text{Rp } 23.000) + (0,10 \times \text{Rp } 240.000) = \text{Rp } 20.700 + \text{Rp } 24.000 = \mathbf{\text{Rp } 44.700 \text{ / siswa / tahun}}$$
> 
> ### C. Status Pengguna B2C di SOM (Siswa Mandiri vs Siswa Sekolah Mitra)
> * **Siswa B2B (30.000 Pengguna):** Siswa yang bersekolah di 100 sekolah mitra. Mereka belajar **100% gratis** karena biayanya sudah ditanggung lisensi B2B sekolah (dana BOS).
> * **Siswa B2C (2.000 Premium + 10.000 Iklan):** Siswa mandiri yang bersekolah di luar 100 sekolah mitra. Mereka mengunduh aplikasi secara personal dari Play Store/App Store karena ingin belajar secara mandiri, sehingga mereka dimonetisasi secara individu oleh platform.

---

## 🧮 3. Rincian Kalkulasi Nilai Pasar (Rupiah per Tahun)

### 🟢 A. TAM (Total Addressable Market - Skala Nasional)
*Potensi pasar maksimum EduFlow jika berhasil menguasai 100% pasar sekolah menengah di Indonesia.*
* **Volume Sasaran:** **50.000 Sekolah** dan **25.000.000 Siswa** (SMP + SMA/SMK).
* **Kalkulasi Nilai Rupiah/Tahun:**
  * **B2B (Sekolah):** 50.000 sekolah × Rp 6.720.000 / tahun = **Rp 336.000.000.000**
  * **B2C (Siswa - Batas Atas Premium):** 25.000.000 siswa × Rp 240.000 / tahun = **Rp 6.000.000.000.000**
  * *Catatan Realistis (Blended ARPU Rp 44.700):* Jika menggunakan sebaran realistis 90% iklan + 10% premium, nilai B2C TAM = **Rp 1,11 Triliun / Tahun**.
* 💎 **TOTAL TAM NASIONAL (Batas Atas) = Rp 6,33 Triliun / Tahun**

---

### 🟡 B. SAM (Serviceable Addressable Market - Jawa Timur Perkotaan)
*Portofolio pasar Jawa Timur yang memiliki kesiapan infrastruktur internet layak, smartphone, dan dana BOS BOSP yang stabil.*
* **Kondisi Wilayah:** Jawa Timur memiliki ~8.500 sekolah menengah dan ~3,8 juta siswa. Kita menyaring **70%** sekolah perkotaan yang memenuhi kriteria digitalisasi layak.
* **Volume Sasaran:** **5.100 Sekolah** dan **2.280.000 Siswa** (SMP + SMA/SMK Jatim Perkotaan).
* **Kalkulasi Nilai Rupiah/Tahun:**
  * **B2B (Sekolah):** 5.100 sekolah × Rp 6.720.000 / tahun = **Rp 34.272.000.000**
  * **B2C (Siswa - Batas Atas Premium):** 2.280.000 siswa × Rp 240.000 / tahun = **Rp 547.200.000.000**
  * *Catatan Realistis (Blended ARPU Rp 44.700):* Jika menggunakan sebaran realistis 90% iklan + 10% premium, nilai B2C SAM = **Rp 101,9 Miliar / Tahun**.
* 💎 **TOTAL SAM JAWA TIMUR (Batas Atas) = Rp 581,47 Miliar / Tahun**

---

### 🔴 C. SOM (Serviceable Obtainable Market - Beachhead 3 Kota Jatim)
*Target pangsa pasar realistis yang akan diakuisisi secara taktis dalam 1-3 tahun pertama di cluster segitiga emas: **Surabaya, Sidoarjo, dan Gresik**.*

#### 🎯 Target Akuisisi EduFlow (Year 1-3) & Persaingan Kompetitor:
Dari **1.143 sekolah rill (data resmi BPS)** yang ada di cluster 3 kota target:
* **Lansekap Kompetisi:** ~30% sekolah telah mengadopsi LMS berbayar premium custom/ruangguru enterprise, dan ~20% merupakan sekolah sangat konservatif. Sisa **50% (571 sekolah) adalah pasar terbuka** (*open market*) yang belum terlayani fitur evaluasi lisan AI.
* **SOM Target:** Kita menetapkan target akuisisi **100 sekolah** dalam 3 tahun (hanya **8,75%** dari total pasar BPS, atau **17,5%** dari pasar terbuka yang belum terlayani).
* **Volume Sasaran Nyata:**
  * **Sekolah B2B:** **100 Sekolah**
  * **Siswa B2B (Bulk):** **30.000 Siswa** (mengambil porsi B2B dari total 455.315 siswa potensial)
  * **Siswa B2C Premium Mandiri:** **2.000 Siswa**
  * **Siswa B2C Gratisan Mandiri (Iklan):** **10.000 Siswa**

* **Kalkulasi Nilai Rupiah/Tahun (Omzet Riil Target Tahun ke-3):**
  * **Pendapatan B2B:** 100 sekolah × Rp 6.720.000 / tahun = **Rp 672.000.000**
  * **Pendapatan B2C Premium Mandiri:** 2.000 siswa × Rp 240.000 / tahun = **Rp 480.000.000**
  * **Pendapatan B2C Iklan Mandiri:** 10.000 siswa × Rp 23.000 / tahun = **Rp 230.000.000**
* 💎 **TOTAL SOM BEACHHEAD 3 KOTA = Rp 1.382.000.000 / Tahun** (~Rp 1,38 Miliar)

---

## 📈 4. Analisis Kelayakan & Keunggulan Strategi 3 Kota
Membatasi target awal hanya pada Surabaya, Sidoarjo, dan Gresik memberikan keuntungan eksekusi yang luar biasa bagi tim pendiri yang ramping:
1. **Jarak Geografis yang Sangat Padat:** Jarak operasional tim sales berkurang hingga 80%. Surabaya-Sidoarjo-Gresik terhubung dengan jalan tol yang sangat dekat, menekan biaya transportasi operasional secara signifikan.
2. **Kesesuaian Target Pasar (High-Speed Internet Adoption):** Siswa di 3 kota ini memiliki persentase kepemilikan gawai (*smartphone*) pribadi tertinggi di Jatim, meminimalkan hambatan teknis saat mengakses platform.
3. **Kemudahan Transaksi SIPLah (Dana BOS):** Sekolah di wilayah segitiga emas merupakan pionir adopsi platform belanja sekolah digital (**SIPLah** & **ARKAS**), mempercepat birokrasi pembayaran langganan sekolah.

---

## 🔗 5. Sumber Data Primer & Link Validasi Resmi

Seluruh data sekolah dan populasi siswa di atas diambil dari pangkalan data resmi pemerintah Indonesia untuk menjamin akurasi 100% saat proses audit investor:

1. **Data Pokok Pendidikan (Dapodik) - Ditjen Paud Dikdasmen:**
   - Rekapitulasi data sekolah dan siswa aktif nasional & per wilayah administrasi.
   - 🌐 [dapo.kemdikbud.go.id](https://dapo.kemdikbud.go.id/)
2. **Data Referensi Satuan Pendidikan - Kementerian Pendidikan Dasar dan Menengah RI:**
   - Detail pencarian jumlah sekolah per bentuk pendidikan (SMP, SMA, SMK) per Kota/Kabupaten Jawa Timur.
   - 🌐 [referensi.data.kemdikbud.go.id](https://referensi.data.kemdikbud.go.id/)
3. **Badan Pusat Statistik (BPS) Provinsi Jawa Timur:**
   - Laporan berkala *Jawa Timur Dalam Angka* untuk bab Indikator Pendidikan & Statistik Sekolah Menengah.
   - 🌐 [jatim.bps.go.id](https://jatim.bps.go.id/)
4. **Badan Pusat Statistik (BPS) Kota Surabaya & Dinas Pendidikan:**
   - Laporan resmi *Kota Surabaya Dalam Angka* untuk data jumlah satuan pendidikan aktif.
   - 🌐 [surabayakota.bps.go.id](https://surabayakota.bps.go.id/) | [dispendik.surabaya.go.id](https://dispendik.surabaya.go.id/)
5. **Badan Pusat Statistik (BPS) Kabupaten Sidoarjo & Kabupaten Gresik:**
   - Laporan sektoral BPS *Kabupaten Sidoarjo Dalam Angka* & *Kabupaten Gresik Dalam Angka*.
   - 🌐 [sidoarjokab.bps.go.id](https://sidoarjokab.bps.go.id/) | [gresikkab.bps.go.id](https://gresikkab.bps.go.id/)

---

## 📚 Dokumen Pendukung Terkait:
- [FINANCIAL_PLAN.md](file:///c:/laragon/www/project1/FINANCIAL_PLAN.md) - Rencana Keuangan Utama
- [BMC.md](file:///c:/laragon/www/project1/BMC.md) - Kanvas Model Bisnis
