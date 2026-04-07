# 📋 Isi Google Form — Workshop Data Engineering

Dokumen ini berisi jawaban siap-pakai untuk Google Form penyelenggara workshop.
**Copy-paste langsung ke form.** Airbyte dan Metabase dipisah sesuai permintaan form.

---

---

# 🔄 WORKSHOP 1: AIRBYTE

---

## ① Deskripsi/Overview Workshop

```
Workshop ini memperkenalkan Airbyte, platform ELT (Extract, Load, Transform) open source yang digunakan untuk memindahkan data dari berbagai sumber ke database tujuan secara otomatis melalui Web UI — tanpa perlu menulis kode.

Peserta akan belajar cara menggunakan Airbyte secara langsung (hands-on): mulai dari mengenal antarmuka Web UI, menghubungkan file CSV sebagai sumber data, menyinkronkan data ke Neon PostgreSQL, hingga mengonfigurasi penjadwalan sync otomatis.

Workshop ini relevan karena pipeline data adalah fondasi dari setiap proyek analitik dan BI. Dengan menguasai Airbyte, peserta bisa membangun aliran data yang andal dan berulang tanpa bergantung pada engineer lain — cukup lewat klik di browser.
```

---

## ② Silabus/Topik yang Akan Dibahas

```
1. Pengenalan konsep ELT dan perbedaannya dengan ETL
2. Orientasi Airbyte Web UI (Sources, Destinations, Connections, Jobs)
3. Setup Source: menghubungkan file CSV (orders, products, order_items)
4. Setup Destination: menghubungkan Neon PostgreSQL sebagai tujuan data
5. Membuat Connection dan menjalankan sync pertama
6. Verifikasi data hasil sync di Neon SQL Editor
7. Full Refresh vs Incremental Sync — kapan pakai masing-masing
8. Basic Normalization — flatten data otomatis
9. Scheduled Sync — otomatisasi pipeline tanpa trigger manual
10. Monitoring dan membaca Job Logs untuk debugging
```

---

## ③ Agenda

```
1. Pembukaan & Perkenalan (5 menit)
2. Pengenalan Konsep ELT dan Airbyte (10 menit)
3. Orientasi Airbyte Web UI (10 menit)
4. Hands-on: Setup Source — 3 File CSV (15 menit)
5. Hands-on: Setup Destination — Neon PostgreSQL (15 menit)
6. Hands-on: Membuat Connection & Sync Pertama (15 menit)
7. Verifikasi Data di Neon SQL Editor (5 menit)
8. Full Refresh vs Incremental Sync (15 menit)
9. Basic Normalization (15 menit)
10. Scheduled Sync & Monitoring Job Logs (10 menit)
11. Q&A (5 menit)
```

---

## ④ Prerequisites

```
Tidak ada prasyarat teknis khusus — workshop ini terbuka untuk semua level.

Yang sebaiknya dimiliki peserta:
- Laptop dengan browser Google Chrome (terbaru)
- Akun Hugging Face (gratis) — sudah dibuat sebelum workshop
- Akun Neon PostgreSQL (gratis) — sudah dibuat sebelum workshop
- Pemahaman dasar tentang apa itu database dan tabel (tidak wajib, tapi membantu)

Catatan: Instalasi Airbyte di Hugging Face Spaces dan setup Neon PostgreSQL sudah disiapkan oleh fasilitator sebelum workshop dimulai. Peserta tidak perlu melakukan instalasi apapun di laptop masing-masing.
```

---

## ⑤ Tools yang Perlu Diinstal

```
- Hugging Face (web, semua OS) — platform hosting Airbyte: huggingface.co
- Neon PostgreSQL (web, semua OS) — free database: neon.tech
- DBeaver Community Edition (versi terbaru, Win/Mac/Linux) — GUI database client untuk verifikasi data: dbeaver.io
- Google Chrome (versi terbaru, semua OS) — browser yang direkomendasikan

Catatan: Airbyte diakses via browser, tidak perlu diinstal di laptop peserta.
```

---

## ⑥ Panduan Instalasi Tools

```
- Panduan instalasi DBeaver: https://dbeaver.io/download/
- Panduan setup Neon PostgreSQL (ada di repo): https://github.com/[USERNAME]/workshop-data-engineering/blob/main/airbyte/step-by-step.md
- Panduan lengkap step-by-step workshop Airbyte: https://github.com/[USERNAME]/workshop-data-engineering/blob/main/airbyte/step-by-step.md
```

---

## ⑦ Link Download Tools & Dataset

```
Dataset (CSV files):
https://github.com/[USERNAME]/workshop-data-engineering/raw/main/dataset/orders.csv
https://github.com/[USERNAME]/workshop-data-engineering/raw/main/dataset/products.csv
https://github.com/[USERNAME]/workshop-data-engineering/raw/main/dataset/order_items.csv

Panduan Lengkap (GitHub):
https://github.com/[USERNAME]/workshop-data-engineering

Step-by-Step Guide Airbyte:
https://github.com/[USERNAME]/workshop-data-engineering/blob/main/airbyte/step-by-step.md

Tools:
https://dbeaver.io/download/
https://neon.tech
https://huggingface.co
```

---
---

# 📊 WORKSHOP 2: METABASE

---

## ① Deskripsi/Overview Workshop

```
Workshop ini memperkenalkan Metabase, platform Business Intelligence (BI) open source yang memungkinkan siapa pun — termasuk yang tidak memiliki latar belakang teknis — membuat visualisasi data dan dashboard interaktif langsung dari database.

Peserta akan belajar dua cara mengeksplorasi data di Metabase: menggunakan Query Builder berbasis klik (tanpa SQL) dan Native SQL Editor untuk query yang lebih kompleks. Di akhir sesi, peserta akan membangun Sales Command Center Dashboard dari data e-commerce nyata yang sudah disinkronkan oleh Airbyte.

Workshop ini relevan karena kemampuan membaca dan menyajikan data dalam bentuk visual adalah skill penting di era data-driven. Dengan Metabase, keputusan bisnis bisa diambil lebih cepat berdasarkan fakta, bukan asumsi.
```

---

## ② Silabus/Topik yang Akan Dibahas

```
1. Pengenalan konsep Business Intelligence (BI) dan kegunaan Metabase
2. Orientasi Metabase Web UI (Browse Data, Questions, Dashboards)
3. Koneksi Metabase ke Neon PostgreSQL
4. Query Builder: eksplorasi data tanpa SQL (filter, group by, summarize)
5. Memilih tipe visualisasi yang tepat (bar, line, pie, scatter, table)
6. Native SQL Editor: menulis query JOIN antar tabel
7. Menyimpan hasil query sebagai Question
8. Membuat dan mengatur dashboard interaktif
9. Menambahkan filter dan drill-down ke dashboard
10. Studi Kasus 1: Sales Performance (tren penjualan bulanan)
11. Studi Kasus 2: Product Analysis (produk & kategori terlaris)
12. Studi Kasus 3: Regional Sales (performa per wilayah)
13. Studi Kasus 4: Customer Behavior (pelanggan & pola belanja)
14. Final Project: membangun Sales Command Center Dashboard
```

---

## ③ Agenda

```
1. Pembukaan & Perkenalan (5 menit)
2. Pengenalan Konsep BI dan Metabase (10 menit)
3. Koneksi Metabase ke Neon PostgreSQL (5 menit)
4. Orientasi Metabase Web UI — Browse Data (5 menit)
5. Hands-on: Query Builder tanpa SQL (15 menit)
6. Hands-on: Native SQL Editor dengan JOIN (20 menit)
7. Hands-on: Membuat Dashboard & Menambahkan Filter (20 menit)
8. Hands-on: Studi Kasus 1 — Sales Performance (10 menit)
9. Hands-on: Studi Kasus 2 — Product Analysis (10 menit)
10. Hands-on: Studi Kasus 3 — Regional Sales (10 menit)
11. Hands-on: Studi Kasus 4 — Customer Behavior (10 menit)
12. Hands-on: Final Dashboard Sales Command Center (10 menit)
13. Q&A & Wrap-up (10 menit)
```

---

## ④ Prerequisites

```
Tidak ada prasyarat teknis yang wajib — workshop ini terbuka untuk semua level.

Yang sebaiknya dimiliki peserta:
- Laptop dengan browser Google Chrome (terbaru)
- Sudah mengikuti sesi Airbyte sebelumnya (atau data sudah tersedia di Neon PostgreSQL)
- Pemahaman dasar SQL (SELECT, FROM, WHERE) akan sangat membantu, tapi tidak wajib karena kita juga belajar Query Builder tanpa SQL

Catatan: Metabase sudah berjalan di Hugging Face Spaces dan sudah terhubung ke Neon PostgreSQL. Peserta tidak perlu melakukan instalasi apapun.
```

---

## ⑤ Tools yang Perlu Diinstal

```
- Hugging Face (web, semua OS) — platform hosting Metabase: huggingface.co
- Neon PostgreSQL (web, semua OS) — database yang berisi data workshop: neon.tech
- Google Chrome (versi terbaru, semua OS) — browser yang direkomendasikan

Catatan: Metabase diakses via browser, tidak perlu diinstal di laptop peserta.
```

---

## ⑥ Panduan Instalasi Tools

```
Tidak ada instalasi di laptop peserta yang dibutuhkan — semua tools diakses via browser.

Panduan koneksi Metabase ke Neon PostgreSQL ada di sini:
https://github.com/[USERNAME]/workshop-data-engineering/blob/main/metabase/step-by-step.md

Panduan lengkap step-by-step workshop Metabase:
https://github.com/[USERNAME]/workshop-data-engineering/blob/main/metabase/step-by-step.md

Seluruh SQL query studi kasus:
https://github.com/[USERNAME]/workshop-data-engineering/blob/main/metabase/sql-queries.md
```

---

## ⑦ Link Download Tools & Dataset

```
Dataset (CSV files):
https://github.com/[USERNAME]/workshop-data-engineering/raw/main/dataset/orders.csv
https://github.com/[USERNAME]/workshop-data-engineering/raw/main/dataset/products.csv
https://github.com/[USERNAME]/workshop-data-engineering/raw/main/dataset/order_items.csv

Panduan Lengkap (GitHub):
https://github.com/[USERNAME]/workshop-data-engineering

Step-by-Step Guide Metabase:
https://github.com/[USERNAME]/workshop-data-engineering/blob/main/metabase/step-by-step.md

SQL Queries Studi Kasus:
https://github.com/[USERNAME]/workshop-data-engineering/blob/main/metabase/sql-queries.md

Tools (akses via browser):
https://neon.tech
https://huggingface.co
```

---

> 📝 **Catatan:** Ganti `[USERNAME]` dan `[REPO]` dengan username GitHub dan nama repo kamu setelah di-push.
