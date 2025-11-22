# Data Cleaning dan Penskalaan Kondisional (TYBAS_LIDIA)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Transformation-informational?style=for-the-badge&logo=pandas)
![Jupyter](https://img.shields.io/badge/Jupyter-Pipeline-red?style=for-the-badge&logo=jupyter)

Ini adalah proyek inti yang berfokus pada pembangunan *pipeline* pembersihan dan transformasi data untuk dataset terstruktur. Tujuannya adalah untuk mengoreksi ketidaksesuaian skala dalam kolom fitur utama secara kondisional, memastikan data siap untuk analisis atau pemodelan Machine Learning.

## Fitur Utama

- **Pemuatan Data Excel:** Mengimpor data mentah langsung dari file `.xlsx` menggunakan Pandas.
- **Pemeriksaan Kualitas Data:** Melakukan pemeriksaan awal pada tipe data dan statistik deskriptif.
- **Penskalaan Kondisional (Core Logic):** Menerapkan fungsi kustom untuk menyesuaikan nilai dalam kolom `AA1` hingga `AA5` berdasarkan ambang batas tertentu.
- **Debugging & Verifikasi:** Memastikan bahwa nilai yang salah skala telah berhasil diperbaiki dan data kini konsisten.

## Arsitektur Pipeline

Proyek ini menggunakan alur kerja linier untuk transformasi data. Logika pembersihan utama berpusat pada penyesuaian unit/skala yang diyakini salah dalam kolom fitur.

| Tahap | Proses | Output | Keterangan |
| :--- | :--- | :--- | :--- |
| **1. Inisialisasi** | `Data Loading` | Raw DataFrame | Memuat data dari file `pivot_55_description_640rows(2).xlsx`. |
| **2. Penskalaan** | `Conditional Scaling` | Transformed DataFrame | Menerapkan fungsi kustom untuk mengoreksi nilai yang terlalu besar (misalnya, kesalahan unit/desimal). |
| **3. Finalisasi** | `Verification` | Cleaned DataFrame | Mencetak data untuk mengonfirmasi bahwa penskalaan telah diterapkan dengan benar. |

## Logika Pembersihan Inti

Logika penskalaan kondisional adalah aspek paling krusial dari *pipeline* ini, dirancang untuk memperbaiki kesalahan skala yang umum (misalnya, data dalam meter tercatat sebagai milimeter).

### Fungsi Penskalaan

Logika ini diterapkan pada kolom **AA1, AA2, AA3, AA4, dan AA5**.

```python
# Fungsi kustom untuk membagi nilai dengan 1000 jika nilai tersebut lebih besar dari 10.
def divide_by_1000(value):
    if value > 10:
        return value / 1000
    return value
````

### Penerapan Logika

```python
# Menerapkan fungsi ke kolom yang ditargetkan menggunakan Pandas .apply()
columns_to_scale = ['AA1', 'AA2', 'AA3', 'AA4', 'AA5']
for col in columns_to_scale:
    df[col] = df[col].apply(divide_by_1000)
```

## Struktur Proyek

  - `data_cleaning_pipeline/`
      - **`TYBAS_LIDIA.ipynb`**: Notebook utama yang berisi semua langkah, kode, dan hasil eksekusi.
      - `pivot_55_description_640rows(2).xlsx`: File data input.
      - `README.md`

## Penyiapan dan Instalasi

### Prasyarat

  - Lingkungan Python 3.x
  - Perpustakaan: `pandas`, `numpy`, `openpyxl`

### Langkah-langkah

1.  **Clone Repositori**

    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```

2.  **Instal Dependensi**

    ```bash
    pip install pandas numpy openpyxl
    ```

## Cara Menjalankan

1.  Pastikan file data Excel (`pivot_55_description_640rows(2).xlsx`) berada di direktori yang sama dengan notebook.
2.  Buka notebook di JupyterLab atau VS Code:
    ```bash
    jupyter notebook TYBAS_LIDIA.ipynb
    ```
3.  Jalankan semua sel dari awal hingga akhir untuk mengeksekusi *pipeline* pembersihan dan melihat hasilnya.

<!-- end list -->

