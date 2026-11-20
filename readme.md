# Analisis Deskriptif, Visualisasi, dan Uji Kenormalan Data Life Expectancy (WHO 2000–2015)

Repositori resmi **Kelompok 1** untuk Tugas Besar mata kuliah **BBK2GAB3 – Statistika Industri**. Proyek ini mengolah dataset [Life Expectancy (WHO) di Kaggle](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who) melalui tahapan *data collecting*, *data cleansing*, statistika deskriptif, deteksi *outlier* metode IQR, *sampling* visualisasi, hingga uji kenormalan (*Shapiro-Wilk Test*) dan transformasi data menggunakan Python di [Google Colab](https://colab.research.google.com/).

## Daftar Berkas Repositori

| Nama Berkas | Deskripsi | Keterangan |
| :--- | :--- | :--- |
| `Life Expectancy Data.csv` | Dataset mentah dari WHO & PBB (`2.938` baris × `22` kolom, `193` negara, `2000–2015`) | Sumber data utama |
| `Life_Expectancy_WHO_Cleaned.csv` | Dataset bersih hasil *drop* 15 kolom, hapus duplikat, dan *mean imputation* (`0` `NaN`) | Output Progress 1 (Task 2) |
| `Life_Expectancy_Transformed.csv` | Dataset hasil transformasi (`x³`, `x²`, `√x`, `log₁₀`) dari uji kenormalan | Output Progress 2 |
| `Tubes_statin_kel_1_Complete.ipynb` | Notebook Google Colab kelompok (Bagian 1–6) terhubung langsung ke GitHub Raw | Progress 1 (Task 1–4) |
| `Tugas_2_Normality_Test_Colab.ipynb` | Notebook Google Colab uji kenormalan *Shapiro-Wilk*, *Q-Q Plot*, dan transformasi | Progress 2 |
| `Analisis_Statistika_Life_Expectancy_WHO.xlsx` | Workbook Excel dengan rumus dinamis (`AVERAGE`, `STDEV`, `PERCENTILE`, `AVERAGEIF`) | Pendukung analisis |
| `Laporan_Tugas_Besar_Statistika_Industri_Life_Expectancy_WHO.pdf` | Laporan akhir lengkap Bab I–V (termasuk Sub-bab 4.1–4.2) siap kumpul | Dokumen laporan |

## Variabel Fokus Penelitian

Dari 22 atribut asli, penelitian ini menyaring **7 kolom fokus** agar pemodelan tetap ringkas dan bebas multikolinearitas:

| Kolom | Peran | Tipe Data | Keterangan |
| :--- | :--- | :--- | :--- |
| `Country` & `Year` | Identitas & Waktu | Nominal & Diskrit | Penanda `193` negara selama periode `2000–2015` |
| `Status` | Variabel Kategorik | Nominal (Biner) | `Developing` (`2.426` baris / `82,57%`) vs `Developed` (`512` baris / `17,43%`) |
| `Life_expectancy` | Variabel Dependen ($Y$) | Numerik Kontinu | Rata-rata angka harapan hidup sejak lahir (tahun) |
| `Schooling` | Variabel Independen ($X_1$) | Numerik Kontinu | Rata-rata lama pendidikan formal penduduk (tahun) |
| `Adult_Mortality` | Variabel Independen ($X_2$) | Numerik Kontinu | Probabilitas kematian usia 15–60 tahun per `1.000` populasi |
| `GDP` | Pembanding Ekonomi ($X_3$) | Numerik Kontinu | Produk Domestik Bruto per kapita (USD) |

## Pembagian Tugas & Alur Notebook

Notebook `Tubes_statin_kel_1_Complete.ipynb` dibagi menjadi 6 bagian kerja yang saling terhubung:

1. **Bagian 1 (`[Dyandra]`):** *Data Cleansing* — pembersihan spasi nama kolom, seleksi 7 kolom fokus, penghapusan duplikat (`0` baris), dan *mean imputation* pada sel `NaN`.
2. **Bagian 2 (`[Abyan]`):** Statistika Deskriptif — perhitungan *mean*, *median*, standar deviasi, kuartil variabel numerik, dan proporsi kategori `Status`.
3. **Bagian 3 (`[Nailah]`):** Deteksi Calon Pencilan (*Outlier*) — perhitungan $IQR = Q_3 - Q_1$ serta batas pagar bawah ($Q_1 - 1{,}5 \times IQR$) dan batas pagar atas ($Q_3 + 1{,}5 \times IQR$).
4. **Bagian 4 (`[Dustin]`):** *Sampling* — pengambilan `n = 100` sampel acak tanpa pengembalian (`random_state=42`) menjadi `df_sampel`.
5. **Bagian 5 (`[Reyfan]`):** Visualisasi Hubungan Variabel — *Scatter Plot* (`Schooling` vs `Life_expectancy` + garis regresi) dan *Bar Chart* rata-rata per `Status`.
6. **Bagian 6 (`[Raffata]`):** Visualisasi Statistik — *Histogram* (garis *Mean* & *Median*) serta *Boxplot* sebaran kuartil dan calon pencilan per `Status`.

## Cara Menjalankan di Google Colab

Tidak perlu mengunggah file CSV secara manual setiap kali *runtime* terputus karena notebook langsung membaca berkas dari **GitHub Raw**:

1. Buka [Google Colab](https://colab.research.google.com/), pilih **File → Open notebook → GitHub**, lalu masukkan URL repositori `Dex-toxicabyss/tubes-statin-kel1`.
2. Pilih `Tubes_statin_kel_1_Complete.ipynb` (untuk Progress 1) atau `Tugas_2_Normality_Test_Colab.ipynb` (untuk Progress 2).
3. Klik **Runtime → Run all** (`Ctrl + F9`).

Untuk mengakses dataset mentah secara langsung di skrip Python lain, gunakan tautan *Raw* berikut:

```python
import pandas as pd

url = "https://raw.githubusercontent.com/Dex-toxicabyss/tubes-statin-kel1/main/Life%20Expectancy%20Data.csv"
df = pd.read_csv(url)
```

## Ringkasan Temuan Utama

### Progress 1 — Statistika Deskriptif & Outlier (Data Awal)

| Variabel | Missing (`NaN`) | Mean ($\bar{x}$) | SD ($s$) | Median ($Q_2$) | IQR |
| :--- | :---: | :---: | :---: | :---: | :---: |
| `Life expectancy` (Tahun) | `10` | `69,22` | `9,52` | `72,10` | `12,60` |
| `Schooling` (Tahun) | `163` | `11,99` | `3,36` | `12,30` | `4,20` |
| `Adult Mortality` (per 1.000) | `10` | `164,80` | `124,29` | `144,00` | `154,00` |
| `GDP` (USD) | `448` | `7.483,16` | `14.270,17` | `1.766,95` | `5.446,87` |

* **Kesenjangan Status Negara:** Negara maju (`Developed`) mencatat rata-rata harapan hidup `79,20` tahun (lama sekolah `15,80` tahun), unggul `12,10` tahun dibanding negara berkembang (`Developing`) sebesar `67,10` tahun (lama sekolah `11,20` tahun).
* **Sebaran Homogen vs Ketimpangan Ekstrem:** `Life expectancy` dan `Schooling` memiliki $s < \bar{x}$ (sebaran homogen), sedangkan `GDP` memiliki $s > \bar{x}$ (`14.270,17 > 7.483,16`) yang menunjukkan ketimpangan ekonomi tajam antarnegara.

### Progress 2 — Uji Kenormalan Shapiro-Wilk & Transformasi Data

| Variabel | $W$ Awal | Transformasi Terpilih | $W$ Sesudah Transformasi |
| :--- | :---: | :--- | :---: |
| `Life expectancy` | `0,9563` | Pangkat Tiga ($x^3$) | `0,9833` |
| `Schooling` | `0,9751` | Kuadrat ($x^2$) | `0,9858` |
| `Adult Mortality` | `0,9168` | Akar Kuadrat ($\sqrt{x}$) | `0,9896` |
| `GDP` | `0,5629` | Logaritma Basis 10 ($\log_{10} x$) | `0,9757` |

Seluruh transformasi menaikkan statistik uji Shapiro-Wilk ($W$) mendekati `1,00` (`0,98–0,99`) dan meluruskan sebaran titik pada *Q-Q Plot*. Nilai *p-value* tetap di bawah `0,05` karena tingginya sensitivitas uji Shapiro-Wilk pada ukuran sampel besar ($N = 2.938$).

## Referensi

* Rajarshi, K., Russell, D., & Wang, D. (2018). [Life Expectancy (WHO) – Kaggle Dataset](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who).
* Kar, N. G. (2022). [Life Expectancy Statistical Analysis (WHO) – RStudio Pubs](https://rstudio-pubs-static.s3.amazonaws.com/920150_6696a808906844cb98fc53a6c31d6999.html).
* Aji, C. (2022). [Exploratory Data Analysis (EDA) – RPubs](https://rpubs.com/chaidaraji/eda-life-expectancy).
* Vasantha, K. (2023). [Prediction of WHO Life Expectancy Data Using Multiple Linear Regression](https://www.ijrar.org/papers/IJRAR23A3006.pdf). *IJRAR*, 10(1).
