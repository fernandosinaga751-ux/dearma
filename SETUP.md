# 🔥 PANDUAN SETUP FIREBASE + VERCEL
## Drive Zone — Rental Mobil Medan

---

## ⚡ Setup dalam 10 Menit

### LANGKAH 1: Buat Project Firebase

1. Buka **https://console.firebase.google.com**
2. Klik **"Add project"** → Nama: `drive-zone`
3. Matikan Google Analytics (opsional) → **Create project**
4. Tunggu ~30 detik

---

### LANGKAH 2: Setup Authentication (Login Admin)

1. Di sidebar Firebase, klik **"Authentication"**
2. Klik **"Get started"**
3. Tab **"Sign-in method"** → Enable **"Email/Password"** → Save
4. Tab **"Users"** → **"Add user"**:
   - Email: `admin@drivezone.com` (atau email Anda)
   - Password: buat password kuat
   - Klik **"Add user"**

---

### LANGKAH 3: Setup Firestore Database

1. Di sidebar, klik **"Firestore Database"**
2. Klik **"Create database"**
3. Pilih **"Start in production mode"** → Next
4. Region: **asia-southeast1 (Singapore)** → Enable
5. Tunggu database dibuat

**Setup Rules (Security):**
1. Klik tab **"Rules"**
2. Hapus semua isi, paste ini:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Siapa pun bisa buat dan baca booking (untuk form & tracking)
    match /bookings/{id} {
      allow read: if true;
      allow create: if true;
      allow update, delete: if request.auth != null;
    }
    // Siapa pun bisa baca cars (untuk form)
    match /cars/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    // Hanya admin yang bisa kelola rekening
    match /rekening/{id} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

3. Klik **"Publish"**

---

### LANGKAH 4: Ambil Firebase Config

1. Di Firebase console, klik ikon ⚙️ → **"Project settings"**
2. Scroll ke bawah ke **"Your apps"**
3. Klik ikon **"</>"** (Web)
4. App nickname: `drive-zone-web` → **Register app**
5. Copy bagian `firebaseConfig`:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXX",
  authDomain: "drive-zone-XXXXX.firebaseapp.com",
  projectId: "drive-zone-XXXXX",
  storageBucket: "drive-zone-XXXXX.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456:web:XXXXXXXXX"
};
```

---

### LANGKAH 5: Update 3 File HTML

Buka **index.html**, **admin.html**, dan **tracking.html**.

Di masing-masing file, cari dan **ganti** bagian ini:

```javascript
const firebaseConfig = {
  apiKey: "GANTI_API_KEY",
  authDomain: "GANTI_PROJECT_ID.firebaseapp.com",
  projectId: "GANTI_PROJECT_ID",
  storageBucket: "GANTI_PROJECT_ID.appspot.com",
  messagingSenderId: "GANTI_SENDER_ID",
  appId: "GANTI_APP_ID"
};
```

Dengan config Anda dari Langkah 4.

**Jangan lupa juga ganti nomor WhatsApp di index.html:**
```javascript
const WA_NUMBER = '6281234567890'; // Ganti dengan nomor WA Anda (format 62xxx)
```

---

### LANGKAH 6: Deploy ke Vercel

#### Cara A: GitHub + Vercel (Direkomendasikan)

1. Upload folder `drive-zone-firebase` ke GitHub repository baru
2. Buka **https://vercel.com** → Login dengan GitHub
3. Klik **"New Project"** → Import repository Anda
4. Biarkan semua pengaturan default → **Deploy**
5. Selesai! ✅

#### Cara B: Vercel CLI

```bash
npm install -g vercel
cd drive-zone-firebase
vercel
```

#### Cara C: Drag & Drop (Netlify alternatif)

1. Buka **https://app.netlify.com/drop**
2. Drag folder `drive-zone-firebase` ke browser
3. Live! ✅

---

## ✅ Hasil Akhir

| Halaman | URL |
|---------|-----|
| 🏠 Form Booking | `https://nama-project.vercel.app/` |
| 👨‍💼 Admin Dashboard | `https://nama-project.vercel.app/admin` |
| 📍 Tracking | `https://nama-project.vercel.app/tracking?id=DZ2025XX-XXXXXX` |

---

## 🔥 Fitur Firebase vs Supabase

| Fitur | Firebase | Supabase |
|-------|----------|---------|
| Realtime Updates | ✅ onSnapshot | ✅ Realtime Channels |
| Auth | ✅ Firebase Auth | ✅ Auth |
| Database | Firestore (NoSQL) | PostgreSQL (SQL) |
| Free Tier | 1GB storage, 50K reads/hari | 500MB, unlimited reads |
| Setup | Lebih mudah | Perlu SQL |
| CDN | Global | Regional |

---

## 🆘 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| "permission-denied" | Cek Firestore Rules sudah dipublish |
| Login admin gagal | Cek email/password sudah dibuat di Authentication |
| Data tidak muncul | Cek firebaseConfig sudah diisi benar di ketiga file |
| CORS error | Tambah domain Vercel ke Firebase Authorized Domains |

**Tambah Authorized Domain:**
Firebase Console → Authentication → Settings → Authorized Domains → Add domain (`nama-project.vercel.app`)

---

## 💰 Biaya Firebase (Gratis untuk rental kecil)

- Firestore: 1 GB storage, 50K reads/hari, 20K writes/hari — GRATIS
- Authentication: Unlimited users — GRATIS
- Hosting: 10 GB/bulan — GRATIS
- Vercel: Unlimited deployments — GRATIS

**Total: GRATIS!** 🎉
