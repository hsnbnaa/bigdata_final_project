# 📦 Pipeline ETL & Analisis Data E-Commerce Olist

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-green)
![SQLite](https://img.shields.io/badge/Database-SQLite-lightgrey)
![Status](https://img.shields.io/badge/Status-Completed-success)

Repository ini berisi implementasi **pipeline data end-to-end (ETL & ELT)** untuk memproses, membersihkan, dan menganalisis **dataset E-Commerce**.  
Proyek ini mengubah **data mentah transaksional** menjadi **Data Warehouse berbasis Star Schema** yang siap digunakan untuk analisis bisnis.

---

## 📌 Deskripsi Singkat Studi Kasus

Studi kasus menggunakan **Brazilian E-Commerce Public Dataset by Olist**.  
Dataset ini terdiri dari data yang terfragmentasi dalam beberapa tabel, seperti:

- customers  
- orders  
- order_items  
- payments  
- reviews  
- products  

Tujuan utama proyek ini adalah **menyatukan data tersebut ke dalam satu sumber kebenaran (single source of truth)** berupa **Fact Table**, sehingga dapat menjawab pertanyaan bisnis berikut:

- 📈 Tren penjualan bulanan  
- 🛍️ Kategori produk terlaris  
- ⏰ Waktu belanja tersibuk  
- 🚚 Efisiensi ongkos kirim per wilayah  
- 🚨 Deteksi anomali nilai transaksi  

---

## 🏗️ Arsitektur Sistem

Pipeline dirancang menggunakan **arsitektur ETL berbasis Python dan SQLite**.

### 1️⃣ Extract (Source)
- Data diekstrak dari file **CSV** yang di-hosting di **Google Drive**
- Dataset diunduh langsung melalui URL publik

### 2️⃣ Transform (Processing)
Transformasi dilakukan menggunakan **Pandas**, meliputi:

- **Data Cleaning**
  - Standardisasi nama kolom (snake_case)
  - Penghapusan duplikasi data
- **Imputation**
  - Pengisian missing value pada review dan kategori produk
- **Outlier Removal**
  - Menggunakan metode **Interquartile Range (IQR)** pada harga
- **Standardization**
  - MinMax Scaling pada harga dan ongkos kirim
- **Feature Engineering**
  - Ekstraksi fitur waktu (bulan, jam)
  - Label Encoding wilayah
- **Denormalization**
  - Penggabungan tabel menjadi `fact_orders`

### 3️⃣ Load (Destination)
- Data bersih dimuat ke **SQLite Database (`olist_dw.db`)**
- Skema yang digunakan: **Star Schema**
  - Fact Table
  - Dimension Tables

### 4️⃣ Analytics
- Query analitik dijalankan langsung menggunakan **SQL**
- Digunakan untuk eksplorasi dan pengambilan insight bisnis

---

## 🔄 Perbedaan ETL dan ELT yang Digunakan

Meskipun notebook mencakup keduanya, implementasinya dibedakan secara jelas:

### 🔹 ETL (Extract – Transform – Load)
**Digunakan sebagai pendekatan utama data engineering**

- Transformasi berat dilakukan **sebelum data masuk ke database**
- Meliputi:
  - Pembersihan outlier
  - Normalisasi (MinMaxScaler)
  - Penggabungan tabel
- Tool utama: **Python (Pandas)**

**Tujuan:**  
Memastikan data yang masuk ke Data Warehouse sudah **bersih, konsisten, dan terstandarisasi**

---

### 🔹 ELT (Extract – Load – Transform)
**Digunakan pada tahap analytics & reporting**

- Data bersih terlebih dahulu di-load ke SQLite
- Transformasi lanjutan dilakukan **menggunakan SQL**
  - Aggregation (SUM, AVG)
  - Grouping
- Digunakan pada bagian **“8 Query Analitik”**

**Tujuan:**  
Memanfaatkan efisiensi **query engine database** untuk analisis data terstruktur

---

## ▶️ Cara Menjalankan Pipeline (Step-by-Step)

### 1️⃣ Persiapan Lingkungan
Pastikan **Python 3.x** sudah terinstal, lalu jalankan:

```bash
pip install pandas numpy scikit-learn
