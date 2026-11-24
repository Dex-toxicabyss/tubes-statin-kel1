# Analisis Faktor Penentu Angka Harapan Hidup Global (WHO) Tahun 2015

[](https://colab.research.google.com/drive/10qO0BUJaXCACJvP14Z2zuUveSthyEhW0?ouid=104796351325196284098)

Repositori **Tugas Besar Statistika Industri (BBK2GAB3)** — Kelas 25-02, Program Studi S1 Sistem Informasi, Telkom University (Kelompok 1). Proyek ini menganalisis pengaruh mortalitas dewasa, PDB per kapita, lama pendidikan, dan status negara terhadap angka harapan hidup global (*Life Expectancy*) menggunakan data *cross-sectional* WHO Tahun 2015.

## Ringkasan Proyek

Seluruh alur analisis dijalankan secara *end-to-end* di Google Colab tanpa laporan terpisah, mencakup pembersihan data (*cleansing*), statistika deskriptif, deteksi *outlier* (IQR), uji normalitas dan transformasi data, *simple random sampling* (Slovin), uji hipotesis & Chi-Square, hingga pemodelan korelasi dan regresi linear sederhana.

## Anggota Kelompok 1

| No | Nama Lengkap | NIM | Fokus Kontribusi |
| :--- | :--- | :--- | :--- |
| 1 | Raffata Izacky Yuarga Aletama | `102042500123` | Uji Ulang Normalitas, Visualisasi, & Integrasi |
| 2 | Muhammad Dyandra Putra | `102042500152` | *Data Collecting* & *Cleansing* |
| 3 | Muhammad Abyan Setiabudi | — | Statistika Deskriptif Numerik & Kategorik |
| 4 | Dustin Alea Maisan | — | *Sampling* Slovin ($n = 126$) |
| 5 | Elang Reyfan Bagaskara | — | Visualisasi & *Heatmap* Korelasi |
| 6 | Nailah Farah Ramadhani | — | Deteksi *Outlier* Metode IQR |

## Sumber Data & Teknologi

- **Dataset Utama**: [Life Expectancy (WHO) — Kaggle](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who) ([Mirror CSV di repositori ini](https://www.google.com/search?q=https://raw.githubusercontent.com/Dex-toxicabyss/tubes-statin-kel1/main/Life%2520Expectancy%2520Data.csv))
- **Bahasa & Lingkungan**: Python 3 di Google Colaboratory
- **Library**: `pandas`, `numpy`, `scipy` (`shapiro`, `ttest_1samp`, `ttest_ind`, `chi2_contingency`, `linregress`), `statsmodels` (`qqplot`), `matplotlib`, `seaborn`

## Cara Menjalankan (*Getting Started*)

### Opsi 1: Langsung di Google Colab (Direkomendasikan)
1. Buka notebook melalui badge **Open In Colab** di atas (atau melalui file `.ipynb` di repositori ini).
2. Klik **Runtime → Run all** untuk menjalankan seluruh blok dari tahap *cleansing* hingga regresi linear (dataset otomatis dimuat dari *raw URL* GitHub).

### Opsi 2: Jalankan secara Lokal
```bash
git clone https://github.com/Dex-toxicabyss/tubes-statin-kel1.git
cd tubes-statin-kel1
pip install pandas numpy scipy statsmodels matplotlib seaborn jupyter
jupyter notebook
```

## Alur Analisis & Temuan Utama

| Tahap | Metode yang Digunakan | Ringkasan Hasil (Tahun 2015) |
| :--- | :--- | :--- |
| **1. Cleansing & Deskriptif** | Filter `Year == 2015`, imputasi *mean*, IQR | $N = 183$ negara ($0$ *missing value*); rata-rata `Life_expectancy` **71,62 tahun** (*Developed* 80,71 vs *Developing* 69,69). |
| **2. Uji Normalitas** | Shapiro–Wilk ($\alpha = 0,05$), Q–Q Plot, Transformasi | Awalnya hanya `Schooling` yang normal ($p = 0,3611$). Transformasi $\sqrt{x}$ menormalkan `Adult_Mortality` ($p = 0,4135$); $x^3$ menaikkan $W$ `Life_expectancy` ke `0,9804` dan $\log_{10}$ menaikkan $W$ `GDP` ke `0,9629`. |
| **3. Sampling** | *Simple Random Sampling* (Slovin, $e = 5\%$, `random_state=42`) | Terpilih **$n = 126$** negara (68,85% dari populasi). |
| **4. Uji Hipotesis & Chi-Square** | 1-Sample *t-test* ($\mu > 70$), Welch *t-test*, Chi-Square | Rata-rata sampel **71,34 tahun** signifikan $> 70$ ($p = 0,0340$); perbedaan *Developed* vs *Developing* signifikan ($p = 3,76 \times 10^{-15}$); terdapat hubungan antara `Status` dan kategori harapan hidup ($\chi^2 = 15,9589$; $p = 6,47 \times 10^{-5}$). |
| **5. Korelasi & Regresi Linear** | Korelasi Pearson & OLS (`Schooling` $\rightarrow$ `Life_expectancy`) | Korelasi `Schooling` $r = 0,7831$; model $\hat{Y} = 2,1257X + 44,0909$ ($R^2 = 0,6133$; $p = 2,37 \times 10^{-27}$). Setiap +1 tahun lama sekolah berasosiasi dengan kenaikan harapan hidup **~2,13 tahun**. |

## Struktur Repositori

```text
tubes-statin-kel1/
├── Life Expectancy Data.csv        # Dataset mentah WHO (Kaggle)
├── Tubes_statin_kel_1_.ipynb       # Notebook utama analisis (Colab)
└── README.md                       # Dokumentasi proyek
```

## Referensi

- Rajarshi, K., Russell, D., & Wang, D. (2018). [*Life Expectancy (WHO): Statistical Analysis on Factors Influencing Life Expectancy*](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who). Kaggle.
- Aji, C. (2022). [*Exploratory Data Analysis (EDA) – Life Expectancy WHO Dataset*](https://rpubs.com/chaidaraji/eda-life-expectancy). RPubs.
- Kar, N. G. (2022). [*Life Expectancy Statistical Analysis (WHO)*](https://rstudio-pubs-static.s3.amazonaws.com/920150_6696a808906844cb98fc53a6c31d6999.html). RStudio Pubs.

---
*Terakhir diperbarui: November 2026*
