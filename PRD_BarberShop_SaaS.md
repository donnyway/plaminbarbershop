# Product Requirements Document (PRD)
## BarberShop Customer Management SaaS

**Versi:** 1.0 - MVP  
**Tanggal:** 21 Juni 2026  
**Penulis:** MiMo-v2.5 (Xiaomi LLM Core Team)  
**Fokus:** Manajemen Pelanggan

---

## 1. Overview

### 1.1 Latar Belakang
Barbershop kecil membutuhkan cara sederhana untuk mengelola data pelanggan mereka. Dengan mengetahui **siapa pelanggan**, **seberapa sering mereka datang**, dan **kapan terakhir kali berkunjung**, pemilik barbershop dapat membuat keputusan bisnis yang lebih baik - kapan harus memberikan promo, pelanggan mana yang perlu diprioritaskan, dan bagaimana meningkatkan retensi pelanggan.

### 1.2 Target Pengguna
- Pemilik barbershop kecil (1-3 cabang)
- Resepsionis/admin yang mengelola data pelanggan

### 1.3 Tujuan Produk (MVP)
1. **Mencatat data pelanggan** secara lengkap dan terpusat
2. **Melacak frekuensi kunjungan** setiap pelanggan
3. **Menganalisis pola kunjungan** untuk insight bisnis
4. **Membantu keputusan promosi** berdasarkan data pelanggan
5. **Meningkatkan retensi pelanggan** dengan tracking yang akurat

---

## 2. Fitur Utama MVP

### 2.1 Modul Database Pelanggan

#### 2.1.1 Data yang Dicatat
| Field | Tipe | Keterangan |
|-------|------|------------|
| `id` | UUID | ID unik pelanggan |
| `nama_lengkap` | String | Wajib diisi |
| `nomor_telepon` | String | Unique, untuk identifikasi |
| `whatsapp` | String | Opsional, untuk notifikasi |
| `tanggal_lahir` | Date | Untuk promo ulang tahun |
| `jenis_kelamin` | Enum | Laki-laki / Perempuan |
| `alamat` | String | Opsional |
| `catatan` | Text | Gaya rambut favorit, alergi, dll |
| `foto_profil` | String | URL/base64 foto |
| `tanggal_daftar` | DateTime | Auto-generated |
| `terakhir_kunjungan` | DateTime | Auto-update saat check-in |
| `total_kunjungan` | Integer | Auto-increment |
| `status` | Enum | Aktif / Tidak Aktif / VIP |

#### 2.1.2 Fitur CRUD
- **Create**: Form tambah pelanggan baru (manual atau import)
- **Read**: Tabel pelanggan dengan pencarian & filter
- **Update**: Edit data pelanggan
- **Delete**: Hapus pelanggan (soft delete, data tidak benar-benar hilang)

### 2.2 Modul Tracking Kunjungan

#### 2.2.1 Check-in Pelanggan
- **Quick check-in**: Ketik nomor telepon → sistem langsung kenali
- **Auto-update**: `total_kunjungan` +1, `terakhir_kunjungan` = sekarang
- **Riwayat check-in**: Log setiap kunjungan dengan timestamp

#### 2.2.2 Data Kunjungan yang Terekam
| Field | Keterangan |
|-------|------------|
| `id` | ID unik kunjungan |
| `pelanggan_id` | Relasi ke tabel pelanggan |
| `tanggal_waktu` | Kapan check-in dilakukan |
| `barber` | Nama barber yang melayani (input manual) |
| `layanan` | Layanan yang dipilih (input manual) |
| `catatan_kunjungan` | Catatan opsional |

#### 2.2.3 Riwayat Kunjungan
- **Timeline per pelanggan**: Lihat semua kunjungan dalam urutan kronologis
- **Filter**: Berdasarkan tanggal, barber, atau layanan
- **Statistik**: Rata-rata kunjungan per bulan, kunjungan terakhir

### 2.3 Modul Analitik & Insight

#### 2.3.1 Dashboard Utama
```
┌─────────────────────────────────────────────────┐
│  📊 RINGKASAN HARI INI                          │
├─────────────────────────────────────────────────┤
│  👥 Total Pelanggan: 245                        │
│  ✅ Check-in Hari Ini: 12                       │
│  📈 Pelanggan Aktif (30 hari): 89              │
│  ⚠️ Pelanggan Tidak Aktif (60+ hari): 45       │
└─────────────────────────────────────────────────┘
```

#### 2.3.2 Kategori Pelanggan (Otomatis)
Berdasarkan frekuensi kunjungan, sistem otomatis mengkategorikan:

| Kategori | Kriteria | Keterangan |
|----------|----------|------------|
| 🌟 **VIP** | ≥ 10 kunjungan/bulan | Pelanggan paling loyal |
| 💎 **Loyal** | 4-9 kunjungan/bulan | Pelanggan tetap |
| 👍 **Aktif** | 1-3 kunjungan/bulan | Pelanggan reguler |
| ⚠️ **Kurang Aktif** | 1 kunjungan/2 bulan | Perlu diberi perhatian |
| 🔴 **Hilang** | ≥ 60 hari tidak datang | Berpotensi churn |

#### 2.3.3 Laporan Insight
1. **Laporan Frekuensi Kunjungan**
   - Pelanggan paling sering datang
   - Pelanggan yang sudah lama tidak datang
   - Rata-rata kunjungan per minggu/bulan

2. **Laporan Pola Kunjungan**
   - Hari tersibuk (Senin-Minggu)
   - Jam paling ramai
   - Tren kunjungan (naik/turun)

3. **Laporan Pelanggan Baru vs Lama**
   - Jumlah pelanggan baru per bulan
   - Retention rate (pelanggan yang kembali)
   - Churn rate (pelanggan yang berhenti datang)

### 2.4 Modul Promosi Berbasis Data

#### 2.4.1 Rekomendasi Otomatis
Sistem memberikan rekomendasi promosi berdasarkan data:

| Kondisi | Rekomendasi Promo |
|---------|-------------------|
| Pelanggan tidak datang 30 hari | "Diskon 20% untuk kunjungan berikutnya" |
| Pelanggan baru pertama kali | "Gratis upgrade layanan berikutnya" |
| Pelanggan ulang tahun bulan ini | "Happy Birthday! Diskon 30% spesial untuk Anda" |
| Pelanggan mencapai 20 kunjungan | "Selamat! Anda mendapat 1x cukur GRATIS" |
| Pelanggan VIP | "Undangan eksklusif untuk event kami" |

#### 2.4.2 Export Data untuk Promosi
- **Export daftar pelanggan** (CSV/Excel)
- **Export pelanggan tidak aktif** untuk kampanye re-engagement
- **Export pelanggan ulang tahun** untuk blast promo

### 2.5 Modul Pengaturan

#### 2.5.1 Profil Barbershop
- Nama barbershop
- Alamat
- Nomor telepon
- Logo

#### 2.5.2 Pengaturan Kategori
- Definisi kategori pelanggan (bisa diedit)
- Threshold kunjungan untuk setiap kategori
- Aturan auto-kategorisasi

#### 2.5.3 Backup & Restore
- **Backup otomatis** setiap hari (file .db)
- **Manual backup** kapan saja
- **Restore** dari file backup

---

## 3. Persyaratan Teknis

### 3.1 Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────┐
│                    FRONTEND (PWA)                        │
│  React/Vue.js + Tailwind CSS                            │
│  Ringan, responsive, akses via browser                  │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   BACKEND API                            │
│  Node.js (Express) atau Python (FastAPI)                │
│  REST API untuk komunikasi data                         │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   DATABASE                               │
│  SQLite (barbershop.db)                                 │
│  File-based, zero-config, ringan                       │
└─────────────────────────────────────────────────────────┘
```

### 3.2 Database Schema (SQLite)

```sql
-- Tabel Pelanggan
CREATE TABLE pelanggan (
    id TEXT PRIMARY KEY,
    nama_lengkap TEXT NOT NULL,
    nomor_telepon TEXT UNIQUE NOT NULL,
    whatsapp TEXT,
    tanggal_lahir DATE,
    jenis_kelamin TEXT CHECK(jenis_kelamin IN ('L', 'P')),
    alamat TEXT,
    catatan TEXT,
    foto_profil TEXT,
    tanggal_daftar DATETIME DEFAULT CURRENT_TIMESTAMP,
    terakhir_kunjungan DATETIME,
    total_kunjungan INTEGER DEFAULT 0,
    status TEXT CHECK(status IN ('aktif', 'tidak_aktif', 'vip')) DEFAULT 'aktif'
);

-- Tabel Kunjungan
CREATE TABLE kunjungan (
    id TEXT PRIMARY KEY,
    pelanggan_id TEXT NOT NULL,
    tanggal_waktu DATETIME DEFAULT CURRENT_TIMESTAMP,
    barber TEXT,
    layanan TEXT,
    catatan_kunjungan TEXT,
    FOREIGN KEY (pelanggan_id) REFERENCES pelanggan(id)
);

-- Tabel Pengaturan
CREATE TABLE pengaturan (
    key TEXT PRIMARY KEY,
    value TEXT,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### 3.3 Tech Stack

| Komponen | Teknologi | Alasan |
|----------|-----------|--------|
| **Frontend** | React + Tailwind CSS | Komponen reusable, styling cepat |
| **Backend** | Node.js (Express) | Simple, banyak contoh |
| **Database** | SQLite | Zero-config, file-based, ringan |
| **ORM** | Prisma | Type-safe, migrasi mudah |
| **Hosting** | Vercel (FE) + Railway (BE) | Gratis tier tersedia |

### 3.4 Kebutuhan Perangkat

**Server:**
- RAM: 512MB - 1GB
- Storage: 10GB
- Bandwidth: 100GB/bulan

**Client (Admin/Resepsionis):**
- Browser modern (Chrome/Firefox/Safari)
- RAM: 4GB
- Resolusi: 1024x768 minimum
- Koneksi internet: 3G+

---

## 4. User Interface (UI)

### 4.1 Struktur Halaman

```
📱 Mobile (Bottom Nav)          💻 Desktop (Sidebar)
┌─────────────────┐            ┌─────────────────────────┐
│ 🏠 Dashboard    │            │ 🏠 Dashboard            │
│ 👥 Pelanggan    │            │ 👥 Pelanggan            │
│ 📋 Check-in     │            │ 📋 Check-in             │
│ 📊 Laporan      │            │ 📊 Laporan              │
│ ⚙️ Pengaturan   │            │ 📈 Insight              │
└─────────────────┘            │ ⚙️ Pengaturan           │
                               └─────────────────────────┘
```

### 4.2 Wireframe Utama

#### Dashboard
```
┌─────────────────────────────────────────────────┐
│  📊 Dashboard Hari Ini                  [Profil] │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐      │
│  │ 👥 245   │  │ ✅ 12    │  │ ⚠️ 45    │      │
│  │ Pelanggan│  │ Check-in │  │ Hilang   │      │
│  │ Total    │  │ Hari Ini │  │ (60+ hr) │      │
│  └──────────┘  └──────────┘  └──────────┘      │
│                                                 │
│  📋 REKOMENDASI PROMO                           │
│  ┌─────────────────────────────────────────┐    │
│  │ • 8 pelanggan belum datang 30+ hari     │    │
│  │ • 3 pelanggan berulang tahun bulan ini  │    │
│  │ • 5 pelanggan baru belum check-in lagi  │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  🕐 CHECK-IN TERAKHIR                          │
│  ┌─────────────────────────────────────────┐    │
│  │ 10:30 - Andi Saputra (32x kunjungan)   │    │
│  │ 10:15 - Budi Santoso (8x kunjungan)    │    │
│  │ 10:00 - Rina Wati (1x kunjungan)       │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
└─────────────────────────────────────────────────┘
```

#### Daftar Pelanggan
```
┌─────────────────────────────────────────────────┐
│  👥 Pelanggan                          [+ Baru]  │
├─────────────────────────────────────────────────┤
│ 🔍 [Cari nama/telepon...        ] [Filter ▼]    │
├─────────────────────────────────────────────────┤
│ ☐  │ Nama          │ Telepon    │ Kunjungan │ Status
├─────────────────────────────────────────────────┤
│ ☐  │ Andi Saputra  │ 0812xxxxx │ 32x       │ 🌟 VIP
│ ☐  │ Budi Santoso  │ 0856xxxxx │ 8x        │ 💎 Loyal
│ ☐  │ Rina Wati     │ 0878xxxxx │ 1x        │ 👍 Aktif
│ ☐  │ Dedi Kurnia   │ 0821xxxxx │ 0x        │ 🔴 Hilang
│ ...                                              │
├─────────────────────────────────────────────────┤
│ Menampilkan 1-20 dari 245    [< 1 2 3 ... 13 >] │
└─────────────────────────────────────────────────┘
```

#### Form Check-in
```
┌─────────────────────────────────────────────────┐
│  📋 Check-in Pelanggan                    [✕]   │
├─────────────────────────────────────────────────┤
│                                                 │
│  🔍 Nomor Telepon: [________________] [Cari]    │
│                                                 │
│  ┌─────────────────────────────────────────┐    │
│  │ 👤 Andi Saputra                         │    │
│  │ 📱 0812-XXXX-XXXX                       │    │
│  │ 📊 32x kunjungan | VIP                  │    │
│  │ 📅 Terakhir: 15 Jun 2026                │    │
│  └─────────────────────────────────────────┘    │
│                                                 │
│  Barber:    [___________ ▼]                     │
│  Layanan:   [___________ ▼]                     │
│  Catatan:   [________________]                  │
│                                                 │
│  [❌ Batal]              [✅ Check-in Sekarang]  │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## 5. Rencana Pengembangan

### 5.1 MVP (Fase 1) - 4-6 Minggu

| Minggu | Task | Output |
|--------|------|--------|
| 1 | Setup project, database schema | Project structure, DB ready |
| 2 | Backend API (CRUD pelanggan) | API endpoint siap |
| 3 | Frontend - Dashboard & Daftar Pelanggan | 2 halaman utama |
| 4 | Frontend - Form Check-in & Riwayat | Fitur check-in berfungsi |
| 5 | Modul Laporan & Insight dasar | Dashboard analitik |
| 6 | Testing, bug fixing, deploy | MVP ready |

### 5.2 Fitur MVP

✅ **Include:**
- CRUD pelanggan (tambah, edit, hapus, lihat)
- Check-in cepat dengan pencarian telepon
- Riwayat kunjungan per pelanggan
- Auto-kategorisasi pelanggan (VIP/Loyal/Aktif/Tidak Aktif/Hilang)
- Dashboard dengan ringkasan
- Laporan dasar (pelanggan aktif, tidak aktif)
- Rekomendasi promo sederhana
- Backup & restore database
- Export data ke CSV

❌ **Exclude (Fase 2):**
- Integrasi WhatsApp
- Notifikasi otomatis
- Multi-cabang
- Program loyalitas otomatis
- Aplikasi mobile native

---

## 6. Model Bisnis (MVP)

### 6.1 Pricing Sederhana

| Tier | Harga | Fitur |
|------|-------|-------|
| **Free** | Gratis | 50 pelanggan, 1 user |
| **Basic** | Rp 99.000/bulan | 500 pelanggan, 3 user |
| **Pro** | Rp 199.000/bulan | Unlimited pelanggan, 10 user |

### 6.2 Target Awal
- **Bulan 1-3**: 20 barbershop (Free tier)
- **Bulan 4-6**: 10 bayar (Basic/Pro) = Rp 1-2 juta/bulan
- **Tahun 1**: 50 bayar = Rp 5-10 juta/bulan

---

## 7. Success Metrics

| Metrik | Target MVP | Cara Ukur |
|--------|------------|-----------|
| Jumlah barbershop daftar | 50 dalam 3 bulan | Database user |
| Check-in per hari | 10 per barbershop | Log check-in |
| Retention rate | > 60% user aktif bulan ke-2 | Analytics |
| NPS (kepuasan) | > 40 | Survey |

---

## 8. Database Design (Lengkap)

### 8.1 Entity Relationship

```
┌─────────────────┐       ┌─────────────────┐
│    PELANGGAN    │       │    KUNJUNGAN    │
├─────────────────┤       ├─────────────────┤
│ id (PK)         │──┐    │ id (PK)         │
│ nama_lengkap    │  │    │ pelanggan_id(FK)│
│ nomor_telepon   │  │    │ tanggal_waktu   │
│ whatsapp        │  └───>│ barber          │
│ tanggal_lahir   │       │ layanan         │
│ jenis_kelamin   │       │ catatan         │
│ alamat          │       └─────────────────┘
│ catatan         │
│ foto_profil     │       ┌─────────────────┐
│ tanggal_daftar  │       │   PENGATURAN    │
│ terakhir_kunjung│       ├─────────────────┤
│ total_kunjungan │       │ key (PK)        │
│ status          │       │ value           │
└─────────────────┘       │ updated_at      │
                          └─────────────────┘
```

---

## 9. API Endpoints (Preview)

### 9.1 Pelanggan
```
GET    /api/pelanggan          - List semua pelanggan
GET    /api/pelanggan/:id      - Detail pelanggan
POST   /api/pelanggan          - Tambah pelanggan baru
PUT    /api/pelanggan/:id      - Update pelanggan
DELETE /api/pelanggan/:id      - Hapus pelanggan (soft delete)
GET    /api/pelanggan/search?q= - Cari pelanggan
```

### 9.2 Check-in
```
POST   /api/check-in           - Check-in pelanggan
GET    /api/check-in/hari-ini  - Check-in hari ini
GET    /api/check-in/riwayat/:id - Riwayat kunjungan pelanggan
```

### 9.3 Laporan
```
GET    /api/laporan/dashboard  - Data dashboard
GET    /api/laporan/frekuensi  - Laporan frekuensi kunjungan
GET    /api/laporan/pola       - Laporan pola kunjungan
GET    /api/laporan/export     - Export data (CSV)
```

---

**Catatan:** PRD ini fokus pada MVP Manajemen Pelanggan. Fitur lain (booking, kasir, inventaris) akan ditambahkan di fase pengembangan berikutnya.

**Status:** Siap untuk review dan validasi dengan calon pengguna.
