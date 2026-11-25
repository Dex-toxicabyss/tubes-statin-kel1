# Analisis Faktor Penentu Angka Harapan Hidup Global (WHO) Tahun 2015

Repositori resmi **Tugas Besar Statistika Industri (BBK2GAB3)** — Kelas 25-02, Program Studi S1 Sistem Informasi, Telkom University (Kelompok 1). Proyek ini menyajikan analisis statistik *end-to-end* terhadap faktor sosial-ekonomi dan kesehatan yang memengaruhi angka harapan hidup global (*Life Expectancy*) menggunakan data *cross-sectional* **World Health Organization (WHO) Tahun 2015**.

## Daftar Isi

* [Latar Belakang & Tujuan Penelitian](#latar-belakang--tujuan-penelitian)

* [Spesifikasi Dataset & Kamus Variabel](#spesifikasi-dataset--kamus-variabel)

* [Metodologi & Alur Pipeline](#metodologi--alur-pipeline)

* [Ringkasan Hasil Kuantitatif](#ringkasan-hasil-kuantitatif)

* [Temuan Utama & Implikasi](#temuan-utama--implikasi)

* [Cara Menjalankan (Getting Started)](#cara-menjalankan-getting-started)

* [Struktur Repositori](#struktur-repositori)

* [Tim Peneliti (Kelompok 1)](#tim-peneliti-kelompok-1)

* [Referensi](#referensi)

## Latar Belakang & Tujuan Penelitian

Angka harapan hidup (*Life Expectancy*) merupakan indikator makro utama dalam mengukur kualitas kesehatan publik dan pembangunan manusia suatu negara. Dengan menggunakan irisan data *cross-sectional* **Tahun 2015** ($N = 183$ negara), studi ini bertujuan untuk:

1. **Meringkas profil global & kesenjangan struktural** antara negara maju (*Developed*) dan berkembang (*Developing*) melalui statistika deskriptif serta deteksi pencilan berbasis *Interquartile Range* (IQR).

2. **Mengevaluasi asumsi kenormalan distribusi** keempat variabel numerik utama menggunakan uji Shapiro–Wilk dan Q–Q Plot, serta menerapkan transformasi data yang tepat.

3. **Menguji signifikansi perbedaan dan asosiasi** secara inferensial menggunakan *1-Sample t-test*, *2-Sample Welch's t-test*, dan uji independensi *Chi-Square* pada sampel representatif (rumus Slovin, $n = 126$).

4. **Memodelkan hubungan prediktif** antara lama pendidikan (`Schooling`) dan angka harapan hidup (`Life_expectancy`) melalui analisis korelasi Pearson dan regresi linear sederhana.

## Spesifikasi Dataset & Kamus Variabel

* **Sumber Primer**: [Life Expectancy (WHO) — Kaggle](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who)

* **Mirror Dataset (Raw CSV)**: [Life Expectancy Data.csv](https://www.google.com/search?q=https://raw.githubusercontent.com/Dex-toxicabyss/tubes-statin-kel1/main/Life%2520Expectancy%2520Data.csv)

* **Cakupan Data**: Filter *cross-sectional* `Year == 2015` menghasilkan $N = 183$ **observasi negara** (0 duplikat).

| Variabel | Peran | Skala Data | Deskripsi Operasional & Satuan | Missing Value (2015) | Penanganan | 
| ----- | ----- | ----- | ----- | ----- | ----- | 
| `Country` | Identitas | Nominal | Nama negara berdaulat | 0 | Dipertahankan | 
| `Year` | Filter Waktu | Diskrit | Tahun observasi (difilter khusus `2015`) | 0 | Dipertahankan | 
| `Status` | Independen (Kategorik) | Nominal | Klasifikasi status negara (`Developed` / `Developing`) | 0 | Dipertahankan | 
| `Life_expectancy` | **Dependen (**$Y$**)** | Rasio | Rata-rata angka harapan hidup saat lahir (tahun) | 0 | Dipertahankan | 
| `Adult_Mortality` | Independen ($X_1$) | Rasio | Probabilitas kematian usia 15–60 tahun per 1.000 penduduk | 0 | Dipertahankan | 
| `GDP` | Independen ($X_2$) | Rasio | Produk Domestik Bruto per kapita (USD) | 29 (15,85%) | Imputasi *Mean* | 
| `Schooling` | **Independen (**$X_3$**)** | Rasio | Rata-rata lama menempuh pendidikan formal (tahun) | 10 (5,46%) | Imputasi *Mean* | 

## Metodologi & Alur Pipeline

Seluruh analisis dijalankan secara berurutan di Google Colaboratory dengan pendekatan *reproducible*:

```
[1. Data Collecting & Cleansing] ──▶ [2. EDA, Stat Deskriptif & IQR] ──▶ [3. Uji Normalitas & Transformasi]
                                                                                     │
[5. Korelasi Pearson & Regresi OLS] ◀── [4. Uji Hipotesis & Chi-Square] ◀── [Sampling Slovin (n=126)]



```

* **Deteksi Pencilan (IQR)**: $\text{IQR} = Q_3 - Q_1$, dengan batas normal $[Q_1 - 1.5 \times \text{IQR},\; Q_3 + 1.5 \times \text{IQR}]$.

* **Uji Normalitas (**$\alpha = 0,05$**)**: Shapiro–Wilk ($W$) dan Q–Q Plot; variabel yang menolak $H_0$ diuji dengan kandidat transformasi ($\log_{10}$, $\ln$, $\sqrt{x}$, $x^2$, $x^3$).

* **Teknik Sampling**: *Stratified Random Sampling* Proporsional (`Status`: 22 *Developed*, 104 *Developing*; `random_state=42`) dengan ukuran sampel Slovin ($e = 5\%$):

  $$
  n = \left\lceil \frac{N}{1 + N \cdot e^2} \right\rceil = \left\lceil \frac{183}{1 + 183(0.05)^2} \right\rceil = 126 \text{ negara}
  $$

* **Statistika Inferensial**: *1-Sample t-test* (uji representativitas terhadap $\mu_{\text{pop}} = 71,62$), *Welch's 2-Sample t-test* (`Developed` vs `Developing`), *Chi-Square Test of Independence* (batas median populasi $73,90$ tahun), Korelasi Pearson ($r$), dan Regresi Linear Sederhana ($\hat{Y} = aX + b$).

## Ringkasan Hasil Kuantitatif

### 1. Statistika Deskriptif & Deteksi Outlier (Populasi $N = 183$)

| Variabel | Mean | Std. Dev | Median ($Q_2$) | Min – Max | Status Distribusi | 
| ----- | ----- | ----- | ----- | ----- | ----- | 
| `Life_expectancy` | **71,62** | 8,12 | 73,90 | 51,00 – 88,00 | *Left-skewed* (0 *outlier*) | 
| `Adult_Mortality` | **152,86** | 97,56 | 138,00 | 1,00 – 484,00 | *Right-skewed* (1 *outlier* atas) | 
| `GDP` | **7.185,33** | 10.725,15 | 4.132,76 | 33,68 – 66.346,52 | Sangat *right-skewed* ($s > \bar{x}$) | 
| `Schooling` | **12,93** | 2,83 | 12,93 | 4,90 – 20,40 | Simetris / Normal | 

### 2. Uji Normalitas Shapiro–Wilk & Transformasi Data ($\alpha = 0,05$)

| Variabel | $W$ Sebelum | $p$-value Sebelum | Metode Terpilih | $W$ Sesudah | $p$-value Sesudah | Keputusan Akhir | 
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | 
| `Life_expectancy` | 0,9644 | `1,33e-04` | $x^3$ (Kubik) | **0,9804** | `1,12e-02` | Membaik, masih tolak $H_0$ | 
| `Adult_Mortality` | 0,9508 | `5,70e-06` | $\sqrt{x}$ (Akar Kuadrat) | **0,9920** | `4,13e-01` | **Normal (Gagal Tolak** $H_0$**)** | 
| `GDP` | 0,6060 | `2,74e-20` | $\log_{10}(x)$ | **0,9629** | `9,05e-05` | Membaik drastis, masih tolak $H_0$ | 
| `Schooling` | 0,9915 | `3,61e-01` | *Tidak perlu* | 0,9915 | `3,61e-01` | **Normal (Gagal Tolak** $H_0$**)** | 

### 3. Uji Hipotesis, Chi-Square, Korelasi, & Regresi Linear (Sampel $n = 126$)

| Pengujian Statistik | Hipotesis / Model | Statistik Uji | $p$-value | Kesimpulan ($\alpha = 0,05$) | 
| ----- | ----- | ----- | ----- | ----- | 
| **1-Sample *t-test*** | $H_0: \mu = 71,62$ vs $H_1: \mu \ne 71,62$ | $t = -0,0960$ | `0,9237` | **Gagal Tolak $H_0$** — Rata-rata sampel (**71,55 th**) sangat representatif |
| **Welch's 2-Sample *t-test*** | *Developed* (80,57) vs *Developing* (69,64) | $t = 10,1303$ | `7,52e-15` | **Tolak $H_0$** — Selisih 10,93 tahun sangat signifikan |
| **Chi-Square Independensi** | `Status` vs `Kategori_LE` (Median $73,90$) | $\chi^2 = 20,6229$ ($df=1$) | `5,59e-06` | **Tolak $H_0$** — Terdapat hubungan signifikan (min *expected* = 10,83) |
| **Korelasi Pearson** | `Schooling` vs `Life_expectancy` | $r = +0,7703$ | — | Korelasi positif kuat (`Adult_Mortality`: $r = -0,7748$; `GDP`: $r = +0,4634$) |
| **Regresi Linear (OLS)** | $\hat{Y} = 2,2143X + 42,9519$ | $R^2 = 0,5934$ | `5,41e-26` | **Signifikan** — 59,34% variasi `Life_expectancy` dijelaskan `Schooling` |

## Temuan Utama & Implikasi

* **Kesenjangan Struktural Global**: Negara maju (*Developed*) memiliki rata-rata angka harapan hidup sekitar **11 tahun lebih tinggi** dibandingkan negara berkembang (*Developing*), serta 95,45% negara maju pada sampel (21 dari 22) berada di atas median global 73,90 tahun.

* **Peran Pendidikan terhadap Kesehatan**: Setiap penambahan **1 tahun rata-rata lama sekolah (`Schooling`)** berasosiasi dengan peningkatan angka harapan hidup sebesar **\~2,13 tahun** ($\beta_1 = 2,1257$), menjadikan investasi pendidikan sebagai prediktor sosio-ekonomi terkuat dalam model linear sederhana ini.

* **Tekanan Mortalitas Dewasa**: `Adult_Mortality` memiliki korelasi negatif terkuat terhadap harapan hidup ($r = -0,7748$), menegaskan pentingnya intervensi kesehatan usia produktif.

## Cara Menjalankan (*Getting Started*)

### Opsi 1: Google Colaboratory (Direkomendasikan)

1. Buka notebook melalui badge **Open In Colab** di bagian atas halaman ini.

2. Klik **Runtime → Run all** untuk menjalankan seluruh analisis secara otomatis (dataset akan ditarik langsung dari *raw URL* GitHub).

### Opsi 2: Lingkungan Lokal (Jupyter Notebook)

```
# 1. Clone repositori
git clone https://github.com/Dex-toxicabyss/tubes-statin-kel1.git
cd tubes-statin-kel1

# 2. Instal dependensi yang diperlukan
pip install pandas numpy scipy statsmodels matplotlib seaborn jupyter

# 3. Jalankan Jupyter Notebook
jupyter notebook



```

## Struktur Repositori

```
tubes-statin-kel1/
├── Life Expectancy Data.csv                        # Dataset mentah WHO (Kaggle)
├── Tubes_Statistika_Industri_Kelompok_1_Full.ipynb # Notebook utama (terstruktur + komentar per step)
├── Life_Expectancy_2015_Transformed.csv            # Output dataset bersih + kolom transformasi (generated)
└── README.md                                       # Dokumentasi proyek



```

## Tim Peneliti (Kelompok 1)

| No | Nama Lengkap | NIM | Peran & Bagian Pengerjaan | 
| ----- | ----- | ----- | ----- | 
| 1 | **Raffata Izacky Yuarga Aletama** | `102042500123` | Uji Ulang Normalitas, Visualisasi Boxplot/Histogram, & Integrasi Notebook | 
| 2 | **Muhammad Dyandra Putra** | `102042500152` | *Data Collecting* & *Data Cleansing* (`df_clean`) | 
| 3 | **Muhammad Abyan Setiabudi** | — | Statistika Deskriptif Numerik & Kategorik | 
| 4 | **Dustin Alea Maisan** | — | Perancangan & Eksekusi *Stratified Random Sampling* Slovin ($n = 126$) | 
| 5 | **Elang Reyfan Bagaskara** | — | Visualisasi Komposisi, *Scatter Plot*, & *Heatmap* Korelasi | 
| 6 | **Nailah Farah Ramadhani** | — | Deteksi Pencilan (*Outlier*) Menggunakan Metode IQR | 

## Referensi

1. Rajarshi, K., Russell, D., & Wang, D. (2018). [*Life Expectancy (WHO): Statistical Analysis on Factors Influencing Life Expectancy*](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who). Kaggle / World Health Organization (WHO).

2. Aji, C. (2022). [*Exploratory Data Analysis (EDA) – Life Expectancy WHO Dataset*](https://rpubs.com/chaidaraji/eda-life-expectancy). RPubs.

3. Kar, N. G. (2022). [*Life Expectancy Statistical Analysis (WHO): Data Cleaning, EDA, MFA, and Regression*](https://rstudio-pubs-static.s3.amazonaws.com/920150_6696a808906844cb98fc53a6c31d6999.html). RStudio Pubs.

*Terakhir diperbarui: November 2026 — Program Studi S1 Sistem Informasi, Telkom University*