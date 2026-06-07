BAB 5
RANCANGAN IMPLEMENTASI

5.1 Peta Jalan Pengembangan (Roadmap)

EduFlow menerapkan strategi pengembangan bertahap (Phased-Rollout Strategy) untuk mencapai target pasar mahasiswa secara mandiri (B2C) dalam kurun waktu 18-24 bulan ke depan. Pola ekspansi ini membagi fokus bisnis secara bertingkat, diawali dari lingkup kampus peluncuran awal (*beachhead*), perluasan regional jaringan kampus, hingga skala nasional.

Fase 1: Validasi Produk & Penetrasi Awal (Bulan 1 - 6)
Pada tahap ini, fokus utama EduFlow adalah menyempurnakan engine AI (Gemini 2.5 Flash dan Whisper STT) serta akuisisi pengguna awal (*early adopters*).
* **Bulan 1-2:** Uji coba versi beta (*beta testing*) pada kelompok mahasiswa aktif di Telkom University Kampus Surabaya untuk menguji akurasi Whisper STT lokal dalam merekam aksen lisan mahasiswa serta menguji kecepatan respons server AI.
* **Bulan 3-4:** Peluncuran komersial versi pertama (v1.0) dengan target akuisisi 300 pengguna aktif mandiri (BEP operasional tahap awal).
* **Bulan 5-6:** Aktivasi program *Creator Fund* bagi asisten dosen (asdos) dan mahasiswa berprestasi untuk memproduksi rangkuman video mata kuliah dasar (Kalkulus, Alpro, Statistika). Target pencapaian **1.500 s.d. 3.000 pengguna aktif bulanan (MAU)** dengan porsi Premium berkisar antara 15% s.d. 20% (mencapai profitabilitas awal platform).

Fase 2: Skalabilitas & Dominasi Jaringan Kampus (Bulan 7 - 12)
Setelah mencapai validasi pasar yang solid (*Product-Market Fit*), fokus bergeser ke pertumbuhan eksponensial di tingkat regional jaringan kampus Telkom University (SAM).
* **Bulan 7-8:** Kemitraan komunitas dengan organisasi mahasiswa (BEM dan Himpunan Mahasiswa/Hima) di seluruh jaringan kampus Telkom University (Bandung, Jakarta, Purwokerto, Surabaya) serta merilis fitur *Leaderboard* kompetitif lintas fakultas/jurusan.
* **Bulan 9-10:** Integrasi sistem analisis deteksi *learning gap* otomatis untuk merangkum topik-topik mata kuliah yang belum dipahami mahasiswa secara individual.
* **Bulan 11-12:** Pencapaian target **10.000 hingga 15.000 pengguna aktif bulanan (MAU)** di seluruh jaringan regional kampus Telkom University (skema Base Case hingga Optimistis, mengamankan unit economics yang profitabel secara absolut).

Fase 3: Ekspansi Nasional B2C (Tahun ke-2)
Pada tahap ini, pematangan bisnis diarahkan pada perluasan jangkauan pasar mahasiswa nasional dan diversifikasi materi perkuliahan.
* **Ekspansi Pasar B2C Nasional:** Penetrasi pasar mahasiswa di luar Telkom University, menyasar perguruan tinggi negeri dan swasta besar di pulau Jawa (UI, ITB, UGM, Unair, ITS, Binus, dll.) melalui program *Campus Ambassador* (Duta Kampus) dan pemasaran media sosial viral.
* **Ekspansi Silabus Mata Kuliah:** Menambahkan repositori materi video pendek untuk mata kuliah lanjutan di berbagai jurusan (Teknik, Ekonomi, Hukum, Desain) sesuai dengan permintaan dan kebutuhan mahasiswa.
* **API as a Service:** Lisensi modul evaluasi suara AI (*Feynman AI Engine*) ke platform edukasi atau bimbingan belajar pihak ketiga.

5.2 Struktur Tim dan Manajemen (Team Structure)

EduFlow dikelola oleh jajaran pendiri (*Founders*) mahasiswa yang memadukan keahlian teknologi (AI/Engineering) dan pemahaman mendalam tentang kendala akademis perkuliahan. Tutor otomatis berbasis AI memotong alur koreksi manual sehingga tim pengelola dapat tetap ramping dengan biaya operasional minimal.

Ifan Maulana Abi: Chief Executive Officer (CEO)
* **Fokus:** Perumusan strategi bisnis makro B2C, penggalangan dana (*fundraising*), negosiasi kemitraan strategis dengan konten kreator edukasi nasional, serta kepemimpinan visi jangka panjang.
* **Peran:** Memastikan orientasi produk tetap relevan untuk memecahkan masalah belajar mandiri mahasiswa serta menjaga kepatuhan operasional platform terhadap regulasi privasi data pengguna.

Hafiy Atha Rabani: Chief Technology Officer (CTO) & AI Architect
* **Fokus:** Pengembangan arsitektur aplikasi (frontend/backend), integrasi API model LLM (Gemini 2.5 Flash via OpenRouter), optimalisasi rekayasa perintah (*prompt engineering*), serta pengelolaan *serverless cloud infrastructure*.
* **Peran:** Menjaga tingkat *latency* respons AI di bawah 2 detik dan meminimalkan biaya token AI per evaluasi suara agar margin keuntungan kotor tetap di atas 80%.

Muhammad Zukani Luthfi: Chief Marketing Officer (CMO) & Community Manager
* **Fokus:** Akuisisi pengguna B2C (mahasiswa mandiri), kampanye pemasaran media sosial kreatif (TikTok, Instagram, YouTube), program *referral* (Member-Get-Member), serta manajemen program *Creator Fund* untuk pembuat konten akademik.
* **Peran:** Meningkatkan basis pengguna aktif harian (*Daily Active Users*) serta membangun loyalitas komunitas mahasiswa untuk memicu efek pemasaran dari mulut ke mulut (*Word-of-Mouth*).

Jenelle Adelline: Chief Financial Officer (CFO)
* **Fokus:** Pengelolaan arus kas (*cash flow*), perencanaan anggaran operasional, penyusunan laporan keuangan, serta manajemen administrasi internal.
* **Peran:** Menjaga efisiensi pengeluaran dan memastikan kelangsungan operasional tim selama fase validasi pasar.

Mochammad Raditya Akmaludin: Chief Product Officer (CPO)
* **Fokus:** Penyusunan blueprint materi evaluasi agar selaras dengan silabus perkuliahan tinggi dasar, perancangan parameter dan indikator penilaian kognitif (Teknik Feynman), serta riset kebutuhan fitur belajar mandiri.
* **Peran:** Menyelaraskan teknologi AI dengan standar silabus mata kuliah sulit sehingga EduFlow dipercaya mahasiswa sebagai asisten belajar mandiri yang kredibel secara akademik.

5.3 Strategi Mitigasi Risiko

EduFlow mengidentifikasi beberapa risiko utama beserta langkah penyelesaiannya:

5.3.1 Risiko Teknologi (Ketergantungan API & Latency)
* **Ancaman:** Ketidakstabilan sistem (*downtime*) atau kenaikan harga token sepihak dari penyedia API (OpenRouter/Gemini).
* **Mitigasi:** Mengadopsi arsitektur *platform-agnostic*. Jalur pemrosesan transkripsi dan analisis AI dapat dengan cepat dialihkan ke model alternatif lainnya (seperti Llama 3 atau Mistral open-source) tanpa mengubah antarmuka aplikasi.

5.3.2 Risiko Persaingan Pasar & Retensi Pengguna
* **Ancaman:** Munculnya tiruan dari kompetitor besar seperti Duolingo atau Ruangguru yang mengadopsi format video pendek.
* **Mitigasi:** EduFlow mengandalkan spesialisasi Teknik Feynman suara aktif yang dikombinasikan dengan pelokalan silabus mata kuliah perguruan tinggi Indonesia serta harga premium yang sangat murah (Rp20.000/bulan). Pendekatan akar rumput melalui organisasi kemahasiswaan (BEM/Hima) dan program *Campus Ambassador* membangun efek jaringan (*network effects*) yang erat di kalangan mahasiswa, menciptakan benteng retensi yang sulit digoyahkan oleh kompetitor yang hanya bergantung pada iklan berbayar mahal.