# 📚 EduFlow — Platform Pembelajaran Berbasis AI

> Gabungan TikTok scrolling + Duolingo gamifikasi + Feynman Technique untuk verifikasi pemahaman siswa via voice note yang dianalisis AI.

---

## 🎯 Konsep & Visi

Platform pembelajaran di mana:
- **Developer** membuat roadmap kurikulum resmi
- **Content Creator** membuat video pendek per chapter (wajib sesuai roadmap)
- **Siswa** belajar via feed TikTok-style, diukur pemahamannya dengan voice note yang dianalisis AI

---

## 👥 Tiga Peran dalam Sistem

| Role | Akses | Tugas Utama |
|---|---|---|
| **Developer** | Dashboard developer | Buat & publish roadmap, review konten final |
| **Content Creator** | Dashboard creator | Upload video per chapter sesuai brief |
| **Siswa** | Feed belajar | Scroll konten, selesaikan chapter, rekam voice note |

> ⚠️ **Beta:** Semua role hanya bisa daftar dengan invite code dari developer.

---

## 📊 Diagram Alur Sistem

---

### Diagram 1 — Setup Developer + Moderasi Konten Creator

```mermaid
flowchart TD
    subgraph DEV [" — Setup Developer — "]
        A([Developer Login]) --> B[Buat Roadmap Belajar\nTopik, urutan chapter, kurikulum]
        B --> C[Tentukan Checkpoint\nSetelah chapter N → voice note]
        C --> D[Publish Roadmap ke Platform]
    end

    subgraph CREATOR [" — Alur Content Creator — "]
        E([Creator Daftar & Pilih Roadmap]) --> F[Lihat Brief Tiap Chapter]
        F --> G[Buat Konten per Chapter\nVideo pendek, TikTok-style]
        G --> H[Submit Konten ke Platform]
    end

    subgraph MODERASI [" — Moderasi Konten — "]
        I[Auto-review oleh AI\nCek kesesuaian roadmap]
        I --> J{Lolos Moderasi?}
        J -->|Tidak| K[Ditolak + Feedback ke Creator]
        K --> G
        J -->|Lolos| L[Review Manual Developer\nFinal approval sebelum live]
        L --> M([Konten Live di Platform])
    end

    D --> E
    H --> I

    style A fill:#3d2b8a,color:#fff
    style B fill:#3d2b8a,color:#fff
    style C fill:#3d2b8a,color:#fff
    style D fill:#3d2b8a,color:#fff
    style E fill:#2d6a4f,color:#fff
    style F fill:#2d6a4f,color:#fff
    style G fill:#2d6a4f,color:#fff
    style H fill:#2d6a4f,color:#fff
    style I fill:#8b1a1a,color:#fff
    style J fill:#444,color:#fff
    style K fill:#8b1a1a,color:#fff
    style L fill:#8b1a1a,color:#fff
    style M fill:#2d6a4f,color:#fff
```

---

### Diagram 2 — Alur Belajar Siswa

```mermaid
flowchart TD
    A([Daftar Akun]) --> B[Placement Test\nUkur level awal siswa]
    B --> C[Rekomendasi Roadmap\nSesuai hasil test]
    C --> D

    D[Scroll & Tonton Konten\nFeed vertikal TikTok-style] --> E[Selesaikan Chapter\nDapat XP, badge, streak naik]
    E --> EA[Update Analytics Siswa]
    E --> F{Sudah di Checkpoint?}

    F -->|Belum| D
    F -->|Ya| G[Rekam Voice Note\nJelaskan ulang materi chapter]

    G --> H[Whisper + LLM Scoring\nTranskripsi, analisa, skor]
    H --> I{Pemahaman Cukup?}

    I -->|Belum| J[Feedback + Ulang Rekam]
    J --> G
    I -->|Cukup| K[Lanjut Chapter Berikutnya\nXP naik, unlock konten baru]

    K --> KA[Simpan ke Dashboard Progress]
    K --> L[Sistem Notifikasi\nStreak reminder, XP milestone]
    L --> D

    style A fill:#3d2b8a,color:#fff
    style B fill:#3d2b8a,color:#fff
    style C fill:#3d2b8a,color:#fff
    style D fill:#7a4a1a,color:#fff
    style E fill:#7a4a1a,color:#fff
    style EA fill:#8b1a1a,color:#fff
    style F fill:#444,color:#fff
    style G fill:#7a4a1a,color:#fff
    style H fill:#8b1a1a,color:#fff
    style I fill:#444,color:#fff
    style J fill:#8b1a1a,color:#fff
    style K fill:#7a4a1a,color:#fff
    style KA fill:#8b1a1a,color:#fff
    style L fill:#3d2b8a,color:#fff
```

---

### Diagram 3 — Analytics & Dashboard

```mermaid
flowchart TD
    A[Data Terkumpul di Server\nVoice note, XP, chapter log] --> B[Proses & Agregasi\nHitung skor, gap konsep]
    B --> C([Tampilkan di Dashboard])
    C --> D[Dashboard Siswa\nProgress, skor, kelemahan]
    C --> E[Dashboard Developer\nEfektivitas roadmap, dropout rate]

    style A fill:#8b1a1a,color:#fff
    style B fill:#8b1a1a,color:#fff
    style C fill:#444,color:#fff
    style D fill:#3d2b8a,color:#fff
    style E fill:#3d2b8a,color:#fff
```

---

### Diagram 4 — Auth & Role System

```mermaid
flowchart TD
    A([Buka Aplikasi]) --> B{Sudah Punya Akun?}
    B -->|Belum| C[Daftar Baru]
    C --> D[Pilih Role saat Daftar]
    B -->|Ya| E[Login & Verifikasi Session]
    D --> F
    E --> F([Cek Role User])

    F --> G[Developer\nRoadmap builder, approve]
    F --> H[Content Creator\nUpload konten per chapter]
    F --> I[Siswa\nBelajar, voice note, XP]

    G --> J[Dashboard Developer]
    H --> K[Dashboard Creator]
    I --> L[Feed Belajar Siswa]

    style A fill:#444,color:#fff
    style B fill:#444,color:#fff
    style C fill:#444,color:#fff
    style D fill:#444,color:#fff
    style E fill:#444,color:#fff
    style F fill:#444,color:#fff
    style G fill:#3d2b8a,color:#fff
    style H fill:#2d6a4f,color:#fff
    style I fill:#7a4a1a,color:#fff
    style J fill:#3d2b8a,color:#fff
    style K fill:#2d6a4f,color:#fff
    style L fill:#7a4a1a,color:#fff
```

> 📌 **Beta:** Invite-only — hanya user dengan kode undangan yang bisa daftar.

---

### Diagram 5 — Error Handling Voice Note

```mermaid
flowchart TD
    A([Siswa Rekam Voice Note]) --> B[Upload ke Server]
    B --> C{Upload Berhasil?}
    C -->|Gagal| D[Gagal: Retry Otomatis]
    D --> B
    C -->|Berhasil| E[Whisper Transkripsi]

    E --> F{Audio Terdeteksi & Relevan?}
    F -->|Tidak| G[Tidak Valid\nMinta Rekam Ulang]
    G --> A
    F -->|Ya| H[LLM Scoring\nAnalisa pemahaman siswa]

    H --> HE[LLM Timeout?\nFallback ke Skor Manual]

    subgraph BETA [" — Beta Feedback — "]
        I[Tombol Laporkan Masalah\nTersedia di semua layar]
        I --> J[Log Dikirim ke Developer Dashboard]
    end

    H --> I

    style A fill:#7a4a1a,color:#fff
    style B fill:#8b1a1a,color:#fff
    style C fill:#444,color:#fff
    style D fill:#6b0000,color:#fff
    style E fill:#8b1a1a,color:#fff
    style F fill:#444,color:#fff
    style G fill:#6b0000,color:#fff
    style H fill:#8b1a1a,color:#fff
    style HE fill:#6b0000,color:#fff
    style I fill:#3d2b8a,color:#fff
    style J fill:#3d2b8a,color:#fff
```

---

### Diagram 6 — Creator Dashboard Detail

```mermaid
flowchart TD
    A([Creator Login]) --> B[Buka Dashboard Creator]
    B --> C[Lihat Daftar Konten yang Disubmit]
    C --> D{Status Konten?}

    D -->|Pending| E[Menunggu Review AI & Developer]
    D -->|Ditolak| F[Lihat Feedback AI + Developer]
    D -->|Live| G[Lihat Statistik Konten]

    F --> H[Edit & Perbaiki Konten]
    H --> I[Resubmit ke Platform]
    I --> E

    G --> J[Views per Chapter]
    G --> K[Completion Rate Siswa]
    G --> L[Rata-rata Skor Voice Note\nSiswa di Chapter Ini]

    style A fill:#2d6a4f,color:#fff
    style B fill:#2d6a4f,color:#fff
    style C fill:#2d6a4f,color:#fff
    style D fill:#444,color:#fff
    style E fill:#7a4a1a,color:#fff
    style F fill:#8b1a1a,color:#fff
    style G fill:#2d6a4f,color:#fff
    style H fill:#2d6a4f,color:#fff
    style I fill:#2d6a4f,color:#fff
    style J fill:#2d6a4f,color:#fff
    style K fill:#2d6a4f,color:#fff
    style L fill:#2d6a4f,color:#fff
```

---

### Diagram 7 — Roadmap Discovery untuk Siswa

```mermaid
flowchart TD
    A([Siswa Buka Halaman Explore]) --> B[Tampil Semua Roadmap Tersedia]
    B --> C{Filter Roadmap}
    C -->|By Topik| D[Roadmap Sesuai Topik]
    C -->|By Level| E[Roadmap Sesuai Level]
    C -->|Tanpa Filter| F[Semua Roadmap]

    D --> G[Klik Roadmap]
    E --> G
    F --> G

    G --> H[Halaman Detail Roadmap\nDeskripsi, Jumlah Chapter, Creator, Total Siswa]
    H --> I{Sudah Enrolled?}
    I -->|Sudah| J[Lanjutkan Belajar\ndi Chapter Terakhir]
    I -->|Belum| K[Tombol Enroll Roadmap]
    K --> L[Mulai dari Chapter 1\nMasuk ke Feed Belajar]

    style A fill:#6b4c8a,color:#fff
    style B fill:#6b4c8a,color:#fff
    style C fill:#444,color:#fff
    style D fill:#6b4c8a,color:#fff
    style E fill:#6b4c8a,color:#fff
    style F fill:#6b4c8a,color:#fff
    style G fill:#6b4c8a,color:#fff
    style H fill:#6b4c8a,color:#fff
    style I fill:#444,color:#fff
    style J fill:#6b4c8a,color:#fff
    style K fill:#6b4c8a,color:#fff
    style L fill:#6b4c8a,color:#fff
```

---

### Diagram 8 — Manajemen Invite Code

```mermaid
flowchart TD
    subgraph DEV [" — Sisi Developer — "]
        A([Developer Buka Dashboard]) --> B[Menu: Invite Code Management]
        B --> C[Generate Kode Baru]
        C --> D[Atur: Role Target, Expired, Maks Pengguna]
        D --> E[Kode Dibuat & Tersimpan]
        E --> F[Developer Share Kode ke Calon User]
        B --> G[Monitor Kode: Sudah Dipakai / Sisa]
    end

    subgraph USER [" — Sisi User Baru — "]
        H([User Buka Aplikasi]) --> I[Daftar Akun Baru]
        I --> J[Input Invite Code]
        J --> K{Validasi Kode}
        K -->|Tidak Valid / Expired| L[Error: Kode Tidak Valid]
        K -->|Valid| M[Pilih Role Sesuai Kode]
        M --> N[Akun Berhasil Dibuat]
        N --> O[Masuk ke Dashboard Sesuai Role]
    end

    F -.->|Kode dikirim| J

    style A fill:#3d2b8a,color:#fff
    style B fill:#3d2b8a,color:#fff
    style C fill:#3d2b8a,color:#fff
    style D fill:#3d2b8a,color:#fff
    style E fill:#3d2b8a,color:#fff
    style F fill:#3d2b8a,color:#fff
    style G fill:#3d2b8a,color:#fff
    style H fill:#555,color:#fff
    style I fill:#555,color:#fff
    style J fill:#555,color:#fff
    style K fill:#444,color:#fff
    style L fill:#8b1a1a,color:#fff
    style M fill:#555,color:#fff
    style N fill:#555,color:#fff
    style O fill:#555,color:#fff
```

---

### Diagram 9 — Content Selection Algorithm (TikTok-style)

> Satu chapter bisa diisi banyak creator. Algoritma memilih konten berdasarkan **skor pemahaman**, bukan sekadar engagement.

```mermaid
flowchart TD
    A([Chapter N: Beberapa Konten Creator]) --> B[Siswa Masuk ke Chapter N]
    B --> C{Sudah Ada Riwayat Belajar Siswa?}

    C -->|Belum - Cold Start| D[Tampilkan Konten dengan\nTayangan Terbanyak]
    C -->|Sudah Ada Data| E[Algoritma Hitung Content Score]

    E --> F["Skor Konten =
    70% × Avg Voice Note Score
    + 20% × Completion Rate
    + 10% × Rewatch Rate"]

    F --> G[Cek Profil Belajar Siswa\nVisual? Audio? Cepat? Lambat?]
    G --> H[Pilih Konten Skor Tertinggi\n+ Cocok dengan Profil Siswa]

    D --> I[Siswa Nonton Konten]
    H --> I

    I --> J[Kumpulkan Signal:\nCompletion Rate, Rewatch, Waktu Tonton]
    I --> L{Siswa Sampai Checkpoint?}
    L -->|Belum| I
    L -->|Ya| M[Siswa Rekam Voice Note]

    M --> N[Skor Voice Note dihitung LLM]
    N --> O[Update Content Score]
    O --> E

    subgraph FEEDBACK [" — Feedback ke Creator — "]
        P[Creator Lihat Performa Kontennya]
        P --> Q{Skor Bagus?}
        Q -->|Tinggi| R[Konten Makin Sering Ditampilkan]
        Q -->|Rendah| S[Notifikasi: Sarankan Perbaikan]
        S --> T[Creator Revisi & Resubmit]
    end

    O --> P

    style A fill:#444,color:#fff
    style B fill:#6b4c8a,color:#fff
    style C fill:#333,color:#fff
    style D fill:#6b4c8a,color:#fff
    style E fill:#8b1a1a,color:#fff
    style F fill:#8b1a1a,color:#fff
    style G fill:#8b1a1a,color:#fff
    style H fill:#8b1a1a,color:#fff
    style I fill:#6b4c8a,color:#fff
    style J fill:#8b1a1a,color:#fff
    style L fill:#333,color:#fff
    style M fill:#6b4c8a,color:#fff
    style N fill:#8b1a1a,color:#fff
    style O fill:#8b1a1a,color:#fff
    style P fill:#2d6a4f,color:#fff
    style Q fill:#333,color:#fff
    style R fill:#2d6a4f,color:#fff
    style S fill:#2d6a4f,color:#fff
    style T fill:#2d6a4f,color:#fff
```

#### 📊 Formula Content Score

| Bobot | Metrik | Keterangan |
|---|---|---|
| **70%** | Avg Voice Note Score | Seberapa paham siswa setelah nonton konten ini |
| **20%** | Completion Rate | Apakah siswa menonton sampai selesai |
| **10%** | Rewatch Rate | Apakah siswa menonton ulang |

> ⚠️ Algoritma hanya memilih **SIAPA creator** yang mengajarkan chapter. Urutan chapter tetap dikontrol developer.

---

## 🛠️ Tech Stack

### Backend
| Layer | Teknologi | Alasan |
|---|---|---|
| **Framework** | Laravel (PHP) | Sesuai environment Laragon, ekosistem lengkap |
| **Database** | MySQL | Sudah tersedia di Laragon |
| **Cache / Queue** | Redis | Async processing voice note & content scoring |
| **Auth** | Laravel Sanctum | API token untuk mobile |

### Frontend / Mobile
| Platform | Teknologi | Alasan |
|---|---|---|
| **Mobile App** | React Native | TikTok-style swipe, cross-platform iOS & Android |
| **Web Admin** | Next.js | Dashboard developer & creator |

### AI & Storage
| Kebutuhan | Teknologi | Alasan |
|---|---|---|
| **Speech-to-Text** | OpenAI Whisper API | Transkripsi voice note siswa |
| **LLM Scoring** | OpenAI GPT-4o | Analisa pemahaman dari transkrip |
| **Video Storage** | Cloudflare Stream / S3 | Streaming video TikTok-style |
| **Audio Storage** | AWS S3 | Simpan file voice note |

### DevOps (Beta)
| Kebutuhan | Teknologi |
|---|---|
| **Server** | VPS (DigitalOcean / Hetzner) |
| **Containerisasi** | Docker |
| **CI/CD** | GitHub Actions |

---

## 🎯 Scope Beta

### ✅ Fitur Wajib Ada
- [ ] Auth & invite code system
- [ ] Developer: buat & publish roadmap + chapter
- [ ] Creator: upload video per chapter + melihat status konten
- [ ] Moderasi konten: AI review + developer approval
- [ ] Siswa: enroll roadmap, feed TikTok-style, XP & streak
- [ ] Checkpoint voice note + Whisper + LLM scoring
- [ ] Roadmap discovery (browse + filter)
- [ ] Creator dashboard (status + statistik basic)
- [ ] Error handling voice note (retry, fallback)
- [ ] Content selection algorithm (TikTok-style)
- [ ] Analytics dashboard (siswa & developer)

### ❌ Skip untuk Beta
- Monetisasi / payment
- Social / komentar
- Offline mode
- Roadmap versioning
- Placement test detail

---

### Diagram 10 — Dual Navigation Mode (Roadmap vs Discover)

> **Konsep:** Bottom navigation bar dibagi 2 mode utama.
> Mirip TikTok "Following" vs "For You" — siswa bisa switch antara belajar terstruktur dan konten acak saat bosan.

```mermaid
flowchart TD
    A([Siswa Buka App]) --> B[Bottom Navigation Bar]

    B --> C[\ud83d\udcda Tab: ROADMAP\nBelajar Terstruktur]
    B --> D[\ud83d\udd25 Tab: DISCOVER\nKonten Random]

    subgraph ROADMAP_MODE [" \u2014 Mode Roadmap (Terstruktur) \u2014 "]
        C --> C1[Lanjutkan dari Chapter Terakhir\nUrutan wajib sesuai roadmap]
        C1 --> C2[Tonton Video Chapter N]
        C2 --> C3[Selesaikan Chapter\nDapat XP + Streak]
        C3 --> C4{Sudah di Checkpoint?}
        C4 -->|Belum| C2
        C4 -->|Ya| C5[Wajib Voice Note\nSebelum Lanjut Chapter]
        C5 --> C6[Unlock Chapter Berikutnya]
    end

    subgraph DISCOVER_MODE [" \u2014 Mode Discover (Santai) \u2014 "]
        D --> D1[Algoritma Pilih Konten Acak\nDari Semua Roadmap Published]
        D1 --> D2[Filter Otomatis:\nSesuai Level Siswa]
        D2 --> D3[Tonton Video Apapun\nSwipe Up untuk Skip]
        D3 --> D4[Dapat XP Kecil\nTanpa Checkpoint]
        D4 --> D5{Tertarik dengan\nTopik Ini?}
        D5 -->|Ya| D6[Tombol: Lihat Roadmap Ini\nLangsung ke Detail Roadmap]
        D5 -->|Tidak| D3
        D6 --> D7[Enroll Roadmap Baru\nLanjut di Tab Roadmap]
    end

    C5 -.->|Bosan? Switch mode| D
    D -.->|Mau lanjut belajar?| C

    style A fill:#444,color:#fff
    style B fill:#333,color:#fff
    style C fill:#3d2b8a,color:#fff
    style D fill:#7a4a1a,color:#fff
    style C1 fill:#3d2b8a,color:#fff
    style C2 fill:#3d2b8a,color:#fff
    style C3 fill:#3d2b8a,color:#fff
    style C4 fill:#333,color:#fff
    style C5 fill:#8b1a1a,color:#fff
    style C6 fill:#3d2b8a,color:#fff
    style D1 fill:#7a4a1a,color:#fff
    style D2 fill:#8b1a1a,color:#fff
    style D3 fill:#7a4a1a,color:#fff
    style D4 fill:#7a4a1a,color:#fff
    style D5 fill:#333,color:#fff
    style D6 fill:#2d6a4f,color:#fff
    style D7 fill:#2d6a4f,color:#fff
```

#### 🔑 Perbedaan Utama Dua Mode

| | Tab Roadmap 📚 | Tab Discover 🔥 |
|---|---|---|
| **Urutan konten** | Wajib berurutan (bab 1, 2, 3...) | Acak, bebas |
| **Voice note** | ✅ Wajib di checkpoint | ❌ Tidak ada |
| **XP** | Besar (50 XP/chapter) | Kecil (5 XP/video) |
| **Tujuan** | Selesaikan kurikulum | Eksplorasi & refreshing |
| **Bisa enroll baru?** | Tidak langsung | ✅ Dari tombol di video |
| **Algoritma** | Urutan ditentukan developer | TikTok-style content score |

#### 💡 Kenapa Ini Bagus untuk Retention

```
Tanpa Discover mode:
  Siswa bosan dengan roadmap → keluar dari app → churn

Dengan Discover mode:
  Siswa bosan dengan roadmap
    → switch ke Discover
    → nonton video santai
    → mungkin tertarik topik baru
    → enroll roadmap baru
    → kembali ke Tab Roadmap
    → tetap aktif di platform ✅
```

> ⚠️ **Catatan Implementasi:**
> - Konten di Discover **hanya dari roadmap yang sudah published**
> - Filter berdasarkan level siswa (hasil placement test)
> - **Tidak menggantikan** checkpoint voice note di Tab Roadmap
> - Streak hanya naik kalau belajar di **Tab Roadmap**

---

> 📄 **ERD Database** → lihat file `ERD.md`
