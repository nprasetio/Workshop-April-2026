# 📊 Metabase — Step-by-Step Guide

> **Prasyarat:** Metabase sudah berjalan di Hugging Face Spaces. Data sudah masuk ke Neon PostgreSQL via Airbyte. Buka URL Space Metabase kamu di browser.

---

## SESI 3: Eksplorasi & Visualisasi (70 menit)

---

### Step 1 — Orientasi Metabase Web UI (2:00–2:10)

#### Setup Awal (jika pertama kali)

1. Buka URL Space Metabase → muncul wizard setup
2. Pilih bahasa **English**
3. Isi data akun admin: nama, email, password
4. Pada langkah **"Add your data"** → pilih **PostgreSQL**

#### Koneksi ke Neon PostgreSQL

| Field | Nilai |
|-------|-------|
| **Display name** | `Neon Workshop DB` |
| **Host** | `ep-xxxxx.ap-southeast-1.aws.neon.tech` |
| **Port** | `5432` |
| **Database name** | `airbyte_dest` |
| **Username** | *(dari Neon)* |
| **Password** | *(dari Neon)* |
| **SSL** | Aktifkan ✅ |

Klik **Connect** → jika berhasil, lanjut ke **Take me to Metabase**

#### Menu Utama Metabase

| Menu | Fungsi |
|------|--------|
| **Home** | Halaman utama, recent items |
| **Browse data** | Lihat semua tabel di database |
| **New** (+ tombol) | Buat Question atau Dashboard baru |
| **Our analytics** | Semua Question dan Dashboard yang tersimpan |

---

### Step 2 — Browse Data & Eksplorasi Tabel (2:05–2:10)

1. Klik **Browse data** di navbar atas
2. Pilih **Neon Workshop DB**
3. Kamu akan melihat tabel: `orders`, `products`, `order_items`
4. Klik tabel `orders` → Metabase otomatis menampilkan preview data
5. Perhatikan:
   - Kolom-kolom yang tersedia
   - Sample data 10 baris pertama
   - Tipe data setiap kolom

---

### Step 3 — Query Builder: Tanpa SQL (2:10–2:25)

Query Builder memungkinkan kamu membuat query dengan klik — tidak perlu menulis SQL.

#### Contoh: Hitung Order per Region

1. Klik **+ New** → **Question**
2. Pilih **Simple question**
3. Pilih tabel **orders**
4. Klik **Summarize** (kanan atas)
5. Di panel Summarize:
   - **Count of rows** sudah dipilih secara default → biarkan
   - Klik **Add a grouping** → pilih `region`
6. Klik **Visualize**
7. Di panel kanan, pilih tipe chart: **Bar chart**
8. Klik **Save** → beri nama: `Order per Region`

#### Contoh: Filter Orders Completed Saja

1. Dari tabel orders, klik **Filter**
2. Pilih kolom `status`
3. Pilih `Is` → ketik atau pilih `completed`
4. Klik **Add filter**
5. Klik **Summarize** → Count → Group by `order_date` (by Month)
6. Klik **Visualize** → pilih **Line chart**
7. Simpan sebagai: `Revenue per Bulan (completed)`

---

### Step 4 — Native SQL Editor (2:25–2:45)

Untuk query yang lebih kompleks (JOIN, subquery, window function):

1. Klik **+ New** → **Question**
2. Pilih **Native query**
3. Pastikan database: `Neon Workshop DB` terpilih
4. Tulis query di editor

#### Contoh Query: TOP 5 Produk Terlaris

```sql
SELECT
  p.product_name,
  p.category,
  SUM(oi.quantity) AS total_terjual,
  SUM(oi.subtotal) AS total_revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders o   ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
GROUP BY p.product_name, p.category
ORDER BY total_revenue DESC
LIMIT 5;
```

5. Klik tombol **Run query** (atau Ctrl+Enter / Cmd+Enter)
6. Pilih visualisasi yang sesuai
7. Klik **Save** → beri nama

#### Tips SQL Editor

- **Ctrl+Enter** / **Cmd+Enter** → jalankan query
- Klik nama kolom di panel kiri untuk insert ke editor
- Klik ikon 📌 untuk menyimpan snippet SQL yang sering dipakai

---

### Step 5 — Membuat Dashboard Pertama (2:45–3:00)

1. Klik **+ New** → **Dashboard**
2. Beri nama: `Sales Overview Dashboard`
3. Klik **Add a question**
4. Cari dan pilih question yang sudah dibuat:
   - `Order per Region`
   - `Revenue per Bulan (completed)`
5. Atur posisi dan ukuran card dengan drag-and-drop
6. Klik **Save**

#### Tips Layout Dashboard

- Resize card: tarik sudut kanan bawah card
- Pindah card: klik dan drag judul card
- Hapus card: klik ikon ✕ di pojok card

---

### Step 6 — Filter & Drill-down (3:00–3:10)

#### Menambahkan Filter ke Dashboard

1. Buka dashboard → klik **Edit** (pojok kanan atas)
2. Klik **Add a filter**
3. Pilih **Date range** → untuk filter berdasarkan tanggal
4. Muncul panel kanan "Where should this filter go?"
5. Untuk setiap card, hubungkan filter ke kolom yang relevan:
   - Card `Revenue per Bulan` → hubungkan ke `order_date`
   - Card `Order per Region` → hubungkan ke `order_date`
6. Klik **Done** → klik **Save**

#### Menambahkan Filter Dropdown Region

1. Edit dashboard → **Add a filter**
2. Pilih **Text or Category**
3. Hubungkan ke kolom `region` di setiap card yang relevan
4. Simpan

#### Drill-through (klik chart untuk detail)

Klik salah satu bar di chart `Order per Region` → Metabase otomatis menampilkan data detail untuk region tersebut.

---

## SESI 4: Studi Kasus Bisnis (50 menit)

---

### Kasus 1 — Sales Performance (3:10–3:20)

**Pertanyaan bisnis:** Bagaimana tren penjualan per bulan?

Buat question baru dengan query ini:

```sql
SELECT
  TO_CHAR(order_date, 'YYYY-MM') AS bulan,
  COUNT(*) AS total_order,
  SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END) AS order_selesai,
  SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) AS order_batal,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS revenue,
  ROUND(
    SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1
  ) AS cancel_rate_pct
FROM orders
GROUP BY 1
ORDER BY 1;
```

**Visualisasi yang dibuat:**
- Line chart: revenue per bulan
- KPI card: total revenue Jan–Apr
- KPI card: cancel rate rata-rata

---

### Kasus 2 — Product Analysis (3:20–3:30)

**Pertanyaan bisnis:** Produk dan kategori mana yang paling profitable?

```sql
SELECT
  p.product_name,
  p.category,
  SUM(oi.quantity)  AS total_qty,
  SUM(oi.subtotal)  AS total_revenue,
  ROUND(AVG(oi.discount_pct), 1) AS avg_diskon,
  COUNT(DISTINCT oi.order_id) AS muncul_di_n_order
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders   o ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
GROUP BY p.product_name, p.category
ORDER BY total_revenue DESC
LIMIT 10;
```

**Visualisasi yang dibuat:**
- Bar chart horizontal: TOP 10 produk
- Pie/Donut chart: revenue per kategori

---

### Kasus 3 — Regional Sales (3:30–3:40)

**Pertanyaan bisnis:** Wilayah mana paling banyak bertransaksi?

```sql
SELECT
  region,
  payment_method,
  COUNT(*) AS jumlah_order,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS revenue,
  ROUND(
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (PARTITION BY region), 1
  ) AS pct_dalam_region
FROM orders
WHERE status = 'completed'
GROUP BY region, payment_method
ORDER BY region, jumlah_order DESC;
```

**Visualisasi yang dibuat:**
- Bar chart: revenue per region
- Stacked bar: komposisi payment method per region

---

### Kasus 4 — Customer Behavior (3:40–3:50)

**Pertanyaan bisnis:** Siapa pelanggan terbaik? Berapa rata-rata item per order?

```sql
-- Top 10 Customer
SELECT
  customer_id,
  COUNT(*) AS total_order,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS total_belanja,
  MIN(order_date::DATE) AS first_order,
  MAX(order_date::DATE) AS last_order
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 1
ORDER BY total_belanja DESC
LIMIT 10;
```

```sql
-- Basket Analysis
SELECT
  o.order_id,
  o.region,
  COUNT(oi.item_id)   AS jumlah_item,
  SUM(oi.quantity)    AS total_unit,
  ROUND(AVG(oi.discount_pct), 1) AS avg_diskon,
  SUM(oi.subtotal)    AS total_transaksi
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.status = 'completed'
GROUP BY o.order_id, o.region
ORDER BY jumlah_item DESC;
```

**Visualisasi yang dibuat:**
- Table: leaderboard top 10 customer
- Histogram: distribusi items per order

---

### Final Dashboard: Sales Command Center (3:50–4:00)

Buat dashboard baru bernama **Sales Command Center** dan gabungkan:

| Posisi | Chart | Ukuran |
|--------|-------|--------|
| Row 1 | KPI: Total Revenue | Half |
| Row 1 | KPI: Total Order + Cancel Rate | Half |
| Row 2 | Line: Revenue per Bulan | Full |
| Row 3 | Bar H: TOP 10 Produk | Half |
| Row 3 | Donut: Revenue per Kategori | Half |
| Row 4 | Bar: Revenue per Region | Half |
| Row 4 | Stacked: Payment per Region | Half |
| Row 5 | Table: Top 10 Customer | Full |

**Filter yang ditambahkan:**
- Date range → terhubung ke semua card dengan `order_date`
- Dropdown Region → terhubung ke semua card dengan `region`

---

## ✅ Checklist Sesi Metabase

- [ ] Bisa login ke Metabase Web UI
- [ ] Koneksi ke Neon PostgreSQL berhasil
- [ ] Bisa lihat tabel orders, products, order_items di Browse Data
- [ ] Membuat 1 question dengan Query Builder (tanpa SQL)
- [ ] Membuat 1 question dengan Native SQL (dengan JOIN)
- [ ] Dashboard pertama berhasil dibuat dengan 2+ card
- [ ] Filter date range berhasil ditambahkan ke dashboard
- [ ] 4 studi kasus berhasil dibuat sebagai Question
- [ ] Sales Command Center Dashboard selesai
