# 🛠️ TECH STACK — EduFlow

> Dokumen ini menjelaskan bahasa pemrograman, framework, tools,
> dan struktur file lengkap untuk project EduFlow.

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────┐
│                    CLIENTS                          │
│  📱 Mobile (Flutter)       🌐 Web Admin (Next.js)  │
└─────────────────┬───────────────────┬───────────────┘
                  │   REST API        │
┌─────────────────▼───────────────────▼───────────────┐
│              BACKEND — Laravel 11 (PHP 8.3)         │
│  Auth │ Roadmap │ Content │ VoiceNote │ Analytics   │
└───────┬─────────────────────────────────────────────┘
        │ Queue (Redis)
┌───────▼─────────────────────────────────────────────┐
│              AI PROCESSING LAYER (Optimized)        │
│  Self-Hosted faster-whisper (STT, Rp 0)             │
│  + Gemini 1.5 Flash via OpenRouter (scoring, Rp 97) │
│  Audio file → DELETE INSTANTLY setelah transkripsi  │
└─────────────────────────────────────────────────────┘
        │
┌───────▼─────────────────────────────────────────────┐
│              STORAGE LAYER                          │
│  MySQL (data) │ Redis (cache/queue) │ CF Stream (video) │
│  ⚠️ Audio TIDAK disimpan — dihapus instan (privasi) │
└─────────────────────────────────────────────────────┘
```

---

## 📦 Stack Per Layer

### 🔴 Backend — Laravel 11 (PHP 8.3)

| Komponen | Teknologi | Versi |
|---|---|---|
| Framework | Laravel | 11.x |
| Language | PHP | 8.3 |
| Auth | Laravel Sanctum | latest |
| Queue | Laravel Queue + Redis | latest |
| ORM | Eloquent | built-in |
| Validation | Laravel FormRequest | built-in |
| AI - STT | `faster-whisper` (via Python subprocess / dedicated worker) | latest |
| AI - LLM | OpenRouter PHP (HTTP client ke openrouter.ai) | latest |
| Cache | predis/predis (Redis) | latest |

**Kenapa Laravel?**
- ✅ Sudah tersedia di Laragon — langsung pakai
- ✅ AI (Copilot/Cursor) punya training data Laravel terbanyak = kode paling akurat
- ✅ Built-in Queue untuk proses async Whisper + LLM
- ✅ Eloquent ORM sangat readable dan AI-friendly
- ✅ Komunitas terbesar, dokumentasi terlengkap

---

### 🔵 Mobile App — Flutter (Dart)

| Komponen | Teknologi | Versi |
|---|---|---|
| Framework | Flutter | 3.x (stable) |
| Language | Dart | 3.x |
| State Management | Riverpod | latest |
| HTTP Client | Dio | latest |
| Video Player | video_player + chewie | latest |
| Audio Record | record | latest |
| Auth Token Storage | flutter_secure_storage | latest |
| Animasi Lottie | lottie | latest |
| Notifikasi | flutter_local_notifications | latest |
| Image Caching | cached_network_image | latest |
| Navigation | go_router | latest |

**Kenapa Flutter?**
- ✅ **Performa terbaik** untuk TikTok-style feed — animasi 60fps smooth
- ✅ **Dart = sangat AI-friendly**, syntax bersih dan mudah di-generate
- ✅ `record` package = voice recording native di Android & iOS
- ✅ `video_player` = video playback performa tinggi
- ✅ `Riverpod` = state management modern, testable
- ✅ Animasi XP, badge, waveform jauh lebih smooth vs React Native
- ✅ Satu codebase → Android + iOS sekaligus

---

### 🟢 Web Admin — Next.js 14 (TypeScript)

| Komponen | Teknologi | Versi |
|---|---|---|
| Framework | Next.js | 14.x |
| Language | TypeScript | 5.x |
| CSS | Tailwind CSS | 3.x |
| HTTP Client | Axios | latest |
| Charts | Recharts | latest |
| Form | react-hook-form + zod | latest |
| UI Components | Radix UI | latest |
| Icons | lucide-react | latest |
| Auth Storage | js-cookie | latest |

**Kenapa Next.js (bukan Flutter Web)?**
- ✅ Flutter Web tidak ideal untuk tabel data & charts kompleks
- ✅ App Router = modern, performa optimal
- ✅ Server components untuk dashboard data-heavy
- ✅ Ekosistem chart (Recharts) lebih mature untuk admin dashboard

---

### 🗄️ Database & Infrastructure

| Layer | Teknologi | Fungsi | Biaya |
|---|---|---|---|
| **Database Utama** | MySQL 8.0 | Semua data aplikasi | Termasuk VPS |
| **Cache & Queue** | Redis | Session, queue job async | Termasuk VPS / Upstash free tier |
| **Video Storage** | Cloudflare Stream | Upload & streaming video konten | $1/1.000 mnt delivered |
| **Audio Storage** | ~~AWS S3~~ → **Tidak ada** | Audio **dihapus instan** setelah ditranskripsi | **Rp 0** |
| **AI Transkripsi** | **Self-Hosted faster-whisper** (model `base`/`tiny`) | Speech-to-text voice note tanpa biaya API | **Rp 0** |
| **AI Scoring** | **Gemini 1.5 Flash via OpenRouter** | Analisa pemahaman dari transkrip lisan | ~Rp 97/evaluasi |
| **Runtime** | Laragon (local) / VPS Linux (production) | — | — |

> 💡 **Mengapa bukan OpenAI?** Self-Hosted Whisper + Gemini Flash menghemat **95% biaya AI** dibanding OpenAI Whisper API + GPT-4o, dengan performa setara untuk kasus penggunaan evaluasi suara pendek (30-120 detik).

---

## 📁 Struktur File Lengkap

### Backend — Laravel

```
backend/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Auth/
│   │   │   │   └── AuthController.php
│   │   │   ├── Developer/
│   │   │   │   ├── RoadmapController.php
│   │   │   │   ├── ChapterController.php
│   │   │   │   ├── ContentModerationController.php
│   │   │   │   └── InviteCodeController.php
│   │   │   ├── Creator/
│   │   │   │   ├── ContentController.php
│   │   │   │   └── ContentAnalyticsController.php
│   │   │   └── Student/
│   │   │       ├── FeedController.php
│   │   │       ├── EnrollmentController.php
│   │   │       ├── VoiceNoteController.php
│   │   │       └── StudentAnalyticsController.php
│   │   │
│   │   ├── Middleware/
│   │   │   └── RoleMiddleware.php        ← Cek role: developer/creator/student
│   │   │
│   │   └── Requests/                     ← Validasi input
│   │       ├── Auth/
│   │       │   ├── RegisterRequest.php
│   │       │   └── LoginRequest.php
│   │       ├── Developer/
│   │       │   ├── StoreRoadmapRequest.php
│   │       │   └── StoreChapterRequest.php
│   │       └── Student/
│   │           ├── StoreVoiceNoteRequest.php
│   │           └── CompleteChapterRequest.php
│   │
│   ├── Models/                           ← Eloquent Models
│   │   ├── User.php
│   │   ├── InviteCode.php
│   │   ├── Roadmap.php
│   │   ├── Chapter.php
│   │   ├── Content.php
│   │   ├── ContentScore.php
│   │   ├── ContentView.php
│   │   ├── Enrollment.php
│   │   ├── ChapterProgress.php
│   │   ├── VoiceNote.php
│   │   └── StudentAnalytic.php
│   │
│   ├── Services/                         ← Business Logic (bukan di Controller!)
│   │   ├── AI/
│   │   │   ├── WhisperService.php        ← Transkripsi via faster-whisper (Self-Hosted/RunPod)
│   │   │   └── LLMScoringService.php     ← Evaluasi kognitif via Gemini 1.5 Flash (OpenRouter)
│   │   ├── ContentScoreService.php       ← Hitung algoritma TikTok
│   │   ├── VoiceNoteService.php          ← Orchestrate proses voice note (Hapus instan setelah transkripsi)
│   │   ├── ContentModerationService.php  ← AI review konten creator
│   │   └── StudentXPService.php          ← Hitung XP & streak
│   │
│   └── Jobs/                             ← Async Queue Jobs
│       ├── ProcessWhisperTranscription.php   ← Job 1: Transkripsi audio
│       └── ProcessLLMScoring.php             ← Job 2: Scoring dari transkrip
│
├── database/
│   ├── migrations/                       ← Satu file per tabel
│   │   ├── 2024_01_01_000001_create_users_table.php
│   │   ├── 2024_01_01_000002_create_invite_codes_table.php
│   │   ├── 2024_01_01_000003_create_roadmaps_table.php
│   │   ├── 2024_01_01_000004_create_chapters_table.php
│   │   ├── 2024_01_01_000005_create_contents_table.php
│   │   ├── 2024_01_01_000006_create_content_scores_table.php
│   │   ├── 2024_01_01_000007_create_content_views_table.php
│   │   ├── 2024_01_01_000008_create_enrollments_table.php
│   │   ├── 2024_01_01_000009_create_chapter_progress_table.php
│   │   ├── 2024_01_01_000010_create_voice_notes_table.php
│   │   └── 2024_01_01_000011_create_student_analytics_table.php
│   └── seeders/
│       ├── DatabaseSeeder.php
│       ├── UserSeeder.php               ← 1 developer + beberapa invite code
│       └── RoadmapSeeder.php            ← Data contoh roadmap
│
└── routes/
    └── api.php                          ← Semua route API dikelompokkan per role
```

---

### Mobile — Flutter

```
mobile/
├── lib/
│   ├── main.dart                        ← Entry point app
│   │
│   ├── screens/                         ← Satu folder per fitur
│   │   ├── auth/
│   │   │   ├── login_screen.dart
│   │   │   └── register_screen.dart     ← Input invite code di sini
│   │   └── student/
│   │       ├── feed_screen.dart         ← TikTok-style video feed (PageView vertikal)
│   │       ├── explore_screen.dart      ← Browse & filter roadmap
│   │       ├── roadmap_detail_screen.dart ← Detail + tombol enroll
│   │       ├── voice_note_screen.dart   ← Record voice note + waveform
│   │       ├── voice_note_result_screen.dart ← Skor + feedback LLM
│   │       └── progress_screen.dart    ← XP, streak, analytics diri
│   │
│   ├── widgets/                         ← Widget reusable
│   │   ├── video_player_widget.dart     ← Wrapper video_player + chewie
│   │   ├── xp_counter_widget.dart      ← Tampilan XP + animasi Lottie
│   │   ├── voice_waveform_widget.dart  ← Animasi waveform custom painter
│   │   ├── roadmap_card.dart           ← Card di explore screen
│   │   ├── chapter_progress_bar.dart   ← Progress bar chapter
│   │   └── score_badge.dart            ← Badge skor voice note
│   │
│   ├── services/                        ← Semua pemanggilan API (Dio)
│   │   ├── api_service.dart            ← Dio instance + interceptor auth
│   │   ├── auth_service.dart
│   │   ├── roadmap_service.dart
│   │   ├── feed_service.dart
│   │   └── voice_note_service.dart
│   │
│   ├── providers/                       ← State management (Riverpod)
│   │   ├── auth_provider.dart          ← State login/logout
│   │   ├── feed_provider.dart          ← State feed chapter
│   │   ├── voice_note_provider.dart    ← State rekam & polling
│   │   └── enrollment_provider.dart   ← State roadmap yang diikuti
│   │
│   ├── models/                          ← Data models (dari JSON API)
│   │   ├── user.dart
│   │   ├── roadmap.dart
│   │   ├── chapter.dart
│   │   ├── content.dart
│   │   └── voice_note.dart
│   │
│   └── config/
│       ├── api_config.dart             ← BASE_URL
│       ├── theme.dart                  ← Warna EduFlow: BG #0A1931 | Button #1A3D63 | Accent #4A7FA7 | Logo #B3CFE5 | Surface #F6FAFD
│       └── router.dart                 ← go_router navigation
│
├── pubspec.yaml                         ← Dependencies Flutter
├── android/                             ← Android native config
└── ios/                                 ← iOS native config
```

---

### Web Admin — Next.js

```
admin/
├── app/
│   ├── (auth)/
│   │   └── login/
│   │       └── page.tsx
│   │
│   ├── (developer)/                     ← Route group khusus developer
│   │   ├── layout.tsx                   ← Sidebar developer
│   │   ├── page.tsx                     ← Dashboard overview
│   │   ├── roadmaps/
│   │   │   ├── page.tsx                 ← List roadmap
│   │   │   ├── create/page.tsx          ← Buat roadmap baru
│   │   │   └── [id]/
│   │   │       ├── page.tsx             ← Detail roadmap
│   │   │       └── chapters/page.tsx    ← Manage chapter
│   │   ├── contents-review/
│   │   │   └── page.tsx                 ← Review & approve/reject konten
│   │   ├── invite-codes/
│   │   │   └── page.tsx                 ← Generate & monitor invite code
│   │   └── analytics/
│   │       └── page.tsx                 ← Analytics roadmap + dropout
│   │
│   └── (creator)/                       ← Route group khusus creator
│       ├── layout.tsx                   ← Sidebar creator
│       ├── page.tsx                     ← Dashboard overview
│       ├── contents/
│       │   ├── page.tsx                 ← List konten + status
│       │   └── upload/page.tsx          ← Upload video baru
│       └── analytics/
│           └── page.tsx                 ← Stats views, completion, VN score
│
├── components/
│   ├── ui/                              ← Komponen dasar (Button, Badge, Card)
│   ├── tables/
│   │   ├── ContentReviewTable.tsx
│   │   └── InviteCodeTable.tsx
│   ├── charts/
│   │   ├── DropoutChart.tsx
│   │   └── ScoreChart.tsx
│   └── layout/
│       ├── DeveloperSidebar.tsx
│       └── CreatorSidebar.tsx
│
├── lib/
│   ├── api.ts                           ← Axios instance
│   ├── auth.ts                          ← Auth helpers
│   └── utils.ts                         ← Helper functions
│
├── types/                               ← TypeScript interfaces (bisa share dengan mobile)
│   ├── User.ts
│   ├── Roadmap.ts
│   └── Content.ts
│
└── tsconfig.json
```

---

## 🚀 Alur Data End-to-End

```
[VOICE NOTE FLOW - ASYNC & OPTIMIZED]

Mobile                  Backend               Queue Worker
  │                        │                       │
  ├─ POST /voice-notes ───►│                       │
  │                        ├── Simpan Lokal Temp   │
  │                        ├── Status: "processing"│
  │                        ├── Dispatch Job ────────►│
  │◄── { id, "processing" }│                       ├── faster-whisper (STT)
  │                        │                       ├── Transkrip Selesai
  ├─ GET /voice-notes/5 ──►│                       ├── Gemini 1.5 Flash (Scoring)
  │◄── { "processing" }    │                       ├── Update DB & DELETES Audio
  │                        │◄── Update status ─────┤
  ├─ GET /voice-notes/5 ──►│    "scored"           │
  │◄── { score: 82,        │                       │
  │     feedback: "..." }  │                       │
  │                        │                       │
```

---

## 💡 Kenapa Stack Ini Optimal untuk Vibe Developer

| Alasan | Detail |
|---|---|
| **Laravel = AI favorite** | Model AI punya training data Laravel terbanyak → kode generated lebih akurat |
| **Flutter = UI premium** | Animasi smooth 60fps, TikTok-style feed paling mulus dibanding framework lain |
| **Dart = AI-friendly** | Syntax Dart bersih & terstruktur → AI generate kode lebih akurat |
| **Riverpod** | State management modern, mudah di-prompt ke AI per provider |
| **Struktur modular** | Tiap fitur terpisah → AI bisa fokus per file tanpa context yang besar |
| **Services pattern** | Logic bisnis di Service = lebih mudah di-prompt ke AI secara spesifik |
