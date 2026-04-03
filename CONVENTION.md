# 📐 CONVENTION — EduFlow Code Standards

> Aturan kode yang wajib disepakati berdua agar kode konsisten,
> mudah dibaca, dan tidak konflik saat merge.

---

## 🌿 1. Git & Branch Convention

### Penamaan Branch

```
Format: <type>/<nama-singkat-fitur>

Contoh:
feature/auth-login
feature/roadmap-crud
feature/voicenote-upload
fix/voicenote-retry-bug
hotfix/login-crash
chore/setup-redis
```

| Prefix | Kapan Dipakai |
|---|---|
| `feature/` | Fitur baru |
| `fix/` | Bug fix biasa |
| `hotfix/` | Bug kritis di production |
| `chore/` | Setup, konfigurasi, tidak ada kode logika |
| `refactor/` | Refaktor kode tanpa ubah fungsi |

---

### Format Commit Message

```
Format: <type>: <deskripsi singkat dalam bahasa Inggris>

Contoh yang BENAR:
feat: add voice note upload endpoint
fix: handle whisper timeout fallback
chore: setup redis queue connection
refactor: extract scoring logic to service
docs: update API.md with new endpoint

Contoh yang SALAH:
❌ update file
❌ fix bug
❌ done
❌ wip
```

| Type | Kapan Dipakai |
|---|---|
| `feat` | Tambah fitur baru |
| `fix` | Perbaiki bug |
| `chore` | Setup/konfigurasi |
| `refactor` | Refaktor kode |
| `docs` | Update dokumentasi |
| `test` | Tambah atau update test |
| `style` | Formatting, tidak ada perubahan logika |

---

### Pull Request Rules

1. **Selalu buat PR ke `develop`**, tidak langsung ke `main`
2. **Minimal 1 review** dari developer lain sebelum merge
3. **Judul PR** = sama dengan commit message utama
4. **Deskripsi PR** wajib isi:
   - Apa yang berubah
   - Cara test
   - Screenshot kalau ada perubahan UI

---

## 🖥️ 2. Backend Convention — Laravel

### Penamaan File & Class

| Komponen | Format | Contoh |
|---|---|---|
| Controller | `PascalCase` + `Controller` | `VoiceNoteController.php` |
| Model | `PascalCase` singular | `VoiceNote.php` |
| Migration | `snake_case` + timestamp | `2024_01_01_create_voice_notes_table.php` |
| Job | `PascalCase` + aksi | `ProcessWhisperTranscription.php` |
| Service | `PascalCase` + `Service` | `VoiceNoteService.php` |
| Request | `PascalCase` + `Request` | `StoreVoiceNoteRequest.php` |

---

### Struktur Controller

```php
<?php

namespace App\Http\Controllers;

use App\Models\VoiceNote;
use App\Services\VoiceNoteService;
use App\Http\Requests\StoreVoiceNoteRequest;
use Illuminate\Http\JsonResponse;

class VoiceNoteController extends Controller
{
    // Gunakan Dependency Injection, bukan new di dalam method
    public function __construct(
        private VoiceNoteService $voiceNoteService
    ) {}

    public function store(StoreVoiceNoteRequest $request): JsonResponse
    {
        $voiceNote = $this->voiceNoteService->upload($request->validated());

        return response()->json([
            'success' => true,
            'data' => $voiceNote,
            'message' => 'Voice note sedang diproses'
        ], 201);
    }
}
```

**Rules:**
- Controller hanya untuk **terima request → panggil service → return response**
- **Logika bisnis** masuk ke `Service`, bukan controller
- Selalu gunakan **Form Request** untuk validasi
- Response selalu format: `{ success, data, message }`

---

### Penamaan Variabel & Method

```php
// ✅ BENAR
$voiceNote = VoiceNote::find($id);
$contentScore = $this->calculateScore($completion, $rewatch, $voiceNoteScore);

public function calculateScore(float $completion, float $rewatch, float $voiceNote): float
{
    return ($voiceNote * 0.7) + ($completion * 0.2) + ($rewatch * 0.1);
}

// ❌ SALAH
$vn = VoiceNote::find($id);          // singkatan tidak jelas
$cs = $this->calc($c, $r, $v);       // terlalu disingkat
public function calc($c, $r, $v) {}  // nama tidak deskriptif
```

---

### Database & Migration Rules

```php
// Selalu gunakan snake_case untuk nama kolom
Schema::create('voice_notes', function (Blueprint $table) {
    $table->id();
    $table->foreignId('student_id')->constrained('users')->cascadeOnDelete();
    $table->foreignId('chapter_id')->constrained()->cascadeOnDelete();
    $table->string('audio_url');
    $table->text('transcript')->nullable();
    $table->float('llm_score')->nullable();
    $table->text('llm_feedback')->nullable();
    $table->unsignedTinyInteger('attempt_number')->default(1);
    $table->enum('status', ['uploading', 'processing', 'scored', 'failed'])->default('uploading');
    $table->timestamps();
});
```

**Rules:**
- Nama tabel: **plural snake_case** → `voice_notes`, `content_scores`
- Nama kolom: **singular snake_case** → `student_id`, `audio_url`
- Selalu ada `created_at` dan `updated_at` (`$table->timestamps()`)
- Foreign key: `{nama_tabel_singular}_id` → `student_id`, `chapter_id`

---

### API Response Format

```php
// Selalu gunakan format ini:

// Success
return response()->json([
    'success' => true,
    'data'    => $data,
    'message' => 'Berhasil'
], 200);

// Error validasi
return response()->json([
    'success' => false,
    'message' => 'Data tidak valid',
    'errors'  => $validator->errors()
], 422);

// Not found
return response()->json([
    'success' => false,
    'message' => 'Data tidak ditemukan'
], 404);

// Unauthorized
return response()->json([
    'success' => false,
    'message' => 'Tidak memiliki akses'
], 403);
```

---

## 📱 3. Frontend Convention — React Native & Next.js

### Penamaan File & Komponen

| Komponen | Format | Contoh |
|---|---|---|
| Screen | `PascalCase` + `Screen` | `FeedScreen.tsx` |
| Component | `PascalCase` | `VideoCard.tsx` |
| Hook | `camelCase` + `use` prefix | `useVoiceRecorder.ts` |
| Service (API) | `camelCase` + `Service` | `voiceNoteService.ts` |
| Type/Interface | `PascalCase` | `VoiceNote.ts` |
| Constant | `SCREAMING_SNAKE_CASE` | `API_BASE_URL` |

---

### Struktur Komponen React

```tsx
// ✅ Format yang benar

import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet } from 'react-native';

// Type declaration di atas komponen
interface VoiceNoteCardProps {
  score: number;
  feedback: string;
  onRetry: () => void;
}

const VoiceNoteCard: React.FC<VoiceNoteCardProps> = ({
  score,
  feedback,
  onRetry
}) => {
  // State declarations
  const [isVisible, setIsVisible] = useState(false);

  // Effects
  useEffect(() => {
    setIsVisible(score > 0);
  }, [score]);

  // Render
  return (
    <View style={styles.container}>
      <Text style={styles.score}>{score}</Text>
      <Text style={styles.feedback}>{feedback}</Text>
    </View>
  );
};

// Styles di bawah komponen
const styles = StyleSheet.create({
  container: {
    padding: 16,
    borderRadius: 8,
  },
  score: {
    fontSize: 24,
    fontWeight: 'bold',
  },
  feedback: {
    fontSize: 14,
    marginTop: 8,
  },
});

export default VoiceNoteCard;
```

---

### Pemanggilan API

```typescript
// Buat file service terpisah per domain
// src/services/voiceNoteService.ts

import api from '../config/axios';

export const voiceNoteService = {
  // Upload voice note
  upload: async (chapterId: number, audioFile: FormData) => {
    const response = await api.post('/voice-notes', audioFile, {
      headers: { 'Content-Type': 'multipart/form-data' }
    });
    return response.data;
  },

  // Cek status (polling)
  getStatus: async (id: number) => {
    const response = await api.get(`/voice-notes/${id}`);
    return response.data;
  }
};

// ❌ JANGAN panggil axios langsung di dalam komponen/screen
```

---

### State Management Rules

```typescript
// Gunakan React hooks (useState, useReducer) untuk state lokal
// Gunakan Context API untuk state global (auth, user info)
// TIDAK perlu Redux untuk project ini (terlalu kompleks untuk beta)

// Contoh Auth Context:
// src/context/AuthContext.tsx
export const AuthContext = createContext<AuthContextType | null>(null);
```

---

### TypeScript Rules

```typescript
// ✅ SELALU definisikan tipe data
interface User {
  id: number;
  name: string;
  email: string;
  role: 'developer' | 'creator' | 'student';
}

// ❌ JANGAN gunakan 'any'
const user: any = ...;  // DILARANG

// ✅ Kalau tidak tahu tipenya, gunakan 'unknown' dan lakukan type guard
const processData = (data: unknown) => {
  if (typeof data === 'string') { ... }
};
```

---

## 📝 4. Dokumentasi Code

### Kapan Harus Tulis Komentar

```php
// ✅ Tulis komentar untuk logika yang tidak obvious
// Formula: 70% VN score + 20% completion + 10% rewatch
// Bobot diambil dari hasil riset efektivitas konten edukasi
$totalScore = ($voiceNoteScore * 0.7) + ($completionRate * 0.2) + ($rewatchRate * 0.1);

// ❌ Jangan komentar yang obvious
// Ambil user dari database
$user = User::find($id);
```

```typescript
// ✅ Gunakan JSDoc untuk fungsi kompleks
/**
 * Polling status voice note setiap 3 detik
 * sampai status 'scored' atau 'failed'
 * @param voiceNoteId - ID voice note yang diproses
 * @param maxAttempts - Maksimal polling (default: 20 = 1 menit)
 */
const pollVoiceNoteStatus = async (voiceNoteId: number, maxAttempts = 20) => {
  ...
};
```

---

## 🚫 5. Rules yang TIDAK BOLEH Dilanggar

| No | Larangan |
|---|---|
| 1 | ❌ Push langsung ke `main` atau `develop` |
| 2 | ❌ Commit API key / secret ke repository |
| 3 | ❌ Gunakan `any` di TypeScript |
| 4 | ❌ Tulis logika bisnis di Controller (masuk ke Service) |
| 5 | ❌ Hardcode URL API di komponen (pakai config/constant) |
| 6 | ❌ Merge PR sendiri tanpa review dari partner |
| 7 | ❌ Buat migration baru tanpa update `ERD.md` |
| 8 | ❌ Tambah endpoint baru tanpa update `API.md` |

---

## ✅ 6. Checklist Sebelum Buat PR

```
Sebelum push & buat Pull Request, pastikan:

[ ] Kode sudah tested manual (endpoint jalan / screen tampil)
[ ] Tidak ada console.log / dd() / var_dump() yang tertinggal
[ ] Commit message sudah sesuai format
[ ] Tidak ada API key yang tidak sengaja ter-commit
[ ] Kalau ada endpoint baru → API.md sudah diupdate
[ ] Kalau ada tabel/kolom baru → ERD.md sudah diupdate
```
