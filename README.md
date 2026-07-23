# 🪒 BarberShop SaaS

**Sistem Manajemen Pelanggan untuk Barbershop**

[![Version](https://img.shields.io/badge/version-1.0--mvp-blue.svg)](https://github.com)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com)

---

## 📖 Tentang

**BarberShop SaaS** adalah aplikasi berbasis web yang membantu pemilik barbershop kecil (1-3 cabang) untuk mengelola data pelanggan secara efektif. Dengan sistem ini, Anda dapat:

- 📋 Mencatat data pelanggan secara lengkap dan terpusat
- 📊 Melacak frekuensi kunjungan setiap pelanggan
- 🎯 Menganalisis pola kunjungan untuk insight bisnis
- 🎁 Membantu keputusan promosi berdasarkan data pelanggan
- 💪 Meningkatkan retensi pelanggan dengan tracking yang akurat

---

## ✨ Fitur Utama

### 👥 Manajemen Pelanggan
- CRUD pelanggan (tambah, edit, hapus, lihat)
- Pencarian & filter data pelanggan
- Import/Export data CSV

### 📋 Check-in Pelanggan
- Quick check-in dengan nomor telepon
- Auto-update riwayat kunjungan
- Tracking barber & layanan

### 📊 Analitik & Insight
- Dashboard ringkasan harian
- Auto-kategorisasi pelanggan:
  - 🌟 **VIP** - ≥10 kunjungan/bulan
  - 💎 **Loyal** - 4-9 kunjungan/bulan
  - 👍 **Aktif** - 1-3 kunjungan/bulan
  - ⚠️ **Kurang Aktif** - 1 kunjungan/2 bulan
  - 🔴 **Hilang** - ≥60 hari tidak datang
- Laporan frekuensi & pola kunjungan

### 🎁 Promosi Berbasis Data
- Rekomendasi promo otomatis
- Pelanggan ulang tahun
- Pelanggan tidak aktif
- Program loyalitas

---

## 🛠 Tech Stack

| Komponen | Teknologi |
|----------|-----------|
| **Frontend** | React + Tailwind CSS |
| **Backend** | Node.js (Express) |
| **Database** | SQLite |
| **ORM** | Prisma |
| **Hosting** | Vercel (FE) + Railway (BE) |

---

## 📦 Instalasi

### Prerequisites
- Node.js v18+
- npm atau yarn

### Clone Repository
```bash
git clone https://github.com/username/barbershop-saas.git
cd barbershop-saas
```

### Install Dependencies
```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

### Setup Database
```bash
cd backend
npx prisma migrate dev
npx prisma db seed
```

### Jalankan Aplikasi
```bash
# Backend (port 5000)
cd backend
npm run dev

# Frontend (port 3000)
cd frontend
npm run dev
```

Aplikasi tersedia di: `http://localhost:3000`

---

## 🗄 Database Schema

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

## 🔌 API Endpoints

### Pelanggan
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/api/pelanggan` | List semua pelanggan |
| GET | `/api/pelanggan/:id` | Detail pelanggan |
| POST | `/api/pelanggan` | Tambah pelanggan baru |
| PUT | `/api/pelanggan/:id` | Update pelanggan |
| DELETE | `/api/pelanggan/:id` | Hapus pelanggan |

### Check-in
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| POST | `/api/check-in` | Check-in pelanggan |
| GET | `/api/check-in/hari-ini` | Check-in hari ini |
| GET | `/api/check-in/riwayat/:id` | Riwayat kunjungan |

### Laporan
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/api/laporan/dashboard` | Data dashboard |
| GET | `/api/laporan/frekuensi` | Laporan frekuensi |
| GET | `/api/laporan/export` | Export CSV |

---

## 💰 Pricing

| Tier | Harga | Fitur |
|------|-------|-------|
| **Free** | Gratis | 50 pelanggan, 1 user |
| **Basic** | Rp 99.000/bulan | 500 pelanggan, 3 user |
| **Pro** | Rp 199.000/bulan | Unlimited pelanggan, 10 user |

---

## 🗺 Roadmap

### MVP (Fase 1)
- [x] Setup project & database
- [ ] CRUD Pelanggan
- [ ] Fitur Check-in
- [ ] Dashboard & Analitik
- [ ] Rekomendasi Promo
- [ ] Deploy ke Production

### Fase 2
- [ ] Integrasi WhatsApp
- [ ] Notifikasi Otomatis
- [ ] Multi-cabang
- [ ] Program Loyalitas

### Fase 3
- [ ] Aplikasi Mobile Native
- [ ] Integrasi Payment Gateway
- [ ] Fitur Booking Online

---

## 📁 Struktur Proyek

```
barbershop-saas/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   └── utils/
│   ├── public/
│   └── package.json
├── backend/
│   ├── src/
│   │   ├── routes/
│   │   ├── controllers/
│   │   ├── models/
│   │   └── middleware/
│   ├── prisma/
│   └── package.json
└── README.md
```

---

## 🤝 Contributing

1. Fork repository
2. Buat branch baru (`git checkout -b feature/fitur-baru`)
3. Commit perubahan (`git commit -m 'Add fitur baru'`)
4. Push ke branch (`git push origin feature/fitur-baru`)
5. Buat Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 📧 Contact

**Tim Pengembang**
- Email: dev@barbershop-saas.com
- GitHub: [https://github.com/username/barbershop-saas](https://github.com)

---

Dibuat dengan ❤️ untuk barbershop Indonesia 🇮🇩
