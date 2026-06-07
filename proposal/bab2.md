BAB 2
DESKRIPSI PRODUK DAN TEKNOLOGI

2.1 Deskripsi Produk EduFlow
EduFlow adalah platform pembelajaran mikro digital berbasis kecerdasan buatan (AI) yang dirancang secara khusus untuk mentransformasikan perilaku konsumsi media digital pasif mahasiswa generasi Z menjadi aktivitas edukasi aktif yang terarah. Berbeda dengan platform EdTech konvensional yang menyajikan video pembelajaran berdurasi panjang (15 hingga 60 menit) yang rentan memicu kejenuhan kognitif (cognitive overload), EduFlow mengadopsi format umpan video pendek yang selaras dengan silabus dan kurikulum pendidikan tinggi. Melalui pendekatan navigasi ganda (Roadmap terstruktur dan Discovery eksploratif) serta sistem gamifikasi (XP, Streak, leveling, dan papan peringkat), platform ini berhasil mempertahankan minat belajar mahasiswa dengan memanfaatkan kebiasaan *doomscrolling* mereka menjadi pengalaman belajar yang produktif dan menyenangkan.

Keunggulan utama EduFlow terletak pada integrasi kognitif verbal aktif yang mengadopsi Teknik Feynman melalui fitur Checkpoint Voice Note. Sebagai syarat untuk membuka materi selanjutnya, sistem akan mewajibkan mahasiswa untuk menjelaskan kembali konsep yang baru dipelajari secara lisan dengan bahasa sehari-hari. Rekaman ini diproses secara asinkron menggunakan teknologi pemrosesan bahasa alami (NLP), yang diawali oleh transkripsi suara otomatis secara lokal menggunakan model Speech-to-Text (STT) berbasis Whisper open-source, dan dilanjutkan dengan analisis kontekstual Large Language Model (Gemini 2.5 Flash via OpenRouter) untuk mendeteksi kesenjangan pemahaman konseptual. Inovasi ini menyimulasikan kehadiran tutor privat interaktif secara massal dan terjangkau, memutus rantai ilusi pemahaman (*illusion of understanding*) yang sering terjadi pada pembelajaran satu arah, sekaligus melatih keterampilan komunikasi verbal mahasiswa secara sistematis.

2.2 Aplikasi Mobile Multi-Peran EduFlow (Mahasiswa dan Kreator)
Aplikasi mobile EduFlow bertindak sebagai pintu gerbang utama bagi seluruh ekosistem pengguna baik mahasiswa maupun konten kreator edukasi untuk berinteraksi langsung melalui ponsel berbasis Android maupun iOS. Aplikasi ini dirancang dengan antarmuka yang modern, ringan, dan super lancar saat digunakan memutar video pendek tanpa adanya jeda atau patah-patah. Sistem di dalam aplikasi mendeteksi jenis akun pengguna sejak tahap masuk (login), kemudian menyesuaikan tampilan menu dan hak akses secara otomatis sesuai peran masing-masing pengguna.

Untuk memberikan pengalaman terbaik yang disesuaikan dengan kebutuhan setiap pengguna, aplikasi mobile EduFlow membagi fitur-fiturnya berdasarkan dua peran utama:

2.2.1 Antarmuka Mahasiswa
Menyajikan pemutaran video pendek vertikal yang terstruktur agar mahasiswa dapat belajar bab demi bab secara terarah. Mahasiswa dibimbing melalui Roadmap belajar mandiri, merekam penjelasan suara mereka dengan visualisasi grafis yang interaktif, serta mendapatkan *feedback* kualitatif dan skor pemahaman langsung dari AI secara instan. Fitur motivasi seperti streak harian, poin pengalaman (XP), dan lencana pencapaian disajikan untuk menjaga konsistensi belajar mandiri.

2.2.2 Antarmuka Konten Kreator
Memberikan kendali penuh bagi kreator edukasi untuk mengelola materi ajar mereka langsung dari ponsel. Kreator dapat mengunggah video ajar baru sesuai panduan silabus dan kurikulum perguruan tinggi, memantau statistik penayangan dan tingkat penyelesaian video, melihat rata-rata nilai pemahaman mahasiswa yang menonton konten mereka, serta memantau dana insentif maupun apresiasi digital yang diperoleh secara transparan.

2.3 Website Admin EduFlow (Pusat Kendali Pengembang dan Kreator)
Website Admin EduFlow merupakan pusat kendali terpusat berbasis web yang diakses melalui browser komputer desktop untuk memfasilitasi kebutuhan manajemen tingkat lanjut bagi pihak pengembang (Developer/Administrator) serta konten kreator. Berbeda dengan aplikasi mobile yang dirancang khusus untuk kebutuhan interaksi yang cepat, web ini dirancang khusus untuk mempermudah pekerjaan administratif pada data menggunakan layar monitor komputer, seperti penyusunan materi silabus perkuliahan, analisis mendalam terhadap perkembangan belajar mahasiswa secara agregat, serta moderasi video pengajaran. Antarmuka sistem web dibuat sangat bersih, profesional, dan mudah dinavigasikan agar seluruh aktivitas pengelolaan platform dapat dipantau dari satu layar utama.

Untuk menjaga kualitas dan kelancaran operasional platform, website admin ini memiliki dua dashboard utama dengan fungsi sebagai berikut:

2.3.1 Dashboard Pengembang
Berfungsi sebagai pusat kendali utama platform yang mencakup:
a. Penyusunan Roadmap kurikulum: Fitur untuk merancang silabus, membuat topik mata pelajaran baru, dan menyusun urutan bab belajar resmi yang akan tampil di aplikasi ponsel mahasiswa.
b. Pusat Moderasi Konten: Halaman ulasan materi di mana tim pengelola dapat memeriksa video yang dikirim oleh kreator. Sistem ini dilengkapi asisten kecerdasan buatan (AI) yang mendeteksi kesesuaian video secara otomatis sebelum pengelola memberikan persetujuan akhir.
c. Manajemen Akun & Promo B2C: Fitur untuk mengelola akun pengguna, memantau keaktifan belajar mahasiswa secara umum, serta mengonfigurasi skema promosi langsung (B2C) seperti kode *referral* individu.

2.3.2 Dashboard Kreator Versi Desktop
Menyajikan laporan kinerja konten yang jauh lebih rinci untuk para pengajar. Kreator dapat menganalisis grafik naik-turunnya minat tonton mahasiswa di setiap detik video, melihat persentase jumlah mahasiswa yang menonton video hingga selesai, melihat sebaran nilai ujian suara mahasiswa per materi, serta membaca rangkuman kendala pemahaman mahasiswa agar kreator dapat menyempurnakan metode pengajaran pada materi video selanjutnya.

2.4 Integrasi Sistem dan Alur Kerja (System Integration & Workflow)
Mahasiswa menonton video belajar pendek dan merekam penjelasan konsep menggunakan suara mereka sendiri di aplikasi ponsel. Teknologi kecerdasan buatan (AI) secara otomatis mengubah rekaman suara kognitif mahasiswa menjadi teks transkrip, lalu menganalisis kedalaman pemahaman konseptual dan memberikan masukan belajar secara instan. Demi menjaga keamanan privasi data pengguna, file rekaman suara langsung dihapus dari sistem setelah proses penilaian selesai disimpan di database.

Setiap video pelajaran pendek yang diunggah oleh kreator disaring otomatis oleh AI untuk memastikan materi ajar sesuai dengan silabus perkuliahan dan bebas dari konten negatif. Video yang lolos kurasi kemudian didistribusikan ke ponsel mahasiswa. Sistem secara otomatis akan memprioritaskan penyebaran video-video yang terbukti paling sukses membantu mahasiswa memahami konteks dan konsep materi.
