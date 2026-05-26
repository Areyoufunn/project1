# 🎨 WIREFRAME — EduFlow UI Design

> Referensi visual untuk Dev 2 (Frontend) dalam membangun tampilan aplikasi.
> Gunakan wireframe ini sebagai konteks saat prompt ke AI untuk generate kode UI.

---

## 🖼️ Preview Wireframe

### Mobile — Student Screens
![Student Screens](wireframes/student_screens.png)

### Web Admin — Developer & Creator Dashboard
![Dashboard Screens](wireframes/dashboard_screens.png)

### Mobile — Dual Navigation (Roadmap vs Discover)
![Dual Navigation Wireframe](wireframes/dual_navigation.png)

---


## 📱 Mobile App — Student Screens

### Screen 1: Feed Belajar (TikTok-style)
**File:** `mobile/src/screens/student/FeedScreen.tsx`

**Elemen yang harus ada:**
- Full screen video player (vertikal)
- Overlay gelap di bawah dengan info chapter
- Progress chapter di atas: `Chapter 3 / 10`
- XP counter kanan atas: `350 XP 🔥5`
- Nama chapter & creator di bawah
- Tombol swipe up untuk lanjut
- Tombol side kanan: bookmark

**Behavior:**
- Swipe UP → chapter berikutnya
- Video autoplay saat masuk
- Pause saat swipe / background

---

### Screen 2: Voice Note Recording
**File:** `mobile/src/screens/student/VoiceNoteScreen.tsx`

**Elemen yang harus ada:**
- Teks instruksi: *"Jelaskan materi Chapter 3 dengan kata-katamu sendiri"*
- Tombol rekam bulat besar (merah) di tengah
- Animasi waveform saat merekam
- Timer rekaman: `0:23`
- Tombol stop & kirim
- Tips singkat: *"Tidak perlu sempurna — jelaskan seperti ke teman"*

**Behavior:**
- Tap tombol → mulai rekam
- Tap lagi → stop
- Konfirmasi sebelum kirim

---

### Screen 3: Hasil Voice Note
**File:** `mobile/src/screens/student/VoiceNoteResultScreen.tsx`

**Elemen yang harus ada:**
- Skor besar: `82 / 100` (warna hijau jika lulus, merah jika belum)
- Label: `Pemahaman Cukup ✅` / `Belum Cukup ❌`
- Feedback teks dari LLM
- Progress bar pemahaman
- Tombol: `Lanjut Chapter 4` (jika lulus) / `Rekam Ulang` (jika belum)

---

### Screen 4: Explore Roadmap
**File:** `mobile/src/screens/student/ExploreScreen.tsx`

**Elemen yang harus ada:**
- Search bar di atas
- Filter tabs: `Semua | Programming | Design | dll`
- Card per roadmap berisi:
  - Ikon topik
  - Judul roadmap
  - Badge level: `Beginner / Intermediate / Advanced`
  - Total siswa: `245 siswa`
  - Progress bar (kalau sudah enrolled)
  - Tombol `Mulai` / `Lanjutkan`

---

## 🌐 Web Admin — Dashboard Screens

### Screen 5: Developer Dashboard
**File:** `admin/app/(developer)/page.tsx`

**Elemen yang harus ada:**
- Sidebar navigasi: Roadmaps, Content Review, Analytics, Invite Codes
- Stats cards:
  - Siswa Aktif
  - Konten Pending Review
  - Roadmap Published
- Tabel konten pending: Creator, Chapter, AI Score, Tombol Approve/Reject
- Grafik dropout rate per chapter

---

### Screen 6: Creator Dashboard
**File:** `admin/app/(creator)/page.tsx`

**Elemen yang harus ada:**
- Sidebar navigasi: My Contents, Analytics, Roadmaps
- Stats cards:
  - Konten Live
  - Konten Pending
  - Avg Voice Note Score siswa
- Tabel konten:
  - Chapter
  - Status badge (Live / Pending / Ditolak)
  - Views
  - Completion Rate
  - Avg VN Score
- Klik "Ditolak" → lihat feedback AI + developer

---

## 🎨 Design System

### 🎨 Color Palette — EduFlow Brand

| Token | Hex | Fungsi |
|-------|-----|--------|
| `colorBackground` | `#0A1931` | Background utama (dark navy) |
| `colorSurface` | `#F6FAFD` | Surface card, teks utama, input bg |
| `colorPrimary` | `#1A3D63` | Button, CTA, elemen aktif |
| `colorAccent` | `#4A7FA7` | Hover state, icon aktif, highlight |
| `colorSecondary` | `#B3CFE5` | Logo, subtitle, border, placeholder |
| `colorError` | `#C0392B` | Error, warning, badge gagal |
| `colorSuccess` | `#27AE60` | Success, skor lulus, badge unlock |
| `colorOverlay` | `rgba(10,25,49,0.75)` | Overlay video, modal backdrop |

```dart
// Flutter — lib/config/theme.dart
const Color colorBackground = Color(0xFF0A1931); // BG
const Color colorSurface    = Color(0xFFF6FAFD); // Font / Surface
const Color colorPrimary    = Color(0xFF1A3D63); // Button
const Color colorAccent     = Color(0xFF4A7FA7); // Hover
const Color colorSecondary  = Color(0xFFB3CFE5); // Logo
const Color colorError      = Color(0xFFC0392B); // Error
const Color colorSuccess    = Color(0xFF27AE60); // Success
```

### Gradient

```dart
// Gradient utama untuk header / hero section
const LinearGradient primaryGradient = LinearGradient(
  begin: Alignment.topLeft,
  end: Alignment.bottomRight,
  colors: [Color(0xFF0A1931), Color(0xFF1A3D63)],
);

// Gradient aksen (tombol CTA)
const LinearGradient accentGradient = LinearGradient(
  begin: Alignment.centerLeft,
  end: Alignment.centerRight,
  colors: [Color(0xFF1A3D63), Color(0xFF4A7FA7)],
);
```

### Font

```
Font Utama  : Inter (Google Fonts)
Heading     : Inter Bold (700)     → warna: #F6FAFD
Subheading  : Inter SemiBold (600) → warna: #B3CFE5
Body        : Inter Regular (400)  → warna: #F6FAFD
Caption     : Inter Medium (500)   → warna: #B3CFE5
```

### Ukuran Teks (Flutter sp)

```
H1 : 28sp
H2 : 22sp
H3 : 18sp
Body : 14sp
Caption : 12sp
```

### Border Radius

```
Button   : 12px
Card     : 16px
Badge    : 999px (pill)
Input    : 10px
Bottom Sheet : 24px (top corners)
```

### Elevation & Shadow (Android)

```dart
// Card shadow
BoxShadow(
  color: Color(0xFF0A1931).withOpacity(0.4),
  blurRadius: 12,
  offset: Offset(0, 4),
)
```

---

## 💡 Tips Prompt ke AI untuk Generate UI

Saat minta AI buatkan komponen Flutter, gunakan template ini:

```
Buatkan [nama screen/widget] untuk Flutter Android dengan:
- Background: #0A1931 (dark navy)
- Button/CTA: #1A3D63
- Hover/aktif: #4A7FA7
- Logo/subtitle: #B3CFE5
- Teks & surface: #F6FAFD
- Font: Inter (Google Fonts)
- Elemen: [list elemen dari wireframe di atas]
- Data dari API: [paste format response dari API.md]
- Behavior: [jelaskan interaksi]
```

**Contoh prompt:**
```
Buatkan feed_screen.dart untuk Flutter Android dengan:
- Background #0A1931 (dark navy) fullscreen
- Video player fullscreen menggunakan video_player + chewie
- Overlay bawah: chapter title (#F6FAFD), creator name (#B3CFE5)
- Tombol CTA warna #1A3D63, hover #4A7FA7
- XP counter kanan atas: "350 XP 🔥5" dengan warna #4A7FA7
- Progress chapter atas: "3/10" warna #B3CFE5
- Swipe up gesture untuk next chapter
- Data chapter dari API response: { content_id, title, video_url, creator.name }
```
