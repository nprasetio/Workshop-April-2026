# 📊 Metabase — SQL Queries: 4 Studi Kasus

Salin query-query ini ke **Metabase SQL Editor** (New → Question → Native query).

---

## 📈 Kasus 1: Sales Performance

### 1a. Ringkasan Bulanan

```sql
SELECT
  TO_CHAR(order_date, 'YYYY-MM')  AS bulan,
  COUNT(*)                         AS total_order,
  SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END) AS order_selesai,
  SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) AS order_batal,
  SUM(CASE WHEN status = 'refunded'  THEN 1 ELSE 0 END) AS order_refund,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS revenue,
  ROUND(
    SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1
  ) AS cancel_rate_pct
FROM orders
GROUP BY 1
ORDER BY 1;
```

**Simpan sebagai:** `Sales - Monthly Summary`
**Visualisasi:** Line chart (revenue), Bar chart (order count)

---

### 1b. KPI Total Revenue

```sql
SELECT
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS total_revenue,
  COUNT(*)                                                           AS total_order,
  SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END)            AS order_sukses,
  ROUND(
    SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1
  ) AS avg_cancel_rate
FROM orders;
```

**Simpan sebagai:** `Sales - KPI Summary`
**Visualisasi:** Scalar (single number) untuk masing-masing KPI

---

## 🏆 Kasus 2: Product Analysis

### 2a. TOP 10 Produk Terlaris

```sql
SELECT
  p.product_name,
  p.category,
  SUM(oi.quantity)               AS total_qty_terjual,
  SUM(oi.subtotal)               AS total_revenue,
  ROUND(AVG(oi.discount_pct), 1) AS avg_diskon_pct,
  COUNT(DISTINCT oi.order_id)    AS muncul_di_n_order
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders   o ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
GROUP BY p.product_name, p.category
ORDER BY total_revenue DESC
LIMIT 10;
```

**Simpan sebagai:** `Products - Top 10 by Revenue`
**Visualisasi:** Row chart (bar horizontal)

---

### 2b. Revenue per Kategori

```sql
SELECT
  p.category,
  COUNT(DISTINCT oi.order_id)    AS jumlah_order,
  SUM(oi.quantity)               AS total_unit_terjual,
  SUM(oi.subtotal)               AS total_revenue,
  ROUND(
    SUM(oi.subtotal) * 100.0 / SUM(SUM(oi.subtotal)) OVER (), 1
  ) AS pct_dari_total
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders   o ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
GROUP BY p.category
ORDER BY total_revenue DESC;
```

**Simpan sebagai:** `Products - Revenue by Category`
**Visualisasi:** Pie/Donut chart

---

### 2c. Rating vs Revenue (Korelasi)

```sql
SELECT
  p.product_name,
  p.category,
  p.rating,
  SUM(oi.subtotal) AS total_revenue,
  SUM(oi.quantity) AS total_terjual
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders   o ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
GROUP BY p.product_name, p.category, p.rating
ORDER BY p.rating DESC;
```

**Simpan sebagai:** `Products - Rating vs Revenue`
**Visualisasi:** Scatter plot (X=rating, Y=revenue)

---

## 🗺️ Kasus 3: Regional Sales

### 3a. Revenue per Wilayah

```sql
SELECT
  region,
  COUNT(*)                                                            AS total_order,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END)  AS revenue,
  ROUND(AVG(CASE WHEN status = 'completed' THEN total_amount END), 0) AS avg_order_value,
  SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END)             AS order_sukses,
  ROUND(
    SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1
  ) AS success_rate
FROM orders
GROUP BY region
ORDER BY revenue DESC;
```

**Simpan sebagai:** `Regional - Revenue by Region`
**Visualisasi:** Bar chart

---

### 3b. Payment Method per Region

```sql
SELECT
  region,
  payment_method,
  COUNT(*)                                                           AS jumlah_order,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS revenue,
  ROUND(
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (PARTITION BY region), 1
  ) AS pct_dalam_region
FROM orders
WHERE status = 'completed'
GROUP BY region, payment_method
ORDER BY region, jumlah_order DESC;
```

**Simpan sebagai:** `Regional - Payment Method by Region`
**Visualisasi:** Stacked bar chart

---

## 🛒 Kasus 4: Customer Behavior

### 4a. Top 10 Customer

```sql
SELECT
  customer_id,
  COUNT(*)                                                           AS total_order,
  SUM(CASE WHEN status = 'completed' THEN total_amount ELSE 0 END) AS total_belanja,
  MIN(order_date::DATE)                                             AS first_order,
  MAX(order_date::DATE)                                             AS last_order,
  COUNT(DISTINCT region)                                            AS jumlah_region
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 1
ORDER BY total_belanja DESC
LIMIT 10;
```

**Simpan sebagai:** `Customers - Top 10 Leaderboard`
**Visualisasi:** Table

---

### 4b. Basket Analysis (Items per Order)

```sql
SELECT
  o.order_id,
  o.order_date::DATE              AS tanggal,
  o.region,
  o.payment_method,
  COUNT(oi.item_id)               AS jumlah_item,
  SUM(oi.quantity)                AS total_unit,
  ROUND(AVG(oi.discount_pct), 1) AS avg_diskon,
  SUM(oi.subtotal)                AS total_transaksi
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.status = 'completed'
GROUP BY o.order_id, o.order_date, o.region, o.payment_method
ORDER BY jumlah_item DESC;
```

**Simpan sebagai:** `Customers - Basket Analysis`
**Visualisasi:** Histogram (distribusi jumlah_item)

---

### 4c. Rata-rata Diskon per Kategori

```sql
SELECT
  p.category,
  ROUND(AVG(oi.discount_pct), 1)  AS avg_diskon_pct,
  SUM(oi.quantity)                 AS total_unit,
  COUNT(DISTINCT oi.order_id)      AS jumlah_order
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders   o ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
  AND oi.discount_pct > 0
GROUP BY p.category
ORDER BY avg_diskon_pct DESC;
```

**Simpan sebagai:** `Customers - Discount by Category`
**Visualisasi:** Bar chart

---

## 💡 Tips Metabase

- Tekan **Ctrl+Enter** (Windows) atau **Cmd+Enter** (Mac) untuk run query
- Klik **Explore results** setelah run untuk eksplorasi lebih lanjut
- Klik **Visualization** untuk ganti tipe chart
- Klik **Save** dan pilih koleksi sebelum menutup tab
