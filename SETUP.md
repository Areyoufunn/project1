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
| Node.js | v18+ | https://nodejs.org |
| Expo CLI | Latest | `npm install -g expo-cli` |
| Android Studio | Latest | Untuk emulator Android |
| Expo Go App | Latest | Install di HP dari Play Store / App Store |

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
mkdir eduflow
cd eduflow

# 2. Init git
git init
git branch -M main

# 3. Buat repo di GitHub, lalu hubungkan
git remote add origin https://github.com/username/eduflow.git

# 4. Buat branch develop
git checkout -b develop
git push -u origin develop
```

---

## 🖥️ 2. Setup Backend — Laravel (Dev 1)

### Langkah 1: Buat Project Laravel

```bash
# Di dalam folder eduflow/
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
DB_DATABASE=eduflow
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
AWS_BUCKET=eduflow-audio

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
# (buka Laragon → klik MySQL → buat database "eduflow")

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

## 📱 3. Setup Frontend Mobile — React Native / Expo (Dev 2)

### Langkah 1: Buat Project Expo

```bash
# Di dalam folder eduflow/
npx create-expo-app mobile --template blank-typescript
cd mobile
```

### Langkah 2: Install Dependencies

```bash
# Navigation
npx expo install @react-navigation/native @react-navigation/stack @react-navigation/bottom-tabs
npx expo install react-native-screens react-native-safe-area-context

# HTTP Client
npm install axios

# Video Player (TikTok-style)
npx expo install expo-video

# Audio Recording (untuk voice note)
npx expo install expo-av

# Secure Storage (simpan token)
npx expo install expo-secure-store

# Notifications
npx expo install expo-notifications

# UI Components
npm install @shopify/flash-list
npm install react-native-reanimated
```

### Langkah 3: Konfigurasi API URL

Buat file `mobile/src/config/api.ts`:

```typescript
export const API_BASE_URL = __DEV__
  ? 'http://192.168.x.x:8000/api'  // Ganti dengan IP local komputer Dev 1
  : 'https://api.eduflow.id/v1';
```

> ⚠️ Gunakan **IP lokal** (bukan localhost) agar HP fisik bisa connect ke backend.
> Cari IP: jalankan `ipconfig` di terminal → cari IPv4 Address

### Langkah 4: Jalankan

```bash
# Jalankan Expo
npx expo start

# Scan QR code dengan Expo Go app di HP
# ATAU tekan 'a' untuk jalankan di Android emulator
```

---

## 🌐 4. Setup Admin Web — Next.js (Dev 2)

### Langkah 1: Buat Project Next.js

```bash
# Di dalam folder eduflow/
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
git clone https://github.com/username/eduflow.git
cd eduflow

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
eduflow/
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
| `php artisan migrate` error | Pastikan database `eduflow` sudah dibuat di MySQL |
| Mobile tidak bisa connect ke backend | Pastikan pakai IP lokal, bukan `localhost` |
| Redis connection refused | Jalankan Redis server: `redis-server` |
| OpenAI error 401 | API key salah atau quota habis |
| Queue tidak jalan | Pastikan `php artisan queue:work` aktif di terminal terpisah |
| Expo QR code tidak bisa scan | Pastikan HP dan laptop dalam WiFi yang sama |
