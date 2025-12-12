<h1 align="center">📊 Data Mining – Kelompok 9</h1>

<p align="center">
  <strong>Analisis Diamond Crash Strategy pada Trading Spot DOGE<br/>
  Menggunakan Patterned Dataset Model & CRISP-DM</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Mata%20Kuliah-Data%20Mining-blue" />
  <img src="https://img.shields.io/badge/Kelas-A-success" />
  <img src="https://img.shields.io/badge/Kelompok-9-orange" />
</p>

---

## 🧾 Informasi Kelas

-   **Mata Kuliah** : Data Mining
-   **Kelas** : A
-   **Kelompok** : 9

---

## 👥 Anggota Kelompok

<table>
  <tr>
    <th>No</th>
    <th>NIM</th>
    <th>Nama</th>
  </tr>
  <tr>
    <td align="center">1</td>
    <td align="center"><strong>22082010131</strong></td>
    <td>M. Sa'aduddin Abdillah Yusuf</td>
  </tr>
  <tr>
    <td align="center">2</td>
    <td align="center"><strong>22082010134</strong></td>
    <td>Dias Norman</td>
  </tr>
  <tr>
    <td align="center">3</td>
    <td align="center"><strong>22082010149</strong></td>
    <td>Krisna Eko Prasetyo</td>
  </tr>
</table>

---

## 🧠 Deskripsi Singkat Proyek

Project ini berfokus pada:

-   Analisis pola pergerakan harga **DOGE/USD** menggunakan **Patterned Dataset Model**
-   Penerapan metode **CRISP-DM** untuk alur kerja data mining
-   Pemanfaatan **K-Means Clustering** untuk mengelompokkan kondisi pasar
-   Evaluasi performa strategi **Diamond Crash** dibandingkan strategi **Buy & Hold**

---

## 📂 Struktur Proyek (Sesuai Repo)

```bash
.
├── DOGE_Patterned_Dataset_CRISPDM.ipynb   # Notebook utama (CRISP-DM end-to-end)
├── README.md                              # Dokumentasi proyek
├── data/
│   ├── raw/
│   │   └── doge_ohlc_daily.csv            # Data OHLC harian (mentah)
│   └── processed/
│       ├── doge_patterned_complete.csv    # Dataset berpola (Complete)
│       ├── doge_patterned_crash.csv       # Dataset berpola (Crash)
│       ├── doge_patterned_moon.csv        # Dataset berpola (Moon)
│       ├── clustering_metrics.csv         # Rekap metrik evaluasi clustering
│       ├── doge_monthly_roi_summary.csv   # Ringkasan ROI bulanan
│       └── doge_diamond_crash_events.csv  # Event Diamond Crash terdeteksi
├── figures/                               # Output visualisasi
│   ├── clustering_metrics_heatmap.png
│   ├── kmeans_clustering_comparison.png
│   ├── doge_price_volume_timeseries.png
│   ├── roi_comparison.png
│   ├── strategy_comparison.png
│   └── ptr_plr_distribution.png
└── models/
  └── kmeans_complete.pkl                # Model K-Means tersimpan
```

---

## ▶️ Cara Menjalankan (Singkat)

1. Buka notebook utama:

-   `DOGE_Patterned_Dataset_CRISPDM.ipynb`

2. Pastikan data tersedia:

-   `data/raw/doge_ohlc_daily.csv`

3. Output yang dihasilkan notebook:

-   File CSV di `data/processed/`
-   Gambar visualisasi di `figures/`

---

## 🖼️ Visualisasi Utama

-   Heatmap evaluasi clustering: `figures/clustering_metrics_heatmap.png`
-   Perbandingan hasil K-Means: `figures/kmeans_clustering_comparison.png`
-   Perbandingan strategi: `figures/strategy_comparison.png`
