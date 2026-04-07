# 🔄 Workshop Data Engineering: Airbyte + Metabase

**End-to-End Data Pipeline** — dari file CSV ke dashboard bisnis interaktif.

> Workshop ini mengajarkan cara membangun pipeline data modern menggunakan **Airbyte** (ELT platform) dan **Metabase** (Business Intelligence), di-deploy di **Hugging Face Spaces** dengan **Neon PostgreSQL** sebagai database gratis.

---

## 📁 Struktur Repository

```
workshop-data-engineering/
├── README.md                    ← Kamu di sini
├── dataset/
│   ├── orders.csv               ← 80 transaksi Jan–Apr 2024
│   ├── products.csv             ← 30 produk, 8 kategori
│   └── order_items.csv          ← 136 baris detail item per order
├── airbyte/
│   ├── README.md                ← Panduan lengkap Airbyte
│   ├── step-by-step.md          ← Langkah demi langkah
│   └── sql-queries.md           ← Query verifikasi data
├── metabase/
│   ├── README.md                ← Panduan lengkap Metabase
│   ├── step-by-step.md          ← Langkah demi langkah
│   └── sql-queries.md           ← 4 studi kasus SQL
└── docs/
    ├── agenda.md                ← Agenda workshop lengkap
    └── gform-answers.md         ← Isi Google Form penyelenggara
```

---

## 🗂️ Dataset

Tema: **Toko Online Indonesia (E-Commerce)**

| File | Baris | Deskripsi |
|------|-------|-----------|
| `orders.csv` | 80 | Transaksi penjualan Jan–Apr 2024 |
| `products.csv` | 30 | Katalog produk (8 kategori) |
| `order_items.csv` | 136 | Detail item per order |

Relasi: `orders` ←→ `order_items` ←→ `products`

---

## 🗓️ Agenda Workshop (±4 Jam)

| Sesi | Durasi | Topik |
|------|--------|-------|
| **Sesi 1** | 60 mnt | Intro Airbyte + Import & Sync Data CSV |
| **Sesi 2** | 50 mnt | Airbyte Advanced: Incremental, Normalisasi, Scheduling |
| ☕ Break | 10 mnt | Istirahat |
| **Sesi 3** | 70 mnt | Intro Metabase + Eksplorasi & Visualisasi |
| **Sesi 4** | 50 mnt | Studi Kasus Bisnis + Final Dashboard |

---

## 🛠️ Tools yang Digunakan

- **Airbyte** — [huggingface.co/spaces](https://huggingface.co/spaces) (Docker)
- **Metabase** — [huggingface.co/spaces](https://huggingface.co/spaces) (Docker)
- **Neon PostgreSQL** — [neon.tech](https://neon.tech) (Free tier)
- **DBeaver** — GUI database client (opsional, untuk verifikasi)
- **Google Chrome** — Browser yang direkomendasikan

---

## ⚙️ Prasyarat

Instalasi Airbyte, Metabase di Hugging Face Spaces dan setup Neon PostgreSQL **sudah disiapkan sebelum workshop** oleh fasilitator. Peserta hanya butuh:
- Laptop/komputer dengan browser Google Chrome
- Akun Hugging Face (gratis): [huggingface.co](https://huggingface.co)
- Akun Neon (gratis): [neon.tech](https://neon.tech)

---

## 📚 Panduan per Tools

- 👉 [Panduan Airbyte](./airbyte/README.md)
- 👉 [Panduan Metabase](./metabase/README.md)

---

## 📝 Lisensi

MIT License — bebas digunakan untuk keperluan belajar dan workshop.
