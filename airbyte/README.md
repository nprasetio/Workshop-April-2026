# 🔄 Airbyte — Panduan Workshop

**Sesi 1 & 2 | Total: 110 menit**

Airbyte adalah platform **ELT (Extract, Load, Transform)** open source yang memungkinkan kamu memindahkan data dari ratusan sumber ke destination pilihanmu — tanpa coding, cukup lewat Web UI.

---

## 🎯 Tujuan Pembelajaran

Setelah sesi ini, peserta mampu:
- Memahami konsep ELT dan perbedaannya dengan ETL
- Menavigasi Airbyte Web UI (Sources, Destinations, Connections, Jobs)
- Menghubungkan file CSV sebagai Source di Airbyte
- Menghubungkan Neon PostgreSQL sebagai Destination
- Menjalankan sync manual dan memverifikasi hasilnya
- Memahami perbedaan Full Refresh vs Incremental sync
- Mengaktifkan Basic Normalization
- Menjadwalkan sync otomatis dan membaca Job logs

---

## 📋 Silabus Sesi Airbyte

### Sesi 1 — Import & Sync Data (60 menit)
1. Pengenalan konsep ELT dan Airbyte
2. Orientasi Airbyte Web UI
3. Setup Source: File CSV (orders, products, order_items)
4. Setup Destination: Neon PostgreSQL
5. Membuat Connection & menjalankan sync pertama
6. Verifikasi data di Neon SQL Editor

### Sesi 2 — Airbyte Advanced (50 menit)
1. Full Refresh vs Incremental Sync — kapan pakai masing-masing
2. Basic Normalization — flatten data otomatis
3. Scheduled Sync — otomatisasi pipeline
4. Monitoring & Job Logs — debugging error
5. Tanya jawab

---

## 📁 File di Folder Ini

| File | Isi |
|------|-----|
| `README.md` | Dokumen ini — overview dan panduan umum |
| `step-by-step.md` | Instruksi langkah demi langkah detail |
| `sql-queries.md` | Query SQL untuk verifikasi data setelah sync |

---

## 🔗 Referensi

- Dokumentasi resmi: [docs.airbyte.com](https://docs.airbyte.com)
- abctl (deployment tool): [docs.airbyte.com/platform/deploying-airbyte/abctl](https://docs.airbyte.com/platform/deploying-airbyte/abctl)
