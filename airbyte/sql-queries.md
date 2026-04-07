# 🔍 Airbyte — SQL Query Verifikasi Data

Jalankan query-query ini di **Neon SQL Editor** setelah sync selesai untuk memastikan data masuk dengan benar.

---

## 1. Cek Semua Tabel yang Ada

```sql
SELECT table_name, table_schema
FROM information_schema.tables
WHERE table_schema NOT IN ('pg_catalog', 'information_schema')
ORDER BY table_schema, table_name;
```

---

## 2. Verifikasi Jumlah Baris

```sql
-- Harus: orders=80, products=30, order_items=136
SELECT 'orders'      AS tabel, COUNT(*) AS jumlah_baris FROM orders
UNION ALL
SELECT 'products'    AS tabel, COUNT(*) AS jumlah_baris FROM products
UNION ALL
SELECT 'order_items' AS tabel, COUNT(*) AS jumlah_baris FROM order_items;
```

---

## 3. Preview Data

```sql
-- 5 baris pertama orders
SELECT order_id, customer_id, order_date, status, total_amount, region
FROM orders
LIMIT 5;

-- 5 baris pertama products
SELECT product_id, product_name, category, price, stock
FROM products
LIMIT 5;

-- 5 baris pertama order_items
SELECT item_id, order_id, product_id, quantity, unit_price, subtotal
FROM order_items
LIMIT 5;
```

---

## 4. Cek Kualitas Data

```sql
-- Apakah ada nilai NULL di kolom kritis?
SELECT
  SUM(CASE WHEN order_id IS NULL THEN 1 ELSE 0 END)    AS null_order_id,
  SUM(CASE WHEN order_date IS NULL THEN 1 ELSE 0 END)   AS null_date,
  SUM(CASE WHEN total_amount IS NULL THEN 1 ELSE 0 END) AS null_amount,
  SUM(CASE WHEN status IS NULL THEN 1 ELSE 0 END)       AS null_status
FROM orders;

-- Cek status yang ada di orders
SELECT status, COUNT(*) AS jumlah
FROM orders
GROUP BY status
ORDER BY jumlah DESC;
```

---

## 5. Test JOIN Antar Tabel

```sql
-- Pastikan relasi berfungsi
SELECT
  o.order_id,
  o.order_date,
  o.status,
  COUNT(oi.item_id) AS jumlah_item,
  SUM(oi.subtotal)  AS total_subtotal
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id, o.order_date, o.status
ORDER BY o.order_date
LIMIT 10;
```

---

## 6. Ringkasan Data untuk Konfirmasi

```sql
-- Ringkasan yang akan kamu lihat di Metabase nanti
SELECT
  MIN(order_date)::DATE AS tanggal_pertama,
  MAX(order_date)::DATE AS tanggal_terakhir,
  COUNT(DISTINCT customer_id) AS jumlah_pelanggan_unik,
  COUNT(DISTINCT region) AS jumlah_region,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS total_revenue
FROM orders;
```
