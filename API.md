# 📡 API Design — EduFlow

> Kontrak API antara Backend (Laravel) dan Frontend (React Native + Next.js).
> Semua response dalam format JSON. Base URL: `https://api.eduflow.id/v1`

---

## 🔐 Auth Header

Semua endpoint yang butuh auth wajib sertakan:
```
Authorization: Bearer {token}
```

---

## ✅ Format Response Standar

```json
// SUCCESS
{
  "success": true,
  "data": { ... },
  "message": "OK"
}

// ERROR
{
  "success": false,
  "message": "Pesan error",
  "errors": { ... }
}
```

---

## 1. AUTH

### `POST /auth/register`
Daftar akun baru dengan invite code.

**Request:**
```json
{
  "name": "Budi Santoso",
  "email": "budi@email.com",
  "password": "password123",
  "invite_code": "BETA-ABCD"
}
```
**Response:**
```json
{
  "success": true,
  "data": {
    "token": "eyJ...",
    "user": {
      "id": 1,
      "name": "Budi Santoso",
      "email": "budi@email.com",
      "role": "student"
    }
  }
}
```

---

### `POST /auth/login`
**Request:**
```json
{
  "email": "budi@email.com",
  "password": "password123"
}
```
**Response:** sama dengan register.

---

### `POST /auth/logout`
*Requires auth*

---

### `GET /auth/me`
*Requires auth* — Ambil data user yang sedang login.

---

## 2. INVITE CODES
> 🔒 Hanya Developer

### `GET /invite-codes`
List semua invite code yang pernah dibuat.

**Response:**
```json
{
  "data": [
    {
      "id": 1,
      "code": "BETA-ABCD",
      "role_target": "student",
      "max_uses": 50,
      "used_count": 12,
      "expires_at": "2025-12-31T23:59:59Z"
    }
  ]
}
```

---

### `POST /invite-codes`
Generate invite code baru.

**Request:**
```json
{
  "role_target": "creator",
  "max_uses": 10,
  "expires_at": "2025-12-31"
}
```

---

### `DELETE /invite-codes/{id}`
Nonaktifkan invite code.

---

## 3. ROADMAPS

### `GET /roadmaps`
Browse semua roadmap yang published. Support filter.

**Query Params:**
- `topic` → filter by topik
- `level` → `beginner | intermediate | advanced`
- `search` → search by title

**Response:**
```json
{
  "data": [
    {
      "id": 1,
      "title": "Web Development Fundamentals",
      "topic": "programming",
      "level": "beginner",
      "total_chapters": 10,
      "total_students": 245
    }
  ]
}
```

---

### `GET /roadmaps/{id}`
Detail roadmap + list chapter.

**Response:**
```json
{
  "data": {
    "id": 1,
    "title": "Web Development Fundamentals",
    "description": "...",
    "topic": "programming",
    "level": "beginner",
    "chapters": [
      {
        "id": 1,
        "title": "Pengenalan HTML",
        "order_number": 1,
        "is_checkpoint": false
      }
    ]
  }
}
```

---

### `POST /roadmaps`
> 🔒 Developer only

**Request:**
```json
{
  "title": "Web Development Fundamentals",
  "description": "Belajar web dari nol",
  "topic": "programming",
  "level": "beginner"
}
```

---

### `PUT /roadmaps/{id}`
> 🔒 Developer only — Update roadmap.

---

### `POST /roadmaps/{id}/publish`
> 🔒 Developer only — Publish roadmap (status draft → published).

---

## 4. CHAPTERS

### `GET /roadmaps/{roadmap_id}/chapters`
List chapter dalam roadmap (urut by order_number).

---

### `POST /roadmaps/{roadmap_id}/chapters`
> 🔒 Developer only

**Request:**
```json
{
  "title": "Pengenalan HTML",
  "brief": "Jelaskan struktur dasar HTML, tag penting, dan cara kerja browser merender HTML",
  "order_number": 1,
  "is_checkpoint": false
}
```

---

### `PUT /chapters/{id}`
> 🔒 Developer only — Update chapter.

---

### `DELETE /chapters/{id}`
> 🔒 Developer only — Hapus chapter.

---

## 5. CONTENTS (Video per Chapter)

### `GET /chapters/{chapter_id}/contents`
> 🔒 Creator / Developer — List semua konten di chapter ini.

---

### `POST /chapters/{chapter_id}/contents`
> 🔒 Creator only — Upload konten baru.

**Request (multipart/form-data):**
```
title: "HTML Dasar untuk Pemula"
video_file: [file]
```

**Response:**
```json
{
  "data": {
    "id": 5,
    "status": "pending",
    "message": "Konten sedang diproses AI review"
  }
}
```

---

### `GET /contents/{id}`
Detail konten + status moderasi.

**Response:**
```json
{
  "data": {
    "id": 5,
    "title": "HTML Dasar untuk Pemula",
    "status": "rejected",
    "ai_review_score": 42.5,
    "ai_review_feedback": "Konten tidak mencakup materi 'structure dasar HTML' yang ada di brief chapter"
  }
}
```

---

### `POST /contents/{id}/approve`
> 🔒 Developer only — Final approve konten ke live.

---

### `POST /contents/{id}/reject`
> 🔒 Developer only — Reject dengan feedback.

**Request:**
```json
{
  "feedback": "Konten tidak sesuai brief, tolong tambahkan penjelasan tentang DOCTYPE"
}
```

---

### `GET /contents/{id}/analytics`
> 🔒 Creator / Developer — Statistik konten.

**Response:**
```json
{
  "data": {
    "total_views": 120,
    "completion_rate": 0.78,
    "rewatch_rate": 0.15,
    "avg_voice_note_score": 82.3,
    "total_score": 74.2
  }
}
```

---

## 6. STUDENT — Feed & Discovery

### `GET /student/explore`
Browse roadmap untuk siswa (sama dengan GET /roadmaps tapi dengan status enrollment).

**Response tambahan:**
```json
{
  "data": [
    {
      "id": 1,
      "title": "Web Development Fundamentals",
      "is_enrolled": true,
      "progress_percent": 40
    }
  ]
}
```

---

### `POST /enrollments`
Siswa enroll ke roadmap.

**Request:**
```json
{
  "roadmap_id": 1
}
```

---

### `GET /enrollments`
List roadmap yang sedang diikuti siswa.

---

### `GET /student/feed/{chapter_id}`
> 🔒 Student only — Ambil konten terbaik untuk chapter ini (hasil algoritma TikTok).

**Response:**
```json
{
  "data": {
    "content_id": 5,
    "title": "HTML Dasar untuk Pemula",
    "video_url": "https://stream.cloudflare.com/...",
    "duration_seconds": 180,
    "creator": {
      "id": 3,
      "name": "Andi Creator"
    }
  }
}
```

---

### `POST /student/feed/{chapter_id}/complete`
Tandai siswa sudah selesai nonton chapter.

**Request:**
```json
{
  "content_id": 5,
  "completion_rate": 0.95,
  "rewatch_count": 1,
  "watch_duration_seconds": 171
}
```

**Response:**
```json
{
  "data": {
    "xp_earned": 50,
    "total_xp": 350,
    "streak": 5,
    "is_checkpoint": true,
    "message": "Chapter selesai! Ini adalah checkpoint — rekam voice note untuk lanjut."
  }
}
```

---

## 7. VOICE NOTES

### `POST /voice-notes`
Upload rekaman voice note siswa.

**Request (multipart/form-data):**
```
chapter_id: 1
audio_file: [file .m4a / .mp3 / .wav]
attempt_number: 1
```

**Response:**
```json
{
  "data": {
    "id": 10,
    "status": "processing",
    "message": "Audio sedang diproses, harap tunggu..."
  }
}
```

---

### `GET /voice-notes/{id}`
Cek status processing voice note (polling).

**Response:**
```json
{
  "data": {
    "id": 10,
    "status": "scored",
    "llm_score": 78.5,
    "llm_feedback": "Kamu sudah menjelaskan struktur HTML dengan baik, tapi kurang menyebutkan fungsi DOCTYPE.",
    "is_passed": true
  }
}
```

---

### `GET /chapters/{chapter_id}/voice-notes`
Riwayat voice note siswa di chapter tertentu.

---

## 8. ANALYTICS

### `GET /analytics/student`
> 🔒 Student — Dashboard analytics diri sendiri.

**Response:**
```json
{
  "data": {
    "total_xp": 1250,
    "current_streak": 7,
    "longest_streak": 14,
    "total_chapters_completed": 18,
    "avg_voice_note_score": 81.2,
    "weak_topics": ["CSS Flexbox", "JavaScript Async"]
  }
}
```

---

### `GET /analytics/roadmap/{id}`
> 🔒 Developer — Analytics per roadmap.

**Response:**
```json
{
  "data": {
    "total_students": 245,
    "completion_rate": 0.62,
    "dropout_chapter": {
      "id": 5,
      "title": "CSS Flexbox",
      "dropout_rate": 0.38
    },
    "avg_voice_note_score": 74.5
  }
}
```

---

## 9. WEBHOOK (Internal — Backend ke Backend)

### `POST /webhooks/whisper-done`
Dipanggil setelah Whisper selesai transkripsi (async).

### `POST /webhooks/llm-done`
Dipanggil setelah LLM selesai scoring (async).

---

## 📊 Summary Endpoint

| Method | Endpoint | Role |
|---|---|---|
| POST | /auth/register | Public |
| POST | /auth/login | Public |
| GET | /auth/me | Auth |
| GET/POST/DELETE | /invite-codes | Developer |
| GET | /roadmaps | Public |
| GET | /roadmaps/{id} | Public |
| POST/PUT | /roadmaps | Developer |
| POST | /roadmaps/{id}/publish | Developer |
| GET/POST/PUT/DELETE | /chapters | Developer |
| GET/POST | /contents | Creator |
| POST | /contents/{id}/approve | Developer |
| POST | /contents/{id}/reject | Developer |
| GET | /contents/{id}/analytics | Creator/Dev |
| GET | /student/explore | Student |
| POST/GET | /enrollments | Student |
| GET | /student/feed/{chapter_id} | Student |
| POST | /student/feed/{chapter_id}/complete | Student |
| POST | /voice-notes | Student |
| GET | /voice-notes/{id} | Student |
| GET | /analytics/student | Student |
| GET | /analytics/roadmap/{id} | Developer |
