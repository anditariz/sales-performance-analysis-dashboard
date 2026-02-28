# 📊 Sales Revenue Analytics  
**Virtual Internship – Bank Muamalat × Rakamin Academy**  
**Business Intelligence Project**  

![SQL](https://img.shields.io/badge/SQL-BigQuery-blue)
![Dashboard](https://img.shields.io/badge/Tool-LookerStudio-orange)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

---

## 👋 Tentang Proyek Ini

Proyek ini merupakan Final Task dari program **Virtual Internship Experience – Business Intelligence Analyst**.

Saya menganalisis data penjualan dari PT Sejahtera Bersama, sebuah perusahaan yang bergerak di bidang produk teknologi edukasi seperti Robots, Drones, Robot Kits, eBooks, Training Videos, dan Blueprints yang tersebar di 340 kota di Amerika Serikat.

Tujuan utama proyek ini adalah mengubah data mentah menjadi insight bisnis yang dapat membantu perusahaan mengambil keputusan strategis berbasis data.

Proyek ini dikerjakan secara end-to-end:
- Import data CSV ke BigQuery
- Data cleaning dan validasi
- Pembuatan master table menggunakan SQL
- Visualisasi dashboard di Looker Studio
- Penyusunan rekomendasi bisnis

---

## 🎯 Tujuan Analisis

- Mengidentifikasi produk yang paling berkontribusi terhadap revenue
- Menentukan kota dengan potensi penjualan tertinggi
- Menganalisis tren penjualan bulanan
- Memberikan rekomendasi berbasis data untuk meningkatkan performa bisnis

---

## 🛠 Tools yang Digunakan

- Google BigQuery (SQL)
- Looker Studio
- Google Sheets / CSV
- dbdiagram.io (Perancangan ERD)

---

## 🗂 Struktur Dataset

Dataset terdiri dari 4 tabel utama:

1. `customers_data`
2. `orders_data`
3. `product_data`
4. `productCategory_data`

Relasi utama:
- 1 Customer → banyak Orders
- 1 Order → 1 Product
- 1 Product → 1 Category

Primary key ditentukan di setiap tabel untuk menjaga integritas data sebelum proses join.

---

## 🔎 Data Cleaning

Beberapa proses validasi yang dilakukan:

- Cek duplikasi data
- Cek nilai NULL
- Validasi format email (ditemukan karakter tidak valid seperti '#')
- Koreksi harga produk (dibagi 100 karena tersimpan dalam format cent)

Proses ini penting untuk memastikan analisis dilakukan pada data yang bersih dan akurat.

---

## 🧩 Technical SQL Query

Berikut adalah query utama untuk membuat master table dengan menggabungkan 4 tabel:

```sql
CREATE OR REPLACE TABLE `da-2025-481513.pbi.master_table` AS
WITH ranked AS (
  SELECT
    o.Date AS order_date,
    pc.CategoryName AS category_name,
    p.ProdName AS product_name,
    p.Price / 100 AS product_price,
    o.Quantity AS order_qty,
    (o.Quantity * p.Price / 100) AS total_sales,
    c.CustomerEmail AS cust_email,
    c.CustomerCity AS cust_city,
    ROW_NUMBER() OVER (
      PARTITION BY o.Date, pc.CategoryID
      ORDER BY o.OrderID ASC
    ) AS rn
  FROM orders_data o
  JOIN customers_data c ON o.CustomerID = c.CustomerID
  JOIN product_data p ON o.ProdNumber = p.ProdNumber
  JOIN productCategory_data pc ON p.Category = pc.CategoryID
)

SELECT * EXCEPT(rn)
FROM ranked
WHERE rn = 1
ORDER BY order_date ASC;
```

### Teknik yang Digunakan:

- **JOIN 4 Tabel** untuk menggabungkan data transaksi lengkap
- **CTE (WITH)** untuk membuat query lebih terstruktur dan mudah dibaca
- **ROW_NUMBER()** untuk mencegah duplikasi per kategori per hari
- Perhitungan kolom baru: `total_sales`

Output akhir berupa master table dengan 8 kolom utama yang siap digunakan untuk visualisasi dashboard.

---

## 📊 Dashboard

Dashboard terdiri dari 3 halaman utama:

### 1️⃣ Overview
- Total Revenue
- Total Quantity Sold
- Average Order Value (AOV)
- Monthly Sales Trend
- Distribusi Penjualan per Kategori

### 2️⃣ Analisis Kategori
- Perbandingan Total Qty vs Total Sales
- AOV per kategori
- Insight performa kategori

### 3️⃣ Analisis Kota
- Top 10 Kota berdasarkan Revenue
- Peta sebaran pelanggan
- Distribusi 340 kota aktif

---

## 📈 Insight Utama

### 🤖 Robots = Revenue Driver
- 854 unit terjual
- 44,1% kontribusi total revenue
- AOV tertinggi (~$2.617)

### 📚 eBooks = Volume Tinggi, Nilai Rendah
- Unit terjual paling banyak
- Potensi strategi bundling untuk menaikkan AOV

### 🌍 Pasar Sangat Tersebar
- 340 kota aktif
- Top 10 kota hanya menyumbang ~20% revenue

### 📉 Penurunan di Juli–Agustus
Terdapat pola seasonal dip yang dapat diantisipasi dengan strategi promosi.

---

## 🚀 Rekomendasi Bisnis

1. Fokus upselling pada pembeli Robots & Drones
2. Strategi bundling eBooks dengan produk premium
3. Prioritaskan marketing di Washington & Houston
4. Siapkan kampanye khusus periode low season

---

## 📁 Struktur Repository

```
├── ERD.pdf
├── Final Task.sql
├── data_analysis_pbi.ipynb
├── Final Task Report.pbix
├── Portfolio.pdf
├── assets/
│   ├── dashboard_overview.png
│   ├── dashboard_kategori.png
│   └── dashboard_kota.png
└── README.md
```

---

## ✨ Refleksi

Proyek ini merupakan langkah awal saya sebagai Data Analyst.  
Dari proses data cleaning hingga penyusunan rekomendasi bisnis, saya belajar bahwa analisis bukan hanya tentang angka, tetapi tentang bagaimana menerjemahkan data menjadi keputusan yang berdampak.

---

## 📬 Kontak

📧 anditarizkar@gmail.com  
🔗 linkedin.com/in/andita-rizka-ramadhanti  
💻 github.com/anditariz  

Terima kasih telah mengunjungi repository ini.
