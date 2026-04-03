# 🏃 Sprint Plan — EduFlow Beta

> Tim: 2 Vibe Developer (AI-assisted)
> **Dev 1** → Backend (Laravel API + AI Integration)
> **Dev 2** → Frontend (React Native Mobile + Next.js Admin)
> Estimasi Total: **~5 Minggu** ke Beta siap test

---

## 🗂️ Pembagian Tanggung Jawab

| Layer | Dev 1 (Backend) | Dev 2 (Frontend) |
|---|---|---|
| **Tools** | Laravel, MySQL, Redis, OpenAI API | React Native, Next.js, Axios |
| **Output** | REST API endpoints | Screens & UI components |
| **Interface** | `API.md` sebagai kontrak | `API.md` sebagai referensi |

> 💡 **Aturan Tim:**
> - Selalu buat branch baru per fitur: `feature/nama-fitur`
> - Tidak boleh push langsung ke `main`
> - API yang belum jadi di-mock dulu di frontend (gunakan data dummy)

---

## ⚡ Sprint 1 — Foundation
**Durasi: 1 Minggu**
**Tujuan:** Project setup + Auth system jalan end-to-end

### Dev 1 — Backend
- [ ] Setup Laravel project + konfigurasi MySQL & Redis
- [ ] Buat semua migration (11 tabel dari ERD.md)
- [ ] Setup Laravel Sanctum untuk API auth
- [ ] Endpoint: `POST /auth/register` (dengan validasi invite code)
- [ ] Endpoint: `POST /auth/login`
- [ ] Endpoint: `GET /auth/me`
- [ ] Endpoint: `POST /auth/logout`
- [ ] CRUD Invite Code (`GET` / `POST` / `DELETE /invite-codes`)
- [ ] Role middleware: `developer`, `creator`, `student`
- [ ] Seeder: 1 developer, 3 invite code sample

### Dev 2 — Frontend
- [ ] Setup React Native project (Expo)
- [ ] Setup Next.js project untuk admin web
- [ ] Setup Axios + interceptor auth token
- [ ] Screen: Splash / Onboarding
- [ ] Screen: Login (mobile)
- [ ] Screen: Register + input invite code (mobile)
- [ ] Halaman: Login admin (Next.js)
- [ ] Navigasi dasar per role setelah login

### ✅ Definisi Selesai Sprint 1
> User bisa daftar pakai invite code, login, dan diarahkan ke dashboard sesuai role.

---

## ⚡ Sprint 2 — Core: Roadmap, Chapter & Konten
**Durasi: 1.5 Minggu**
**Tujuan:** Developer bisa buat roadmap, creator bisa upload konten, moderasi jalan

### Dev 1 — Backend
- [ ] CRUD Roadmap (`GET` / `POST` / `PUT` + publish)
- [ ] CRUD Chapter (termasuk `is_checkpoint` & `brief`)
- [ ] Upload konten video Creator (`POST /chapters/{id}/contents`)
- [ ] AI Auto-review konten (kirim ke OpenAI, cek kesesuaian brief)
- [ ] Update status konten: pending → ai_review → rejected/live
- [ ] Endpoint approve/reject Developer (`POST /contents/{id}/approve`)
- [ ] Storage: setup Cloudflare Stream / S3 untuk video
- [ ] Endpoint: `GET /contents/{id}` dengan status & feedback

### Dev 2 — Frontend
- [ ] Next.js: Halaman Developer — buat & edit Roadmap
- [ ] Next.js: Halaman Developer — buat & edit Chapter (dengan toggle checkpoint)
- [ ] Next.js: Halaman Developer — review & approve/reject konten
- [ ] Next.js: Creator Dashboard — list konten + status badge
- [ ] Next.js: Creator — upload video per chapter
- [ ] Next.js: Creator — halaman detail konten (lihat feedback jika ditolak)
- [ ] Mobile: Screen explore roadmap (dummy data dulu)

### ✅ Definisi Selesai Sprint 2
> Developer bisa buat roadmap + chapter, creator bisa upload video, AI mereview, developer approve/reject.

---

## ⚡ Sprint 3 — Student Flow + Voice Note
**Durasi: 1.5 Minggu**
**Tujuan:** Siswa bisa belajar end-to-end, dari enroll sampai voice note dinilai

### Dev 1 — Backend
- [ ] Endpoint enroll roadmap (`POST /enrollments`)
- [ ] Endpoint feed chapter (`GET /student/feed/{chapter_id}`) — konten random dulu, algoritma sprint 4
- [ ] Endpoint selesai chapter + hitung XP (`POST /student/feed/{chapter_id}/complete`)
- [ ] Update `student_analytics` (XP, streak) setiap chapter selesai
- [ ] Upload voice note ke S3 (`POST /voice-notes`)
- [ ] Integrasi OpenAI Whisper → transkripsi audio
- [ ] Integrasi LLM GPT-4o → scoring pemahaman dari transkrip
- [ ] Update status voice note: uploading → processing → scored
- [ ] Error handling: retry upload, fallback skor manual, audio tidak relevan
- [ ] Endpoint polling status voice note (`GET /voice-notes/{id}`)

### Dev 2 — Frontend
- [ ] Mobile: Screen explore roadmap (real API)
- [ ] Mobile: Detail roadmap → tombol enroll
- [ ] Mobile: Feed belajar TikTok-style (swipe vertikal, fullscreen video)
- [ ] Mobile: Overlay XP + badge setelah chapter selesai
- [ ] Mobile: Trigger dan Screen voice note recording
- [ ] Mobile: Progress bar upload + status processing
- [ ] Mobile: Screen hasil scoring voice note (skor + feedback LLM)
- [ ] Mobile: Tombol "Laporkan Masalah" (beta feedback, semua layar)
- [ ] Mobile: Notifikasi streak reminder (local notification)

### ✅ Definisi Selesai Sprint 3
> Siswa bisa enroll roadmap, tonton konten, rekam voice note, terima skor, dan unlock chapter berikutnya.

---

## ⚡ Sprint 4 — Algoritma + Analytics + Polish
**Durasi: 1 Minggu**
**Tujuan:** Algoritma TikTok aktif, dashboard analytics jalan, polish UI

### Dev 1 — Backend
- [ ] Implementasi `content_scores` table update setiap ada data baru
- [ ] Implementasi algoritma content selection di `GET /student/feed/{chapter_id}`
  - Cold start: tampilkan konten tayangan terbanyak
  - Jika ada data: `70% VN score + 20% completion + 10% rewatch`
- [ ] Endpoint analytics siswa (`GET /analytics/student`)
- [ ] Endpoint analytics roadmap untuk developer (`GET /analytics/roadmap/{id}`)
- [ ] Endpoint analytics konten untuk creator (`GET /contents/{id}/analytics`)
- [ ] Webhook handler: Whisper done + LLM done (async queue)

### Dev 2 — Frontend
- [ ] Mobile: Dashboard siswa (XP, streak, rata-rata skor, kelemahan)
- [ ] Next.js: Analytics developer (grafik dropout per chapter, avg score)
- [ ] Next.js: Analytics creator (views, completion rate, avg VN score)
- [ ] Mobile + Web: Polish UI keseluruhan
- [ ] Mobile: Animasi XP naik, badge unlock, streak
- [ ] Testing end-to-end semua flow
- [ ] Bug fixing

### ✅ Definisi Selesai Sprint 4
> Algoritma TikTok aktif memilih konten terbaik. Semua dashboard terisi data real. Beta siap diuji.

---

## 📅 Timeline Overview

```
Minggu 1    → Sprint 1: Foundation & Auth
Minggu 2-3  → Sprint 2: Roadmap + Content + Moderasi
Minggu 3-4  → Sprint 3: Student Flow + Voice Note AI
Minggu 5    → Sprint 4: Algoritma + Analytics + Polish
```

---

## 🌿 Git Branch Strategy

```
main                    ← Production / Beta release
  └── develop           ← Integration branch (merge dari feature)
        ├── feature/auth-backend         (Dev 1)
        ├── feature/auth-frontend        (Dev 2)
        ├── feature/roadmap-backend      (Dev 1)
        ├── feature/roadmap-frontend     (Dev 2)
        ├── feature/voicenote-backend    (Dev 1)
        ├── feature/voicenote-frontend   (Dev 2)
        └── dst...
```

**Alur kerja harian:**
```bash
# Mulai kerja
git pull origin develop
git checkout -b feature/nama-fitur

# Selesai fitur
git add .
git commit -m "feat: deskripsi singkat"
git push origin feature/nama-fitur
# → Buat Pull Request ke develop
```

---

## 🤖 Tips Vibe Coding untuk Tim Ini

### Dev 1 — Backend Prompts
Saat minta AI generate kode, sertakan konteks:
- File `ERD.md` → struktur tabel
- File `API.md` → format request & response
- Contoh: *"Buatkan Laravel controller untuk endpoint POST /voice-notes sesuai ERD.md dan API.md berikut: ..."*

### Dev 2 — Frontend Prompts
Saat minta AI generate kode, sertakan konteks:
- File `API.md` → endpoint yang akan dipanggil
- Contoh: *"Buatkan React Native screen untuk menampilkan feed TikTok-style, konsumsi endpoint GET /student/feed/{chapter_id} dengan format response berikut: ..."*

> 💡 Semakin detail konteks yang dikasih ke AI,
> semakin akurat kode yang dihasilkan.

---

## 📋 Checklist Beta Release

- [ ] Semua Sprint 1-4 selesai
- [ ] Auth + invite code berjalan
- [ ] End-to-end flow siswa tested (enroll → voice note → scoring)
- [ ] End-to-end flow creator tested (upload → moderasi → live)
- [ ] Error handling voice note tested
- [ ] Analytics dashboard terisi data real
- [ ] Deploy ke server (Docker + VPS)
- [ ] Invite 5-10 beta tester internal
