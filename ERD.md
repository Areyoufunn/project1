# 🗄️ ERD — EduFlow Database Design

> Desain database untuk platform pembelajaran EduFlow.
> Dibuat berdasarkan 9 diagram alur sistem.

---

## 📋 Daftar Tabel

| Tabel | Deskripsi |
|---|---|
| `users` | Semua pengguna (developer, creator, siswa) |
| `invite_codes` | Kode undangan untuk registrasi beta |
| `roadmaps` | Kurikulum/roadmap yang dibuat developer |
| `chapters` | Setiap chapter dalam roadmap |
| `contents` | Video konten per chapter dari creator |
| `content_scores` | Skor algoritma TikTok per konten |
| `content_views` | Tracking tonton siswa per konten |
| `enrollments` | Siswa yang enroll ke roadmap |
| `chapter_progress` | Progress siswa per chapter |
| `voice_notes` | Rekaman voice note siswa |
| `student_analytics` | XP, streak, statistik keseluruhan siswa |

---

## 🔗 ERD Diagram

```mermaid
erDiagram
    users {
        int id PK
        string name
        string email
        string password
        enum role "developer | creator | student"
        int invite_code_id FK
        timestamp created_at
        timestamp updated_at
    }

    invite_codes {
        int id PK
        string code UK
        enum role_target "developer | creator | student"
        int created_by FK
        int max_uses
        int used_count
        boolean is_active
        timestamp expires_at
        timestamp created_at
    }

    roadmaps {
        int id PK
        string title
        text description
        string topic
        enum level "beginner | intermediate | advanced"
        int developer_id FK
        enum status "draft | published"
        timestamp created_at
        timestamp updated_at
    }

    chapters {
        int id PK
        int roadmap_id FK
        string title
        text brief
        int order_number
        boolean is_checkpoint
        timestamp created_at
        timestamp updated_at
    }

    contents {
        int id PK
        int chapter_id FK
        int creator_id FK
        string title
        string video_url
        int duration_seconds
        enum status "pending | ai_review | rejected | live"
        float ai_review_score
        text ai_review_feedback
        timestamp developer_approved_at
        timestamp created_at
        timestamp updated_at
    }

    content_scores {
        int id PK
        int content_id FK
        float avg_voice_note_score
        float completion_rate
        float rewatch_rate
        float total_score
        int total_views
        timestamp updated_at
    }

    content_views {
        int id PK
        int content_id FK
        int student_id FK
        int watch_duration_seconds
        float completion_rate
        int rewatch_count
        timestamp last_watched_at
    }

    enrollments {
        int id PK
        int student_id FK
        int roadmap_id FK
        int last_chapter_id FK
        timestamp enrolled_at
    }

    chapter_progress {
        int id PK
        int student_id FK
        int chapter_id FK
        int content_id FK
        boolean is_completed
        int xp_earned
        timestamp completed_at
    }

    voice_notes {
        int id PK
        int student_id FK
        int chapter_id FK
        string audio_url
        text transcript
        float llm_score
        text llm_feedback
        int attempt_number
        enum status "uploading | processing | scored | failed"
        timestamp created_at
    }

    student_analytics {
        int id PK
        int student_id FK
        int total_xp
        int current_streak
        int longest_streak
        int total_chapters_completed
        float avg_voice_note_score
        json weak_topics
        timestamp updated_at
    }

    users ||--o{ roadmaps : "developer membuat"
    users ||--o{ contents : "creator membuat"
    users ||--o{ enrollments : "siswa enroll"
    users ||--o{ chapter_progress : "siswa progress"
    users ||--o{ voice_notes : "siswa rekam"
    users ||--|| student_analytics : "siswa punya"
    users ||--o{ content_views : "siswa tonton"
    users ||--o{ invite_codes : "developer generate"
    users }o--|| invite_codes : "daftar pakai"

    roadmaps ||--o{ chapters : "punya chapter"
    roadmaps ||--o{ enrollments : "diikuti siswa"

    chapters ||--o{ contents : "punya banyak konten"
    chapters ||--o{ chapter_progress : "progress per chapter"
    chapters ||--o{ voice_notes : "voice note per chapter"

    contents ||--|| content_scores : "punya skor algoritma"
    contents ||--o{ content_views : "ditonton siswa"
    contents ||--o{ chapter_progress : "konten yang ditonton"
```

---

## 📐 Detail Tabel

### `users`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `name` | VARCHAR(100) | Nama lengkap |
| `email` | VARCHAR(100) UNIQUE | Email login |
| `password` | VARCHAR(255) | Hashed bcrypt |
| `role` | ENUM | `developer`, `creator`, `student` |
| `invite_code_id` | INT FK NULL | Kode undangan yang dipakai saat daftar |
| `created_at` | TIMESTAMP | Waktu registrasi |
| `updated_at` | TIMESTAMP | Waktu terakhir update |

---

### `invite_codes`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `code` | VARCHAR(20) UNIQUE | Kode unik |
| `role_target` | ENUM | Role yang diberikan saat pakai kode ini |
| `created_by` | INT FK | Developer yang generate |
| `max_uses` | INT | Maksimal pemakaian (0 = unlimited) |
| `used_count` | INT | Sudah dipakai berapa kali |
| `is_active` | BOOLEAN | Status aktif kode (default: `true`) — gunakan ini untuk "nonaktifkan", jangan hard delete |
| `expires_at` | TIMESTAMP NULL | Batas waktu berlaku (null = tidak ada batas) |
| `created_at` | TIMESTAMP | Waktu dibuat |

---

### `roadmaps`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `title` | VARCHAR(150) | Judul roadmap |
| `description` | TEXT | Deskripsi roadmap |
| `topic` | VARCHAR(100) | Topik (programming, design, dll) |
| `level` | ENUM | `beginner`, `intermediate`, `advanced` |
| `developer_id` | INT FK | Developer pembuat |
| `status` | ENUM | `draft`, `published` |
| `created_at` | TIMESTAMP | Waktu dibuat |
| `updated_at` | TIMESTAMP | Waktu terakhir diedit |

---

### `chapters`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `roadmap_id` | INT FK | Roadmap induk |
| `title` | VARCHAR(150) | Judul chapter |
| `brief` | TEXT | Brief untuk creator (apa yang harus diajarkan) |
| `order_number` | INT | Urutan chapter dalam roadmap |
| `is_checkpoint` | BOOLEAN | Apakah chapter ini wajib voice note |
| `created_at` | TIMESTAMP | Waktu dibuat |
| `updated_at` | TIMESTAMP | Waktu terakhir diedit |

---

### `contents`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `chapter_id` | INT FK | Chapter yang dituju |
| `creator_id` | INT FK | Creator pembuat |
| `title` | VARCHAR(150) | Judul video |
| `video_url` | VARCHAR(255) | URL video (Cloudflare/S3) |
| `duration_seconds` | INT | Durasi video |
| `status` | ENUM | `pending`, `ai_review`, `rejected`, `live` |
| `ai_review_score` | FLOAT NULL | Skor dari AI moderasi |
| `ai_review_feedback` | TEXT NULL | Feedback AI jika ditolak |
| `developer_approved_at` | TIMESTAMP NULL | Waktu approval developer |
| `created_at` | TIMESTAMP | Waktu upload |
| `updated_at` | TIMESTAMP | Waktu terakhir update status |

---

### `content_scores` *(Algoritma TikTok)*
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `content_id` | INT FK UNIQUE | Relasi 1-to-1 dengan contents |
| `avg_voice_note_score` | FLOAT | Rata-rata skor voice note siswa (0–100) |
| `completion_rate` | FLOAT | Rata-rata siswa nonton sampai selesai (0–1) |
| `rewatch_rate` | FLOAT | Rata-rata siswa nonton ulang (0–1) |
| `total_score` | FLOAT | `70%×VN + 20%×CR + 10%×RR` |
| `total_views` | INT | Total tayangan |
| `updated_at` | TIMESTAMP | Terakhir diupdate |

---

### `content_views`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `content_id` | INT FK | Konten yang ditonton |
| `student_id` | INT FK | Siswa yang menonton |
| `watch_duration_seconds` | INT | Total detik yang benar-benar ditonton |
| `completion_rate` | FLOAT | Seberapa jauh siswa nonton (0–1) |
| `rewatch_count` | INT | Berapa kali nonton ulang (default: 0) |
| `last_watched_at` | TIMESTAMP | Terakhir ditonton |

> **Unique Constraint:** `UNIQUE(student_id, content_id)` — 1 row per siswa per konten. Saat nonton ulang, row di-UPDATE (bukan INSERT baru). `rewatch_count` di-increment.

---

### `enrollments`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `student_id` | INT FK | Siswa yang enroll |
| `roadmap_id` | INT FK | Roadmap yang diikuti |
| `last_chapter_id` | INT FK NULL | Chapter terakhir yang diakses (cache progress, nullable saat baru enroll) |
| `enrolled_at` | TIMESTAMP | Waktu enroll |

> **Unique Constraint:** `UNIQUE(student_id, roadmap_id)` — satu siswa hanya bisa enroll satu kali per roadmap.

---

### `chapter_progress`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `student_id` | INT FK | Siswa yang progress |
| `chapter_id` | INT FK | Chapter yang progres |
| `content_id` | INT FK NULL | Konten terakhir yang ditonton |
| `is_completed` | BOOLEAN | Status selesai |
| `xp_earned` | INT | XP didapat |
| `completed_at` | TIMESTAMP | Waktu selesai |

> **Unique Constraint:** `UNIQUE(student_id, chapter_id)` — satu row per siswa per chapter.

---

### `voice_notes`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `student_id` | INT FK | Siswa yang merekam |
| `chapter_id` | INT FK | Chapter yang dijelaskan |
| `audio_url` | VARCHAR(255) | URL file audio di S3 |
| `transcript` | TEXT NULL | Hasil transkripsi Whisper (null saat masih processing) |
| `llm_score` | FLOAT NULL | Skor pemahaman dari LLM 0–100 (null saat masih processing) |
| `llm_feedback` | TEXT NULL | Feedback dari LLM (null saat masih processing) |
| `attempt_number` | TINYINT | Percobaan ke-berapa (default: 1) |
| `status` | ENUM | `uploading`, `processing`, `scored`, `failed` |
| `created_at` | TIMESTAMP | Waktu rekam |

> **Unique Constraint:** `UNIQUE(student_id, chapter_id, attempt_number)` — mencegah duplikat upload untuk attempt yang sama.

---

### `student_analytics`
| Kolom | Tipe | Keterangan |
|---|---|---|
| `id` | INT PK | Primary key |
| `student_id` | INT FK UNIQUE | Relasi 1-to-1 dengan users |
| `total_xp` | INT | Total XP yang dikumpulkan (default: 0) |
| `current_streak` | INT | Streak belajar saat ini (hari, default: 0) |
| `longest_streak` | INT | Streak terpanjang yang pernah dicapai (default: 0) |
| `total_chapters_completed` | INT | Total chapter yang diselesaikan (default: 0) |
| `avg_voice_note_score` | FLOAT | Rata-rata skor voice note keseluruhan (default: 0) |
| `weak_topics` | JSON NULL | Array topik yang perlu diperkuat, diisi dari analisis LLM (contoh: `["CSS Flexbox", "Async/Await"]`) |
| `updated_at` | TIMESTAMP | Terakhir diupdate |

---

## 🔑 Catatan Desain

> **1 Chapter → Banyak Content (Multiple Creator)**
> Relasi `chapters` → `contents` adalah **one-to-many**.
> Algoritma di `content_scores` yang menentukan konten mana yang ditampilkan ke siswa tertentu.

> **Voice Note = Kunci Unlock**
> Siswa hanya bisa lanjut chapter berikutnya setelah `voice_notes.llm_score` mencapai threshold yang ditentukan developer di checkpoint.

> **Idempotent Analytics**
> `student_analytics` di-update setiap kali siswa menyelesaikan chapter — bukan di-insert baru.

> **Soft Delete untuk Invite Code**
> Jangan hard delete row `invite_codes`. Gunakan `is_active = false` untuk menonaktifkan kode agar FK di `users.invite_code_id` tidak broken.

> **Unique Constraints Penting**
> - `content_views`: `UNIQUE(student_id, content_id)` — rewatch = UPDATE bukan INSERT
> - `enrollments`: `UNIQUE(student_id, roadmap_id)` — satu siswa satu kali enroll per roadmap
> - `chapter_progress`: `UNIQUE(student_id, chapter_id)` — satu row per siswa per chapter
> - `voice_notes`: `UNIQUE(student_id, chapter_id, attempt_number)` — cegah duplikat upload

> **Kolom Nullable**
> Kolom yang ditandai `NULL` wajib nullable di migration. Kolom score/feedback/transcript bersifat nullable karena diisi setelah proses async selesai.
