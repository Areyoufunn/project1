# ⚙️ SETUP — EduFlow Development Environment

> Panduan setup environment dari nol untuk 2 developer.
> Ikuti urutan ini dengan teliti agar tidak ada yang terlewat.

---

## 📋 Prerequisites — Install Ini Dulu

### Wajib di Semua Komputer
| Software | Versi | Download |
|---|---|---|
| Git | Latest | https://git-scm.com |
| Node.js | v18+ | https://nodejs.org |
| VS Code | Latest | https://code.visualstudio.com |

### Dev 1 — Backend
| Software | Versi | Cara Install |
|---|---|---|
| PHP | 8.2+ | Sudah ada di Laragon |
| Composer | Latest | https://getcomposer.org |
| MySQL | 8.0+ | Sudah ada di Laragon |
| Redis | Latest | https://redis.io atau via Laragon |
| Laragon | Latest | https://laragon.org |

### Dev 2 — Frontend
| Software | Versi | Cara Install |
|---|---|---|
| Flutter SDK | 3.x (stable) | https://flutter.dev/docs/get-started/install |
| Android Studio | Latest | https://developer.android.com/studio |
| Dart SDK | 3.x | Sudah bundled dengan Flutter |
| Node.js | v18+ | https://nodejs.org (untuk Next.js admin) |

> 💡 **Cara cepat install Flutter di Windows:**
> 1. Download Flutter SDK dari https://flutter.dev
> 2. Extract ke `C:\flutter`
> 3. Tambahkan `C:\flutter\bin` ke Environment Variables → PATH
> 4. Jalankan `flutter doctor` untuk cek semua sudah benar

---

## 🔑 Akun & API Keys yang Dibutuhkan

Buat akun di layanan berikut sebelum mulai:

| Layanan | Untuk | Link |
|---|---|---|
| **OpenAI** | Whisper + GPT-4o | https://platform.openai.com |
| **Cloudflare** | Video streaming | https://dash.cloudflare.com |
| **AWS S3** | Simpan audio voice note | https://aws.amazon.com |
| **GitHub** | Repository & kolaborasi | https://github.com |

---

## 🗂️ 1. Setup Repository (Lakukan Sekali, Dev 1)

```bash
# 1. Buat folder project
mkdir EduFlow
cd EduFlow

# 2. Init git
git init
git branch -M main

# 3. Buat repo di GitHub, lalu hubungkan
git remote add origin https://github.com/username/EduFlow.git

# 4. Buat branch develop
git checkout -b develop
git push -u origin develop
```

---

## 🖥️ 2. Setup Backend — Laravel (Dev 1)

### Langkah 1: Buat Project Laravel

```bash
# Di dalam folder EduFlow/
composer create-project laravel/laravel backend
cd backend
```

### Langkah 2: Install Dependencies

```bash
# Auth API
composer require laravel/sanctum

# Queue & Redis
composer require predis/predis

# AWS S3
composer require league/flysystem-aws-s3-v3

# OpenAI PHP Client
composer require openai-php/laravel

# Image/Video handling
composer require intervention/image
```

### Langkah 3: Buat File `.env`

Copy file `.env.example` dan isi dengan konfigurasi berikut:

```env
APP_NAME=EduFlow
APP_ENV=local
APP_KEY=  # akan diisi otomatis
APP_DEBUG=true
APP_URL=http://localhost:8000

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=EduFlow
DB_USERNAME=root
DB_PASSWORD=

# Redis (untuk queue async)
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# Queue — pakai redis agar async
QUEUE_CONNECTION=redis

# OpenAI
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx
OPENAI_ORGANIZATION=  # opsional

# AWS S3 (untuk audio voice note)
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=ap-southeast-1
AWS_BUCKET=EduFlow-audio

# Cloudflare Stream (untuk video)
CLOUDFLARE_ACCOUNT_ID=
CLOUDFLARE_API_TOKEN=
CLOUDFLARE_STREAM_URL=https://api.cloudflare.com/client/v4/accounts

# File Storage
FILESYSTEM_DISK=s3
```

### Langkah 4: Jalankan Setup

```bash
# Generate app key
php artisan key:generate

# Buat database di MySQL
# (buka Laragon → klik MySQL → buat database "EduFlow")

# Jalankan migration
php artisan migrate

# Jalankan seeder (data awal)
php artisan db:seed

# Jalankan server
php artisan serve
# → Running di http://localhost:8000
```

### Langkah 5: Jalankan Queue Worker (di terminal terpisah)

```bash
# Untuk proses async voice note (Whisper + LLM)
php artisan queue:work redis --queue=voice-note
```

### Verifikasi Backend Jalan

Buka browser atau Postman:
```
GET http://localhost:8000/api/health
# Response: {"status": "ok"}
```

---

## 📱 3. Setup Frontend Mobile — Flutter (Dev 2)

### Langkah 1: Verifikasi Flutter Terinstall

```bash
# Cek instalasi Flutter
flutter doctor

# Semua harus ✅ kecuali iOS (kalau tidak punya Mac)
# Android toolchain, connected device, dan VS Code harus OK
```

### Langkah 2: Buat Project Flutter

```bash
# Di dalam folder EduFlow/
flutter create mobile
cd mobile
```

### Langkah 3: Install Dependencies

Edit `pubspec.yaml` dan tambahkan dependencies berikut:

```yaml
dependencies:
  flutter:
    sdk: flutter

  # State Management
  flutter_riverpod: ^2.5.1
  riverpod_annotation: ^2.3.5

  # HTTP Client
  dio: ^5.4.3

  # Navigation
  go_router: ^13.2.0

  # Video Player (TikTok-style)
  video_player: ^2.8.3
  chewie: ^1.8.1

  # Audio Recording (Voice Note)
  record: ^5.1.1

  # Secure Storage (simpan token)
  flutter_secure_storage: ^9.0.0

  # Animasi Lottie (XP, badge)
  lottie: ^3.1.0

  # Push Notification
  flutter_local_notifications: ^17.1.2

  # Image caching
  cached_network_image: ^3.3.1
```

Lalu jalankan:

```bash
flutter pub get
```

### Langkah 4: Konfigurasi API URL

Buat file `mobile/lib/config/api_config.dart`:

```dart
class ApiConfig {
  // Ganti dengan IP lokal komputer Dev 1 saat development
  static const String baseUrl = String.fromEnvironment(
    'API_URL',
    defaultValue: 'http://192.168.x.x:8000/api',
  );
}
```

> ⚠️ Gunakan **IP lokal** (bukan localhost) agar HP fisik bisa connect ke backend.
> Cari IP: jalankan `ipconfig` di terminal → cari IPv4 Address

### Langkah 5: Jalankan

```bash
# Pastikan HP/emulator sudah connect
flutter devices

# Jalankan ke device
flutter run

# Atau spesifik ke Android emulator
flutter run -d emulator-5554
```

---

## 🌐 4. Setup Admin Web — Next.js (Dev 2)

### Langkah 1: Buat Project Next.js

```bash
# Di dalam folder EduFlow/
npx create-next-app admin --typescript --tailwind --app
cd admin
```

### Langkah 2: Install Dependencies

```bash
# HTTP Client
npm install axios

# Charts untuk analytics
npm install recharts

# UI Components
npm install @radix-ui/react-dialog @radix-ui/react-dropdown-menu
npm install lucide-react

# Form handling
npm install react-hook-form zod @hookform/resolvers

# Token storage
npm install js-cookie
npm install --save-dev @types/js-cookie
```

### Langkah 3: Buat File `.env.local`

```env
NEXT_PUBLIC_API_URL=http://localhost:8000/api
```

### Langkah 4: Jalankan

```bash
npm run dev
# → Running di http://localhost:3000
```

---

## 🔄 5. Cara Clone & Setup untuk Developer Kedua

Jika Dev 2 mau setup project yang sudah ada:

```bash
# Clone repo
git clone https://github.com/username/EduFlow.git
cd EduFlow

# Checkout branch develop
git checkout develop

# Setup backend
cd backend
composer install
cp .env.example .env
php artisan key:generate
# → Edit .env dengan konfigurasi yang diberikan Dev 1
php artisan migrate
php artisan db:seed

# Setup mobile
cd ../mobile
npm install

# Setup admin
cd ../admin
npm install
cp .env.local.example .env.local
```

---

## 📁 6. Struktur Folder Project

```
EduFlow/
├── backend/          ← Laravel API (Dev 1)
│   ├── app/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   │   ├── AuthController.php
│   │   │   │   ├── RoadmapController.php
│   │   │   │   ├── ChapterController.php
│   │   │   │   ├── ContentController.php
│   │   │   │   ├── VoiceNoteController.php
│   │   │   │   └── AnalyticsController.php
│   │   │   └── Middleware/
│   │   ├── Models/
│   │   ├── Jobs/           ← Async queue jobs
│   │   │   ├── ProcessWhisper.php
│   │   │   └── ProcessLLMScoring.php
│   │   └── Services/       ← Business logic
│   │       ├── OpenAIService.php
│   │       ├── ContentScoreService.php
│   │       └── VoiceNoteService.php
│   ├── database/
│   │   ├── migrations/
│   │   └── seeders/
│   └── routes/
│       └── api.php
│
├── mobile/           ← React Native / Expo (Dev 2)
│   ├── src/
│   │   ├── screens/
│   │   │   ├── student/
│   │   │   │   ├── FeedScreen.tsx
│   │   │   │   ├── VoiceNoteScreen.tsx
│   │   │   │   └── ExploreScreen.tsx
│   │   │   ├── auth/
│   │   │   └── shared/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/     ← API calls
│   │   └── config/
│   └── app.json
│
├── admin/            ← Next.js Web Admin (Dev 2)
│   ├── app/
│   │   ├── (developer)/
│   │   │   ├── roadmaps/
│   │   │   ├── contents/
│   │   │   └── analytics/
│   │   ├── (creator)/
│   │   │   ├── contents/
│   │   │   └── analytics/
│   │   └── auth/
│   ├── components/
│   └── lib/
│
├── markdown.md       ← Dokumentasi sistem
├── ERD.md            ← Database design
├── API.md            ← API contracts
├── SPRINT.md         ← Sprint plan
├── SETUP.md          ← File ini
└── CONVENTION.md     ← Code conventions
```

---

## 🆘 Troubleshooting Umum

| Masalah | Solusi |
|---|---|
| `php artisan migrate` error | Pastikan database `EduFlow` sudah dibuat di MySQL |
| Mobile tidak bisa connect ke backend | Pastikan pakai IP lokal, bukan `localhost` |
| Redis connection refused | Jalankan Redis server: `redis-server` |
| OpenAI error 401 | API key salah atau quota habis |
| Queue tidak jalan | Pastikan `php artisan queue:work` aktif di terminal terpisah |
| Expo QR code tidak bisa scan | Pastikan HP dan laptop dalam WiFi yang sama |
