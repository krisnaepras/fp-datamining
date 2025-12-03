# Skrip Presentasi Video

DOGE Patterned Dataset & Diamond Crash Strategy

Pembagian peran (urut bicara):

- **P1** = Pembicara 1 → Bagian 1–4
- **P2** = Pembicara 2 → Bagian 5–7
- **P3** = Pembicara 3 → Bagian 8–11

Silakan ganti P1/P2/P3 dengan nama kalian saat rekaman.

---

## 1. Opening & Overview (P1)

**P1:**  
“Halo semuanya, di video ini kami akan menjelaskan isi notebook `DOGE_Patterned_Dataset_CRISPDM.ipynb`.  
Fokus utama notebook ini adalah membangun **Patterned Dataset** untuk aset kripto DOGE, mendeteksi peristiwa **Diamond Crash**, dan menghitung potensi **return on investment (ROI)** dari strategi trading yang membeli saat Diamond Crash terjadi.”

**P1:**  
“Notebook ini mengikuti alur **CRISP-DM**: mulai dari _business understanding_, _data understanding_, _data preparation_, _modeling_, _evaluation_, sampai _deployment_.  
Semua kode, data, dan gambar yang kami tunjukkan di video ini dihasilkan langsung dari notebook tersebut.”

**P1:**  
"Secara data, kami menggunakan **data historis harian DOGE** dalam format OHLCV: open, high, low, close, dan volume.  
Dari data ini, kami bentuk fitur-fitur baru seperti **R, TR, LR, PTR, dan PLR**, lalu kami klasifikasikan hari-hari tertentu menjadi kondisi **crash**, **moon**, dan **neutral**."

**P1:**  
"Di bagian akhir, kami akan tunjukkan berapa banyak **Diamond Crash** yang terdeteksi, bagaimana **D ROI** (24 jam setelah crash) dan **M ROI** (hingga akhir bulan), serta perbandingan strategi Diamond Crash dengan strategi sederhana **buy & hold**."

---

## 2. Setup Environment & Library (P1)

**P1 (scroll ke awal notebook, Cell 0–4):**  
“Di bagian awal notebook, ada sel untuk **cek versi Python** dan mengatur supaya **warnings** tidak mengganggu tampilan output.  
Ini hanya untuk memastikan lingkungan eksekusi stabil dan output bersih ketika notebook dijalankan ulang.”

**P1:**  
“Berikutnya, kita lihat sel `Import semua library yang dibutuhkan`.  
Di sini kami mengimpor:

- `pandas` dan `numpy` untuk manipulasi data,
- `matplotlib` dan `seaborn` untuk visualisasi,
- `sklearn` untuk **K-Means** dan metrik evaluasi clustering,
- library seperti `yfinance` untuk mengambil data harga DOGE,
- serta `pathlib` untuk mengatur struktur folder `data/raw`, `data/processed`, dan `figures`.”

**P1:**  
“Lalu di sel `Definisi path untuk output`, kami mendefinisikan lokasi penyimpanan:

- data mentah di `data/raw/`,
- data terproses di `data/processed/`,
- dan gambar di `figures/`.  
  Dengan begini, setiap kali notebook dijalankan, semua output akan otomatis tersimpan ke folder yang konsisten.”

---

## 3. Pengambilan Data DOGE & EDA (P1)

**P1 (Cell 7 - ambil data):**  
"Sekarang kita masuk ke tahap **Data Understanding**.  
Di sel `Gunakan yfinance untuk mendapatkan data DOGE`, notebook mengambil **data harga harian DOGE** melalui API.  
Data ini kemudian disimpan sebagai cache di file `data/raw/doge_ohlc_daily.csv`, sehingga kalau notebook dijalankan lagi, dia tidak perlu memanggil API terus-menerus."

**P1 (Cell 9 - EDA dasar):**  
"Setelah data tersimpan, kita baca kembali file `doge_ohlc_daily.csv` menggunakan `pandas`.  
Hasil bacaan datanya memiliki **909 baris dan 6 kolom**:

- `timestamp`
- `open`, `high`, `low`, `close`
- `volume`

Notebook menampilkan `head()`, `info()`, dan `describe()` untuk memastikan:

- tipe data sudah sesuai,
- tidak ada kolom yang aneh,
- dan range harga serta volume terlihat wajar."

**P1 (Cell 11 - data quality check):**  
"Di sel `Data Quality Check`, kode mengecek **missing values** dan **duplikasi timestamp**.  
Outputnya menunjukkan bahwa:

- tidak ada baris duplikat berdasarkan `timestamp`,
- tidak ada nilai kosong pada kolom harga dan volume yang mengganggu.  
  Jadi dataset siap dipakai untuk tahap berikutnya."

**P1 (Cell 12 – visualisasi timeseries):**  
“Masih di tahap Data Understanding, kami memvisualisasikan **pergerakan harga penutupan dan volume harian DOGE** ke dalam grafik time series.  
Grafiknya disimpan sebagai `figures/doge_price_volume_timeseries.png`.  
Di gambar ini terlihat bahwa:

- harga DOGE sangat volatil dengan beberapa lonjakan dan penurunan tajam,
- volume perdagangan cenderung naik saat terjadi pergerakan harga yang ekstrem.  
  Ini memberi gambaran awal bahwa DOGE memang aset berisiko tinggi, cocok untuk studi pola crash dan moon.”

---

## 4. Patterned Dataset: R, TR, LR, PTR, PLR (P2)

**P1 (Cell 14 – copy untuk patterned dataset):**  
“Masuk ke tahap **Data Preparation**.  
Di sel `Buat copy DataFrame untuk Patterned Dataset`, kami membuat salinan data harga harian yang nanti akan diperkaya dengan fitur-fitur Patterned Dataset.”

**P1 (Cell 14–16 – perhitungan fitur):**  
“Untuk setiap hari t, kami hitung lima fitur utama:

- `R_t = H_t - L_t` → rentang harga harian,
- `TR_t = H_t - C_t` → jarak antara harga penutupan dan harga tertinggi,
- `LR_t = C_t - L_t` → jarak antara harga penutupan dan harga terendah,
- `PTR` dan `PLR` adalah versi persentase dari posisi harga penutupan terhadap high dan low,  
  dengan sifat penting: **`PTR + PLR = 100`** untuk setiap hari.”

**P1 (Cell 15 – validasi PTR+PLR):**  
“Selanjutnya, ada sel untuk memvalidasi bahwa `PTR + PLR` selalu 100.  
Di output, kita melihat bahwa tidak ada pelanggaran aturan ini.  
Artinya rumus Patterned Dataset sudah konsisten dan bisa dipercaya untuk analisis berikutnya.”

---

## 5. Klasifikasi Crash, Moon, Neutral & Varian Dataset (P2)

**P2 (Cell 16–18 – definisi kondisi):**  
“Berikutnya, kami mendefinisikan **kondisi pasar harian**: crash, moon, atau neutral.  
Logikanya, secara sederhana:

- Hari **crash**: harga penutupan sangat dekat dengan _low_ harian → PLR sangat besar, PTR kecil.
- Hari **moon**: harga penutupan sangat dekat dengan _high_ harian → PTR besar.
- Hari **neutral**: hari-hari yang tidak memenuhi kriteria crash maupun moon.”

**P2 (visualisasi PTR vs PLR):**  
“Hasil klasifikasi ini divisualisasikan dalam scatter plot PTR vs PLR yang disimpan sebagai `figures/ptr_plr_distribution.png`.  
Titik-titik diberi warna berbeda untuk crash, moon, dan neutral.  
Di plot ini terlihat bahwa hari-hari crash dan moon cenderung berada di area yang terpisah jelas di ruang fitur PTR–PLR,  
yang menjadi indikasi awal bahwa fitur ini cocok untuk clustering.”

**P2 (Cell 19–20 – varian patterned dataset):**  
“Setelah itu, kami membentuk tiga varian **Patterned Dataset**:

1. `complete` → berisi semua hari perdagangan (909 baris),
2. `crash` → hanya hari-hari yang terlabel crash (86 baris),
3. `moon` → hanya hari-hari yang terlabel moon (100 baris).

Masing-masing varian disimpan ke:

- `data/processed/doge_patterned_complete.csv`,
- `data/processed/doge_patterned_crash.csv`,
- `data/processed/doge_patterned_moon.csv`.  
  Ini memudahkan kita melakukan eksperimen terpisah untuk tiap kondisi.”

---

## 6. K-Means Clustering pada Patterned Dataset (P2)

**P2 (Cell 22 – feature selection):**  
“Selanjutnya, kita masuk ke tahap **Modeling** dengan algoritma **K-Means clustering**.  
Di sel `Feature selection untuk clustering`, kami memilih beberapa fitur utama Patterned Dataset, lalu melakukan **standardisasi**  
agar semua fitur berada pada skala yang sama sebelum dimasukkan ke K-Means.”

**P2 (Cell 23 – jalankan K-Means):**  
“Di sel `Jalankan K-means pada semua dataset variants`, K-Means diterapkan ke ketiga varian: complete, crash, dan moon.  
Hasilnya, setiap hari pada masing-masing varian akan memiliki **label cluster**.  
Selain itu, untuk tiap varian kami hitung beberapa **metrik evaluasi eksternal** seperti:

- Mutual Information, Adjusted Mutual Information, Normalized Mutual Information,
- Rand Index dan Adjusted Rand Index,
- Fowlkes–Mallows Index,
- Homogeneity dan V-measure.”

**P2 (Cell 24 & data/processed/clustering_metrics.csv):**  
“Semua metrik ini dirangkum dalam file `data/processed/clustering_metrics.csv`.  
Kalau kita lihat isinya:

- Dataset **Complete** (909 sampel) memiliki rata-rata skor sekitar **0,91**,
- Dataset **Crash** (86 sampel) rata-ratanya sekitar **0,78**,
- Dataset **Moon** (100 sampel) rata-ratanya sekitar **0,77**.

Ini menunjukkan bahwa kualitas cluster pada Patterned Dataset tergolong **baik**,  
dan performa terbaik diperoleh pada dataset Complete.”

**P2 (Cell 25 – visualisasi clustering):**  
“Notebook juga menghasilkan visualisasi hasil clustering yang disimpan sebagai `figures/kmeans_clustering_comparison.png`.  
Di gambar ini, titik-titik data ditampilkan dalam dua dimensi (misalnya menggunakan PTR dan PLR) dan diwarnai menurut cluster K-Means.  
Terlihat bahwa titik-titik dalam cluster yang sama cenderung mengumpul, sehingga memvalidasi bahwa Patterned Dataset memang memudahkan pemisahan pola harga.”

**P2 (Cell 32–33 – heatmap metrik):**  
“Selanjutnya, ada sel `Visualisasi perbandingan metrik clustering` yang menghasilkan `figures/clustering_metrics_heatmap.png`.  
Heatmap ini memperlihatkan nilai metrik untuk ketiga dataset.  
Baris untuk dataset Complete cenderung memiliki warna paling ‘panas’ (nilai tinggi),  
sedangkan Crash dan Moon sedikit lebih rendah, namun tetap berada di kisaran yang baik.  
Ini menguatkan kesimpulan bahwa pendekatan Patterned Dataset cukup efektif.”

---

## 7. Diamond Crash Detection & Perhitungan ROI (P3)

**P2 (Cell 26–28 – deteksi Diamond Crash):**  
“Sekarang kita masuk ke bagian inti: **Diamond Crash Detection & ROI Calculation**.  
Pertama, dari dataset crash, kami pilih hanya hari-hari crash yang **paling ekstrem**.  
Kriterianya, nilai **PTR** hari tersebut berada di **persentil ke-90** tertinggi di antara semua hari crash.  
Hari-hari inilah yang kami definisikan sebagai **Diamond Crash**.”

**P2:**  
“Untuk setiap Diamond Crash, notebook menyimpan informasi penting ke file `data/processed/doge_diamond_crash_events.csv`, antara lain:

- `dc_date` → tanggal Diamond Crash,
- `dc_price` → harga penutupan pada hari Diamond Crash,
- `ptr` → nilai PTR pada hari itu (menggambarkan posisi harga),
- `d_max_price` dan `d_max_date` → harga maksimum dan tanggalnya dalam jendela **24 jam setelah Diamond Crash**,
- `m_max_price` dan `m_max_date` → harga maksimum dan tanggalnya **hingga akhir bulan** yang sama.

Total terdapat **86 peristiwa Diamond Crash** dalam periode pengamatan.”

**P2 (contoh isi doge_diamond_crash_events.csv):**  
“Kalau kita lihat beberapa baris pertama:

- 4 Juni 2023 (`2023-06-04`), `dc_price` ≈ 0,07247 USD dengan PTR ≈ 99,36.
- 22 Juni 2023 (`2023-06-22`), `dc_price` ≈ 0,06564 USD dengan PTR ≈ 97,31.  
  Contoh-contoh ini menunjukkan bahwa Diamond Crash benar-benar menangkap hari-hari ketika harga penutupan sangat dekat dengan low harian.”

**P2 (Cell 28–30 – D ROI & M ROI):**  
“Berikutnya, untuk setiap Diamond Crash, notebook menghitung dua jenis ROI:

1. **D ROI (Daily ROI)** → potensi profit maksimum dalam 24 jam setelah Diamond Crash:  
   `D ROI = (P_max_24h - C_dc) / C_dc × 100%`
2. **M ROI (Monthly ROI)** → potensi profit maksimum hingga akhir bulan yang sama:  
   `M ROI = (P_max_month - C_dc) / C_dc × 100%`

Di sini `C_dc` adalah harga penutupan saat Diamond Crash,  
`P_max_24h` adalah harga tertinggi dalam 24 jam setelahnya,  
dan `P_max_month` adalah harga tertinggi hingga akhir bulan kalender tersebut.”

**P2 (contoh D ROI vs M ROI):**  
“Sebagai contoh:  
Untuk salah satu event bulan Juli 2023,

- D ROI sekitar **1,18%**,
- sedangkan M ROI bisa mencapai sekitar **28%** sampai puncak harga di akhir bulan.  
  Ini menggambarkan bahwa jika posisi dibiarkan sampai akhir bulan, POTENSI profitnya bisa jauh lebih besar dibanding hanya mengincar 24 jam pertama.”

---

## 8. Ringkasan Bulanan ROI & Visualisasi (P3)

**P3 (Cell 29 – agregasi bulanan):**  
“Setelah ROI per event dihitung, notebook mengagregasikannya per bulan dan menyimpannya ke `data/processed/doge_monthly_roi_summary.csv`.  
Kolom penting di tabel ini antara lain:

- `year_month` → bulan dan tahun,
- `d_profit` dan `d_roi` → total profit dan total ROI harian (24 jam) dari semua Diamond Crash di bulan itu,
- `m_profit` dan `m_roi` → total profit dan total ROI bulanan,
- `t_dc` → jumlah event Diamond Crash di bulan tersebut,
- `d_roi_avg` dan `m_roi_avg` → rata-rata D ROI dan M ROI per event.”

**P3 (contoh isi doge_monthly_roi_summary.csv):**  
“Kalau kita lihat beberapa baris pertama hasil agregasi:

- **Juni 2023**:
  - `t_dc = 2`,
  - total D ROI ≈ **6,35%**,
  - total M ROI juga ≈ **6,35%**.
- **Juli 2023**:
  - `t_dc = 2`,
  - total D ROI ≈ **5,78%**,
  - total M ROI melonjak hingga sekitar **47,99%**.

Ini memperlihatkan bahwa M ROI sering kali **jauh lebih besar** dibanding D ROI,  
karena ada pergerakan harga signifikan beberapa minggu setelah Diamond Crash.”

**P3 (Cell 30 – visualisasi ROI):**  
“Visualisasi hubungan D ROI dan M ROI per bulan disimpan sebagai `figures/roi_comparison.png`.  
Di grafik ini, tiap bulan ditampilkan dua bar: satu untuk D ROI, satu untuk M ROI.  
Secara visual, kita bisa lihat bahwa **bar M ROI sering lebih tinggi** di banyak bulan,  
menunjukkan potensi keuntungan yang lebih besar jika strategi Diamond Crash dipadukan dengan horizon waktu bulanan.”

---

## 9. Perbandingan dengan Strategi Buy & Hold (P3)

**P3 (Cell 34–35):**  
“Notebook ini juga membandingkan strategi **beli saat Diamond Crash** dengan strategi **buy & hold**.  
Strategi buy & hold di sini adalah:

- beli DOGE di awal periode pengamatan,
- tahan sampai akhir periode tanpa transaksi tambahan.

Sedangkan strategi Diamond Crash adalah:

- hanya masuk posisi ketika terjadi event Diamond Crash sesuai deteksi Patterned Dataset.”

**P3:**  
“Hasil perbandingan ini divisualisasikan di `figures/strategy_comparison.png`.  
Secara umum, grafik menunjukkan bahwa:

- secara historis, strategi Diamond Crash bisa memberikan ROI yang kompetitif,
- namun strategi ini membutuhkan **lebih banyak transaksi** dan kedisiplinan untuk selalu masuk ketika sinyal Diamond Crash muncul.

Notebook juga menekankan bahwa ini adalah hasil **backtesting historis**,  
bukan jaminan performa di masa depan.”

---

## 10. Evaluasi Clustering, Keterbatasan, dan Deployment (P3)

**P3 (Cell 33 – temuan evaluasi clustering):**  
“Pada bagian **Temuan Evaluasi Clustering**, notebook merangkum bahwa:

- Patterned Dataset yang dibangun dari fitur R, TR, LR, PTR, dan PLR mampu membentuk cluster harga DOGE yang cukup terstruktur,
- dataset Complete memberikan metrik clustering terbaik,
- namun dataset Crash dan Moon tetap menunjukkan kualitas cluster yang memadai untuk analisis khusus crash/moon.”

**P3 (Cell 36 – keterbatasan & catatan):**  
“Di bagian **Keterbatasan & Catatan Penting**, kami menuliskan beberapa poin:

- Analisis hanya menggunakan sekitar **909 hari data**; mungkin belum cukup untuk semua kondisi pasar,
- Hasil ROI tidak memperhitungkan **biaya transaksi, slippage, ataupun resiko likuiditas**,
- Parameter seperti threshold PTR/PLR dan jumlah cluster K pada K-Means masih bisa dioptimasi lebih jauh.  
  Jadi, hasil ini sebaiknya dilihat sebagai prototipe strategi, bukan rekomendasi investasi langsung.”

**P3 (Cell 38–40 – deployment & summary):**  
“Tahap terakhir adalah **Deployment**.  
Notebook mengekspor semua hasil ke:

- `data/raw/` → data mentah DOGE OHLCV,
- `data/processed/` → Patterned Dataset, event Diamond Crash, dan ringkasan ROI,
- `figures/` → seluruh grafik utama seperti timeseries harga, distribusi PTR–PLR, heatmap clustering, ROI comparison, dan strategy comparison.

Sel `Final Summary` menampilkan tabel ringkasan akhir yang merangkum metrik-metrik penting secara ringkas.  
Dengan struktur ini, peneliti lain bisa dengan mudah menjalankan ulang seluruh pipeline, mengganti parameter, atau memperpanjang periode data.”

---

## 11. Penutup (P3)

**P3:**  
“Jadi, lewat notebook `DOGE_Patterned_Dataset_CRISPDM.ipynb` ini, kami menunjukkan bagaimana tahapan CRISP-DM diterapkan untuk menganalisis aset kripto DOGE  
mulai dari pengambilan data, pembuatan Patterned Dataset, clustering, sampai evaluasi strategi trading Diamond Crash.”

**P3:**  
“Dari sisi teknis, kita belajar bagaimana mengubah data OHLCV menjadi fitur-fitur yang lebih informatif,  
bagaimana menggunakan K-Means untuk mengelompokkan pola harga,  
dan bagaimana mengevaluasi kualitas cluster dengan berbagai metrik seperti MI, ARI, dan V-measure.”

**P3:**  
“Dari sisi bisnis, kita melihat bahwa strategi membeli saat **Diamond Crash** secara historis bisa memberikan **M ROI** bulanan yang cukup menarik,  
sering kali lebih besar daripada hanya mengincar pergerakan dalam 24 jam pertama,  
dan bisa bersaing dengan strategi sederhana buy & hold.  
Namun tentu saja, semua itu datang dengan risiko dan belum mempertimbangkan biaya transaksi.”

**P3:**  
“Ke depan, penelitian ini bisa dikembangkan lagi dengan memperpanjang periode data, menguji kombinasi parameter Patterned Dataset dan jumlah cluster K yang lebih luas, memasukkan faktor biaya transaksi dan risiko pasar, serta membandingkan Diamond Crash dengan strategi kuantitatif lain di aset kripto maupun instrumen keuangan yang berbeda.”

**P3:**  
“Sekian presentasi dari kami.  
Terima kasih sudah menonton, semoga penjelasan ini bermanfaat dan bisa menjadi dasar untuk eksplorasi strategi trading maupun penelitian lanjutan.”

**P3:**  
“Kalau ada pertanyaan atau saran, silakan disampaikan.  
Terima kasih, dan sampai jumpa di video berikutnya.”
