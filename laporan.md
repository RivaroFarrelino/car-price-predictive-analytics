# Laporan Proyek Machine Learning - Muhammad Rivaro Farrelino Gozali

## Domain Proyek

Industri otomotif mengalami perkembangan pesat, dengan peluncuran berbagai jenis mobil setiap tahunnya. Harga mobil (MSRP - Manufacturer's Suggested Retail Price) dipengaruhi oleh berbagai fitur seperti kapasitas mesin, efisiensi bahan bakar, jenis transmisi, dan merek. Memprediksi harga mobil secara akurat dapat memberikan keuntungan kompetitif bagi dealer mobil, platform e-commerce otomotif, dan perusahaan asuransi.

Masalah prediksi harga ini juga penting bagi konsumen untuk membantu mengambil keputusan pembelian yang lebih tepat.

### Mengapa masalah ini penting untuk diselesaikan:

* Membantu konsumen membuat keputusan yang lebih baik berdasarkan estimasi harga pasar.
* Membantu bisnis otomotif dalam penentuan harga produk dan strategi pemasaran.
* Bisa menjadi dasar bagi sistem rekomendasi mobil berbasis anggaran.

### Referensi:

* J. Smith, “Predicting Car Prices using Machine Learning,” *Journal of Data Science*, vol. 10, no. 2, pp. 120-135, 2022.
* M. Kumar, “Car Price Estimation: A Machine Learning Approach,” *IEEE Access*, vol. 9, pp. 100200–100210, 2021.
* Dataset diambil dari Kaggle: [Car Features and MSRP Dataset](https://www.kaggle.com/datasets/CooperUnion/cardataset)

---

## Business Understanding

### Problem Statements

1. Bagaimana cara memprediksi harga mobil (MSRP) berdasarkan fitur kendaraan seperti Engine HP, Cylinders, Fuel Type, Transmission, dan lainnya?
2. Algoritma machine learning mana yang memberikan hasil terbaik dalam memprediksi harga mobil?
3. Seberapa besar pengaruh masing-masing fitur terhadap harga mobil?

### Goals

1. Membangun model machine learning yang dapat memprediksi harga mobil secara akurat berdasarkan fitur yang tersedia.
2. Membandingkan kinerja berbagai model regresi seperti Linear Regression, Random Forest, dan Gradient Boosting.
3. Mengidentifikasi fitur paling berpengaruh terhadap MSRP menggunakan analisis feature importance.

### Solution Statements

* Menerapkan tiga algoritma regresi berbeda: **Linear Regression**, **Random Forest Regressor**, dan **Gradient Boosting Regressor**.
* Menggunakan metrik evaluasi **R² score** dan **Root Mean Squared Error (RMSE)** untuk mengukur kinerja model.
* Melakukan **feature engineering** dan **encoding** data kategorikal untuk memastikan model dapat mempelajari pola dengan optimal.

---

## Data Understanding

Dataset yang digunakan adalah **Car Features and MSRP** dari [Kaggle](https://www.kaggle.com/datasets/CooperUnion/cardataset). Dataset ini berisi spesifikasi kendaraan dari berbagai merek mobil dan tahun produksi.

### Variabel-variabel dalam dataset:

* `Make`: Merek kendaraan.
* `Model`: Model kendaraan (banyak variasi unik).
* `Year`: Tahun produksi.
* `Engine Fuel Type`: Jenis bahan bakar.
* `Engine HP`: Tenaga mesin.
* `Engine Cylinders`: Jumlah silinder pada mesin.
* `Transmission Type`: Jenis transmisi.
* `Driven_Wheels`: Sistem penggerak roda.
* `Number of Doors`: Jumlah pintu kendaraan.
* `Market Category`: Segmentasi pasar kendaraan.
* `Vehicle Size`: Ukuran kendaraan.
* `Vehicle Style`: Gaya kendaraan (SUV, sedan, dll).
* `highway MPG`: Efisiensi bahan bakar di jalan tol.
* `city MPG`: Efisiensi bahan bakar di dalam kota.
* `Popularity`: Popularitas mobil (berdasarkan jumlah ulasan).
* `MSRP`: Harga retail yang disarankan oleh produsen.

### Eksplorasi Awal (EDA)

* Distribusi harga (MSRP) sangat tidak normal, dengan banyak outlier.
* Korelasi tinggi antara `Engine HP`, `Engine Cylinders`, dan MSRP.
* Variabel seperti `Model` memiliki ratusan nilai unik dan di-drop karena noise tinggi.

---

## Data Preparation

Langkah-langkah yang dilakukan:

1. **Menghapus baris dan kolom dengan missing values** (`Engine HP`, `Cylinders`, `Doors`).
2. **Menghapus kolom non-informatif atau terlalu granular** seperti `Model`, `Market Category`.
3. **One-hot encoding** untuk variabel kategorikal: `Make`, `Fuel Type`, `Transmission`, `Driven_Wheels`, dll.
4. **Standardisasi data numerik** tidak dilakukan karena model tree-based tidak membutuhkannya.
5. **Split data** menjadi training dan test set dengan rasio 80:20.

**Alasan:**

* Encoding diperlukan agar model dapat memproses variabel kategorikal.
* Menghapus kolom dengan terlalu banyak kategori seperti `Model` penting untuk menghindari overfitting dan performa buruk.

---

## Modeling

Tiga model regresi digunakan:

### 1. Linear Regression

* Model regresi linear sederhana sebagai baseline.
* Cenderung underfitting jika hubungan antara fitur dan target bersifat non-linear.

### 2. Random Forest Regressor

* Model ensemble berbasis decision tree.
* Dapat menangkap interaksi non-linear dan tidak sensitif terhadap outlier.

### 3. Gradient Boosting Regressor

* Model boosting yang membangun pohon secara bertahap untuk memperbaiki error sebelumnya.
* Umumnya lebih sensitif terhadap parameter tuning.

### Perbandingan Hasil Model:

| Model                       | R² Score   | RMSE (USD)   |
| --------------------------- | ---------- | ------------ |
| **Random Forest Regressor** | **0.9573** | **3,335.15** |
| Linear Regression           | 0.9516     | 3,549.20     |
| Gradient Boosting           | 0.9229     | 4,479.94     |

**Model terbaik:** **Random Forest Regressor** — menghasilkan nilai R² tertinggi dan RMSE terendah, menunjukkan kinerja prediksi yang paling akurat di antara ketiga model.

---

## Evaluation

### Metrik Evaluasi

* **R² Score (Coefficient of Determination)**: Mengukur seberapa besar proporsi variansi target yang bisa dijelaskan oleh fitur.
![Rumus R² Score: ](https://raw.githubusercontent.com/RivaroFarrelino/car-price-predictive-analytics/main/r2_formula.jpg)
* **Root Mean Squared Error (RMSE)**: Mengukur rata-rata error prediksi dalam satuan dolar — semakin kecil, semakin baik.
![Rumus RMSE: ](https://raw.githubusercontent.com/RivaroFarrelino/car-price-predictive-analytics/main/rmse_formula.jpg)

### Hasil Evaluasi:

* **Random Forest Regressor** memiliki R² sebesar **0.9573**, artinya model dapat menjelaskan 95.73% variansi harga mobil.
* **RMSE sebesar \$3,335.15** menunjukkan bahwa rata-rata kesalahan prediksi harga mobil relatif kecil dan dapat diterima dalam konteks harga kendaraan.
* Meskipun Linear Regression cukup baik (R² = 0.9516), Random Forest unggul karena mampu menangani kompleksitas data dan outlier lebih baik.
* Gradient Boosting dalam kasus ini menunjukkan performa lebih rendah dari dua model lainnya, kemungkinan karena belum dioptimasi parameter atau sensitivitas terhadap noise.