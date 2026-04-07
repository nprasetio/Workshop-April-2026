# 🔄 Airbyte — Step-by-Step Guide

> **Prasyarat:** Airbyte sudah berjalan di Hugging Face Spaces dan Neon PostgreSQL sudah dibuat. Buka URL Space Airbyte kamu di browser.

---

## SESI 1: Import & Sync Data CSV (60 menit)

---

### Step 1 — Orientasi Airbyte Web UI (0:00–0:10)

Setelah login ke Airbyte, kamu akan melihat sidebar kiri dengan menu:

| Menu | Fungsi |
|------|--------|
| **Sources** | Tempat mendaftarkan sumber data |
| **Destinations** | Tempat mendaftarkan tujuan data |
| **Connections** | Pipeline dari source ke destination |
| **Jobs** | Riwayat dan status semua sync |
| **Settings** | Konfigurasi workspace |

**Login Credentials:**
Jalankan perintah ini di terminal Hugging Face Space untuk mendapatkan password:
```bash
abctl local credentials
```
Atau cek di tab **Logs** pada Space kamu.

---

### Step 2 — Setup Source: File CSV (0:10–0:25)

Kita akan menambahkan 3 file CSV sebagai source terpisah.

#### 2a. Tambah Source orders.csv

1. Klik **Sources** di sidebar → klik **+ New source**
2. Di search box, ketik `File` → pilih **File (CSV, JSON, Excel, Feather, Parquet)**
3. Isi form:
   - **Source name:** `orders-csv`
   - **URL:** *(paste URL raw file orders.csv dari GitHub repo ini)*
     ```
     https://raw.githubusercontent.com/[USERNAME]/[REPO]/main/dataset/orders.csv
     ```
   - **File Format:** `csv`
   - **Dataset Name:** `orders`
   - **Storage Provider:** `HTTPS: Public Web`
4. Klik **Test and save**
5. Tunggu hingga muncul ✅ **Connection test succeeded**

#### 2b. Tambah Source products.csv

Ulangi langkah yang sama:
- **Source name:** `products-csv`
- **URL:** `https://raw.githubusercontent.com/[USERNAME]/[REPO]/main/dataset/products.csv`
- **Dataset Name:** `products`

#### 2c. Tambah Source order_items.csv

- **Source name:** `order-items-csv`
- **URL:** `https://raw.githubusercontent.com/[USERNAME]/[REPO]/main/dataset/order_items.csv`
- **Dataset Name:** `order_items`

> 💡 **Tips:** Kamu bisa langsung ke file di GitHub, klik tombol **Raw**, copy URL-nya.

---

### Step 3 — Setup Destination: Neon PostgreSQL (0:25–0:40)

1. Klik **Destinations** di sidebar → klik **+ New destination**
2. Cari dan pilih **PostgreSQL**
3. Isi form dengan detail koneksi Neon kamu:

| Field | Nilai |
|-------|-------|
| **Destination name** | `Neon Workshop DB` |
| **Host** | `ep-xxxxx.ap-southeast-1.aws.neon.tech` |
| **Port** | `5432` |
| **DB Name** | `airbyte_dest` *(atau nama DB yang kamu buat)* |
| **Username** | *(dari Neon dashboard)* |
| **Password** | *(dari Neon dashboard)* |
| **SSL Mode** | `require` ⚠️ *wajib diisi!* |

4. Klik **Test and save**
5. Tunggu hingga muncul ✅ **Connection test succeeded**

> ⚠️ **Jika error:** Pastikan SSL mode = `require`. Ini penyebab paling umum koneksi Neon gagal.

---

### Step 4 — Membuat Connection & Menjalankan Sync (0:40–0:55)

Ulangi untuk ketiga source (orders, products, order_items):

#### Buat Connection untuk orders.csv

1. Klik **Connections** → **+ New connection**
2. **Select source:** pilih `orders-csv`
3. **Select destination:** pilih `Neon Workshop DB`
4. Konfigurasi sync:
   - **Replication frequency:** `Manual` *(untuk workshop)*
   - **Namespace:** biarkan default
   - **Prefix:** biarkan kosong
5. Di bagian **Activate the streams**, pastikan stream `orders` dicentang
6. **Sync mode:** pilih `Full refresh | Overwrite`
7. Klik **Set up connection**
8. Klik tombol **Sync now** (ikon play ▶️)

#### Pantau Progress Sync

1. Klik **Jobs** di sidebar
2. Cari job dengan nama connection yang baru dibuat
3. Status akan berubah: `Pending` → `Running` → `Succeeded` ✅
4. Klik job untuk melihat detail: berapa baris yang di-sync

> 🕐 Proses sync biasanya selesai dalam 1–2 menit untuk dataset kecil.

---

### Step 5 — Verifikasi Data di Neon (0:55–1:00)

1. Buka [neon.tech](https://neon.tech) → masuk ke project kamu
2. Klik **SQL Editor** di sidebar
3. Jalankan query berikut satu per satu:

```sql
-- Cek tabel yang tersedia
SELECT table_name
FROM information_schema.tables
WHERE table_schema NOT IN ('pg_catalog', 'information_schema')
ORDER BY table_name;

-- Cek data orders
SELECT * FROM orders LIMIT 5;

-- Cek jumlah baris
SELECT COUNT(*) FROM orders;
SELECT COUNT(*) FROM products;
SELECT COUNT(*) FROM order_items;
```

**Hasil yang diharapkan:**
- `orders` → 80 baris
- `products` → 30 baris
- `order_items` → 136 baris

---

## SESI 2: Airbyte Advanced (50 menit)

---

### Step 6 — Full Refresh vs Incremental Sync (1:00–1:15)

#### Apa bedanya?

| Mode | Cara Kerja | Kapan Pakai |
|------|-----------|-------------|
| **Full Refresh \| Overwrite** | Hapus semua data lama, import ulang dari awal | Data kecil, tidak ada cursor/timestamp |
| **Full Refresh \| Append** | Tambahkan semua data baru di atas data lama | Perlu riwayat perubahan |
| **Incremental \| Append** | Hanya ambil data baru berdasarkan cursor field | Data besar, ada kolom `updated_at` |
| **Incremental \| Deduped** | Ambil data baru + hapus duplikat berdasarkan primary key | Data production dengan update |

#### Demo: Ubah ke Incremental (jika source mendukung)

1. Buka Connection `orders-csv`
2. Klik tab **Replication**
3. Ubah sync mode ke `Incremental | Append`
4. Set **Cursor field:** `order_date`
5. Klik **Save changes**
6. Klik **Sync now** → bandingkan baris yang di-sync

---

### Step 7 — Basic Normalization (1:15–1:30)

Normalisasi mengubah data flat (satu baris = satu record) menjadi tabel yang lebih terstruktur.

#### Mengaktifkan Normalisasi

1. Buka Connection yang ingin diatur
2. Klik tab **Transformation**
3. Di bagian **Normalized tabular data**, aktifkan toggle ✅
4. Klik **Save changes**
5. Jalankan sync ulang

#### Perbedaan Tabel Hasil

Setelah normalisasi aktif, di Neon kamu akan melihat:
- `_airbyte_raw_orders` → data mentah (JSON string)
- `orders` → tabel normalized (kolom terpisah)

```sql
-- Bandingkan keduanya
SELECT * FROM _airbyte_raw_orders LIMIT 3;
SELECT * FROM orders LIMIT 3;
```

---

### Step 8 — Scheduled Sync (1:30–1:45)

Agar pipeline berjalan otomatis tanpa trigger manual:

1. Buka Connection
2. Klik tab **Settings**
3. Di bagian **Schedule**, pilih frekuensi:
   - `Every 24 hours` → untuk data harian
   - `Every 6 hours` → untuk data lebih sering
   - `Cron` → jadwal custom (misal: setiap hari jam 01.00 WIB)
4. Contoh Cron untuk setiap hari jam 01.00 WIB:
   ```
   0 18 * * *
   ```
   *(Jam 18.00 UTC = 01.00 WIB)*
5. Klik **Save changes**

Di halaman Jobs, kamu akan melihat kolom **Next sync** yang menunjukkan kapan sync berikutnya.

---

### Step 9 — Monitoring & Job Logs (1:45–1:50)

#### Membaca Status Job

| Status | Artinya |
|--------|---------|
| 🟡 `Pending` | Menunggu giliran dieksekusi |
| 🔵 `Running` | Sedang berjalan |
| ✅ `Succeeded` | Berhasil |
| ❌ `Failed` | Gagal — perlu diperiksa |
| ⚠️ `Cancelled` | Dibatalkan manual |

#### Cara Membaca Logs

1. Klik job yang berstatus **Failed**
2. Klik **Logs** untuk melihat detail error
3. Error umum dan solusinya:

| Error | Penyebab | Solusi |
|-------|----------|--------|
| `Connection refused` | Host salah atau DB mati | Cek host di Neon dashboard |
| `SSL required` | SSL mode tidak di-set | Ubah SSL mode ke `require` |
| `Authentication failed` | Username/password salah | Reset credentials di Neon |
| `URL not found` | URL file CSV salah | Pastikan pakai URL **raw** dari GitHub |

---

## ✅ Checklist Sesi Airbyte

- [ ] Bisa login ke Airbyte Web UI
- [ ] 3 Source CSV sudah terdaftar dan test succeeded
- [ ] 1 Destination Neon PostgreSQL sudah terdaftar dan test succeeded
- [ ] 3 Connection sudah dibuat (orders, products, order_items)
- [ ] Semua sync berstatus Succeeded
- [ ] Data terverifikasi di Neon (80 + 30 + 136 baris)
- [ ] Memahami perbedaan Full Refresh vs Incremental
- [ ] Scheduled sync sudah dikonfigurasi
