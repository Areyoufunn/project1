# 🛠️ TECH STACK — EduFlow

> Dokumen ini menjelaskan bahasa pemrograman, framework, tools,
> dan struktur file lengkap untuk project EduFlow.

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────┐
│                    CLIENTS                          │
│  📱 Mobile (React Native)  🌐 Web Admin (Next.js)  │
└─────────────────┬───────────────────┬───────────────┘
                  │   REST API        │
┌─────────────────▼───────────────────▼───────────────┐
│              BACKEND — Laravel 11 (PHP 8.3)         │
│  Auth │ Roadmap │ Content │ VoiceNote │ Analytics   │
└───────┬─────────────────────────────────────────────┘
        │ Queue (Redis)
┌───────▼─────────────────────────────────────────────┐
│              AI PROCESSING LAYER                    │
│  OpenAI Whisper (transkripsi) + GPT-4o (scoring)   │
└─────────────────────────────────────────────────────┘
        │
┌───────▼─────────────────────────────────────────────┐
│              STORAGE LAYER                          │
│  MySQL (data) │ Redis (cache/queue) │ S3/CF (media) │
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
| AI SDK | openai-php/laravel | latest |
| Storage | league/flysystem-aws-s3-v3 | latest |
| Cache | predis/predis (Redis) | latest |

**Kenapa Laravel?**
- ✅ Sudah tersedia di Laragon — langsung pakai
- ✅ AI (Copilot/Cursor) punya training data Laravel terbanyak = kode paling akurat
- ✅ Built-in Queue untuk proses async Whisper + LLM
- ✅ Eloquent ORM sangat readable dan AI-friendly
- ✅ Komunitas terbesar, dokumentasi terlengkap

---

### 🔵 Mobile App — React Native + Expo (TypeScript)

| Komponen | Teknologi | Versi |
|---|---|---|
| Framework | React Native + Expo | SDK 51 |
| Language | TypeScript | 5.x |
| Navigation | React Navigation v6 | latest |
| HTTP Client | Axios | latest |
| Video Player | expo-video | latest |
| Audio Record | expo-av | latest |
| Storage Token | expo-secure-store | latest |
| List Performa | @shopify/flash-list | latest |
| Animasi | react-native-reanimated | latest |
| Notifikasi | expo-notifications | latest |

**Kenapa React Native + Expo?**
- ✅ TypeScript = type-safe, AI generate lebih akurat
- ✅ Expo = tidak perlu konfigurasi native (Android/iOS)
- ✅ `expo-av` = voice recording built-in
- ✅ `expo-video` = video player performa tinggi
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

**Kenapa Next.js?**
- ✅ TypeScript = sama dengan mobile, satu bahasa frontend
- ✅ App Router = modern, performa optimal
- ✅ Server components untuk dashboard data-heavy
- ✅ TypeScript types bisa di-share antara mobile dan admin

---

### 🗄️ Database & Infrastructure

| Layer | Teknologi | Fungsi |
|---|---|---|
| **Database Utama** | MySQL 8.0 | Semua data aplikasi |
| **Cache & Queue** | Redis | Session, queue job async |
| **Video Storage** | Cloudflare Stream | Upload & streaming video konten |
| **Audio Storage** | AWS S3 | Simpan file voice note .m4a |
| **AI Transkripsi** | OpenAI Whisper API | Speech-to-text voice note |
| **AI Scoring** | OpenAI GPT-4o | Analisa pemahaman dari transkrip |
| **Runtime** | Laragon (local) / Docker (production) | — |

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
│   │   ├── OpenAI/
│   │   │   ├── WhisperService.php        ← Panggil Whisper API
│   │   │   └── LLMScoringService.php     ← Panggil GPT-4o untuk scoring
│   │   ├── ContentScoreService.php       ← Hitung algoritma TikTok
│   │   ├── VoiceNoteService.php          ← Orchestrate proses voice note
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

### Mobile — React Native / Expo

```
mobile/
├── src/
│   ├── screens/                         ← Satu folder per role
│   │   ├── auth/
│   │   │   ├── LoginScreen.tsx
│   │   │   └── RegisterScreen.tsx       ← Input invite code di sini
│   │   └── student/
│   │       ├── FeedScreen.tsx           ← TikTok-style video feed
│   │       ├── ExploreScreen.tsx        ← Browse & filter roadmap
│   │       ├── RoadmapDetailScreen.tsx  ← Detail + tombol enroll
│   │       ├── VoiceNoteScreen.tsx      ← Record voice note
│   │       ├── VoiceNoteResultScreen.tsx← Tampilkan skor + feedback
│   │       └── ProgressScreen.tsx       ← XP, streak, analytics diri
│   │
│   ├── components/                      ← Komponen reusable
│   │   ├── VideoPlayer.tsx              ← Wrapper expo-video
│   │   ├── XPCounter.tsx               ← Tampilan XP + streak
│   │   ├── VoiceWaveform.tsx           ← Animasi waveform saat rekam
│   │   ├── RoadmapCard.tsx             ← Card di ExploreScreen
│   │   ├── ChapterProgressBar.tsx      ← Progress bar chapter
│   │   └── ScoreBadge.tsx              ← Badge skor voice note
│   │
│   ├── services/                        ← Semua pemanggilan API
│   │   ├── api.ts                       ← Axios instance + interceptor
│   │   ├── authService.ts
│   │   ├── roadmapService.ts
│   │   ├── feedService.ts
│   │   └── voiceNoteService.ts
│   │
│   ├── context/
│   │   └── AuthContext.tsx             ← Global auth state
│   │
│   ├── hooks/                           ← Custom hooks
│   │   ├── useVoiceRecorder.ts         ← Logic rekam audio
│   │   ├── usePollingStatus.ts         ← Polling voice note status
│   │   └── useXPAnimation.ts           ← Animasi XP naik
│   │
│   ├── types/                           ← TypeScript interfaces
│   │   ├── User.ts
│   │   ├── Roadmap.ts
│   │   ├── VoiceNote.ts
│   │   └── Content.ts
│   │
│   └── config/
│       ├── api.ts                       ← BASE_URL config
│       └── constants.ts                ← Warna, ukuran, dll
│
├── app.json
└── tsconfig.json
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
[VOICE NOTE FLOW - ASYNC]

Mobile                  Backend               Queue Worker
  │                        │                       │
  ├─ POST /voice-notes ───►│                       │
  │                        ├── Simpan di S3         │
  │                        ├── Status: "uploading"  │
  │                        ├── Dispatch Job ────────►│
  │◄── { id, "processing" }│                       ├── Whisper API
  │                        │                       ├── Transkrip selesai
  ├─ GET /voice-notes/5 ──►│                       ├── LLM GPT-4o scoring
  │◄── { "processing" }    │                       ├── Update DB
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
| **TypeScript di frontend** | Type-safe → AI generate komponen dengan props yang benar |
| **Expo = zero native config** | Tidak perlu sentuh Xcode/Android Studio untuk beta |
| **Struktur modular** | Tiap fitur terpisah → AI bisa fokus per file tanpa context yang besar |
| **Services pattern** | Logic bisnis di Service = lebih mudah di-prompt ke AI secara spesifik |
