
# Analisis dan Prediksi Produksi Sumur Gas Lepas Pantai (Project TYBAS LIDIA)

![Python](https://img.shields.io/badge/Python-3.10-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data_Analysis-150458?style=for-the-badge&logo=pandas)
![XGBoost](https://img.shields.io/badge/XGBoost-Model-red?style=for-the-badge)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-Machine_Learning-orange?style=for-the-badge&logo=scikit-learn)

Proyek ini adalah *notebook* Jupyter (`.ipynb`) yang mendokumentasikan proses *end-to-end* pengolahan data produksi migas dari 5 sumur lepas pantai (*offshore*). Fokus utama proyek ini adalah pembersihan data (*data cleaning*) yang ketat, analisis eksploratif, dan pembangunan model *Machine Learning* untuk memprediksi laju produksi gas.

## 🛢️ Latar Belakang Proyek

Data berasal dari lapangan lepas pantai dengan 5 sumur aktif (AA1 - AA5) yang memproduksi *dry gas* dan air, dimulai sekitar tahun 2021. Setiap sumur dilengkapi dengan berbagai sensor tekanan dan temperatur pada *choke*, *casing-annulus*, *wellhead*, dan *bottomhole*.

Tujuan utama adalah membersihkan data dari *noise* dan membangun model prediksi yang akurat untuk parameter produksi.

## 📂 Kamus Data (Data Dictionary)

Dataset asli memiliki format lebar (*wide format*) dengan kode sensor tertentu. Berikut adalah pemetaan variabel yang digunakan dalam analisis:

| Kode Variabel | Deskripsi Fisik | Satuan (Asumsi) |
| :--- | :--- | :--- |
| `91_EP_NATURAL_GAS` | **Gas Production Rate** (Target Variable) | MMSCFD |
| `91_9500062414` | **Water Production Rate** | BWPD |
| `ANPR` | Casing-Annulus Pressure | psig |
| `ANTP` | Casing-Annulus Temperature | degC |
| `C_OP` | Choke Opening | % |
| `C_PR` | Choke Pressure | psig |
| `C_TE` | Choke Temperature | degC |
| `FBHP` | Flowing Bottomhole Pressure | psig |
| `FBHT` | Flowing Bottomhole Temperature | degC |
| `FTHP` | Flowing Tubing Head Pressure | psig |
| `FTHT` | Flowing Tubing Head Temperature | degC |

## 🛠️ Alur Kerja (Pipeline)

Proyek ini terdiri dari beberapa tahapan utama yang didokumentasikan dalam notebook:

### 1. Data Cleaning & Preprocessing
Pembersihan data dilakukan dengan aturan logika domain *engineering*:
* **Koreksi Unit:** Kolom `Water Production Rate` dibagi 1000 jika nilainya > 10, untuk mengoreksi kesalahan satuan pencatatan.
* **Penanganan Outlier Bawah:** Nilai di bawah ambang batas `0.5` dihapus dan digantikan menggunakan interpolasi linear untuk menjaga kontinuitas *time-series*.
* **Restrukturisasi Data:** Mengubah format data dari *Wide* ke *Long format* untuk memudahkan pemrosesan berbasis sumur (*Well ID*).

### 2. Exploratory Data Analysis (EDA)
* **Visualisasi Time Series:** Plotting parameter produksi dan sensor untuk setiap sumur guna melihat tren historis.
* **Analisis Korelasi:** Menggunakan *Heatmap* untuk melihat hubungan antara tekanan, temperatur, dan laju produksi.
* **Distribusi Data:** Menggunakan Histogram dan Boxplot untuk melihat sebaran data sensor.

### 3. Feature Engineering
Untuk meningkatkan kinerja model *machine learning*, fitur-fitur baru dibuat:
* **Smoothing:** Penerapan *Rolling Mean* dengan jendela 7 hari untuk mengurangi *noise* harian.
* **Lag Features:** Membuat fitur *lag* (t-1, t-3, t-7) untuk menangkap ketergantungan waktu.
* **Rolling Statistics:** Menghitung rata-rata dan standar deviasi berjalan.

### 4. Modelling & Forecasting
* **Model Utama:** Menggunakan **XGBoost Regressor** untuk memprediksi `Gas_Production_Rate`.
* **Hyperparameter Tuning:** Menggunakan `GridSearchCV` untuk mencari parameter optimal (`learning_rate`, `max_depth`, `n_estimators`, dll).
* **Proyeksi Eksogen:** Menggunakan **Linear Regression** untuk memproyeksikan nilai masa depan parameter tekanan (`FTHP`, `FBHP`) yang digunakan sebagai *input* model XGBoost.
* **Evaluasi:** Model dievaluasi menggunakan metrik MAE, MSE, RMSE, dan R-Squared.

## 📊 Hasil Evaluasi Model

Contoh hasil evaluasi pada Sumur AA1 (Tuned XGBoost pada data yang di-*smooth*):

| Metrik | Nilai (Test Set) |
| :--- | :--- |
| **R-Squared (R²)** | ~0.94 |
| **RMSE** | ~0.018 |
| **MAE** | ~0.011 |

*(Nilai metrik diambil dari output log pelatihan pada notebook)*

## 🚀 Cara Menjalankan

1.  Pastikan Anda memiliki Python 3.10+ dan pustaka yang diperlukan:
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn xgboost openpyxl
    ```
2.  Letakkan file dataset `pivot_55_description_640rows(2).xlsx` di direktori root.
3.  Jalankan notebook `TYBAS LIDIA.ipynb` secara berurutan.

---
*Proyek ini disusun sebagai bagian dari Tugas Besar (TYBAS) untuk analisis data produksi migas.*
