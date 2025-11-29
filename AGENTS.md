# AGENTS.md – DOGE Patterned Dataset & ROI (CRISP-DM, Jupyter)

> **Studi kasus:** adaptasi paper *“Prediction of ROI Achievements and Potential Maximum Profit on Spot Bitcoin Rupiah Trading Using K-means Clustering and Patterned Dataset Model”* tetapi memakai **DOGE** sebagai aset utama dan kerangka **CRISP-DM**. 

---

## 1. Peran & Tujuan Agent

Kamu adalah **AI Data Scientist** yang bekerja di dalam satu file **Jupyter Notebook (`.ipynb`)**.

**Goal utama:**

1. Membangun ulang **Patterned Dataset Model** seperti di paper, tapi:

   * Aset: **DOGE/IDR**.
   * Periode data: **historical terbaru** (±2–3 tahun terakhir sampai tanggal eksekusi).
2. Melakukan proses **CRISP-DM end-to-end**:

   * Business understanding → Deployment.
3. Menghitung dan membandingkan:

   * **D Profit / D ROI (%)**: potensi profit maksimum dalam 24 jam setelah level **Diamond Crash** pertama di suatu hari.
   * **M Profit / M ROI (%)**: potensi profit maksimum dalam satu bulan setelah level **Diamond Crash** pertama muncul di bulan tersebut.
4. Menghasilkan notebook yang:

   * Terstruktur per fase **CRISP-DM**.
   * Tiap cell: **tulis kode → jalankan → baca & gunakan output → baru lanjut ke langkah berikutnya**.
   * Mudah direplikasi dan dipakai ulang.
5. Menampilkan Visualisasi yang mudah dipahami.

---

## 2. Aturan Umum Eksekusi Notebook

* Notebook utama bernama misalnya: `DOGE_Patterned_Dataset_CRISPDM.ipynb`.
* **Setiap langkah kecil = 1–3 cell** maksimum.
  Urutannya SELALU:

  1. Tulis kode jelas & terkomentar.
  2. Run cell.
  3. **Periksa output** (misalnya dengan `print()`, `head()`, plot sederhana).
  4. Jika ada error / hasil aneh → perbaiki di cell berikutnya (jangan diam-diam mengasumsikan berhasil).
* **JANGAN** melompat ke tahap selanjutnya sebelum:

  * Data terbaca dengan benar.
  * Bentuk kolom terverifikasi.
  * Rumus sudah dicek di contoh kecil.
* Kalau scraping/web API:

  * Selalu ambil **data terbaru** (gunakan parameter tanggal dinamis sampai “hari ini”).
  * Simpan hasil ke file lokal (`.csv`) sebagai cache supaya analisis berikutnya tidak perlu memanggil API terus.

---

## 3. Stack & Library

Gunakan Python (minimal 3.9) dengan library utama:

* Data: `pandas`, `numpy`, `datetime`
* Visualisasi: `matplotlib`, (opsional) `seaborn`, `plotly` untuk interaktif
* ML: `scikit-learn`
* Deep learning (opsional, kalau sampai meniru bagian GRU/LSTM-Conv2D):

  * `tensorflow` atau `pytorch` (pilih satu)
* HTTP/API: `requests` atau client resmi dari exchange (jika ada)
* Lain: `tqdm`, `warnings`, dll sesuai kebutuhan

Pastikan ada satu cell di awal untuk:

```python
import sys, warnings
warnings.filterwarnings("ignore")
print(sys.version)
```

dan satu cell untuk meng-`import` semua library sekaligus.

---

## 4. Kerangka CRISP-DM & Breakdown Tugas

Struktur besar notebook harus mengikuti CRISP-DM dengan **section markdown**:

### 4.1. Business Understanding

**Tujuan:**

* Menjelaskan dalam markdown:

  * Masalah bisnis:

    > “Jika trader membeli DOGE pada saat kondisi **Diamond Crash** (harga mendekati low harian), seberapa besar potensi **profit & ROI** yang bisa dicapai **dalam 24 jam (D)** dan **dalam 1 bulan (M)**?”
  * Pertanyaan riset:

    * Apakah **M ROI (%)** secara konsisten > **D ROI (%)** seperti hasil paper pada Bitcoin? 
    * Bagaimana frekuensi munculnya Diamond Crash per bulan?
  * Lingkup:

    * Hanya **spot DOGE** (bukan futures/leverage).
    * Satu pair quote utama (mis. DOGE/USDT).

**Tugas untuk agent:**

* Buat 1–2 cell markdown yang:

  * Merangkum paper asli singkat (1 paragraf).
  * Menjelaskan adaptasi ke DOGE dan tujuan skripsi/proyek.

---

### 4.2. Data Understanding

**Target:**

* Mengambil **historical OHLCV DOGE** (Open, High, Low, Close, Volume) dengan resolusi **1 jam atau 1 hari** (minimal daily; ideal hourly lalu diagregasi).
* Periode: minimal **2 tahun terakhir hingga hari ini**.

**Tugas data:**

1. **Scraping / API call**

   * Pilih sumber yang stabil: contoh:

     * Exchange (Binance, Coinbase, Indodax, dll) via REST API.
     * Atau aggregator seperti CoinGecko (kalau support OHLC).
   * Parameter dinamis:

     * `symbol="DOGEUSDT"` (atau yang sesuai).
     * Tanggal mulai: `today - 2 years`.
     * Tanggal akhir: `today`.
   * Simpan ke `data/raw/doge_ohlc.csv`.

2. **Exploratory data understanding**

   * Baca CSV → `pandas.DataFrame`.
   * Tampilkan:

     * `df.head()`
     * `df.info()`
     * Statistik dasar: `df.describe()`.
   * Pastikan kolom minimal: `timestamp`, `open`, `high`, `low`, `close`, `volume`.

3. **Quality check**

   * Cek missing value per kolom.
   * Cek duplikat timestamp.
   * Plot sederhana:

     * Time series harga close.
     * High vs Low untuk beberapa bulan contoh.

**Catatan cell:**

* Setiap bagian (scrape → simpan → load → cek) dipisahkan menjadi beberapa cell, dan selalu jalankan + baca output sebelum lanjut.

---

### 4.3. Data Preparation (Patterned Dataset Model)

Adaptasi **Patterned Dataset Model** dari paper untuk DOGE. 

**Langkah:**

1. **Resampling & agregasi harian**

   * Jika data hourly → resample ke daily:

     * `H = high harian`
     * `L = low harian`
     * `C = close harian` (akhir hari)
     * Simpan sebagai `df_daily`.

2. **Hitung fitur Patterned Dataset**
   Untuk setiap hari, buat kolom:

   * `R = H - L`
   * `TR = H - C`   (Top Range)
   * `LR = C - L`   (Lower Range)
   * `PTR = TR / R * 100` (Percent Top Range)
   * `PLR = LR / R * 100` (Percent Low Range)

   Tambahkan cell untuk:

   * Menghitung semua kolom di atas.
   * Cek beberapa baris (`head()`) untuk memvalidasi rumus.

3. **Definisi kondisi Crash & Moon**

   * Mengikuti paper:

     * **Crash**: DOGE sangat dekat dengan low harian → `PTR ≈ 100`, `PLR ≈ 0`.
       Gunakan threshold realistis, misalnya:

       * Crash jika `PTR >= 95` dan `PLR <= 5`.
     * **Moon**: DOGE dekat high harian → `PLR ≈ 100`, `PTR ≈ 0`.
       Threshold misalnya:

       * Moon jika `PLR >= 95` dan `PTR <= 5`.
   * Tambah kolom kategori:

     * `condition = "crash" | "moon" | "neutral"`.

4. **Patterned dataset variants**
   Buat tiga DataFrame terpisah:

   * `df_complete`  → semua baris.
   * `df_crash`     → filter `condition == "crash"`.
   * `df_moon`      → filter `condition == "moon"`.

   Tambah cell untuk print dimensi tiap dataset dan distribusi label.

---

### 4.4. Modeling (Clustering & ROI) – CRISP-DM: Modelling

#### 4.4.1. K-means clustering pada Patterned Dataset

Tujuan: meniru langkah paper yang membandingkan kualitas clustering pada **Complete**, **Moon**, dan **Crash**, lalu menunjukkan bahwa kondisi **Crash** memberi skor terbaik. 

**Langkah:**

1. **Feature selection & scaling**

   * Pilih subset fitur numeric: misalnya `[R, TR, LR, PTR, PLR]`.
   * Normalisasi dengan `MinMaxScaler` atau `StandardScaler`.
   * Simpan scaler untuk reuse.

2. **K-means, K=2**

   * Untuk tiap dataset (`complete`, `moon`, `crash`):

     * Jalankan `KMeans(n_clusters=2, random_state=…)`.
     * Simpan label cluster.
   * Tambah cell untuk visualisasi 2D sederhana (misalnya PTR vs PLR dengan warna cluster).

3. **Hitung metrik clustering**
   Untuk tiap dataset dan bulan (opsional, atau untuk keseluruhan dulu), hitung:

   * `mutual_info_score`
   * `adjusted_mutual_info_score`
   * `normalized_mutual_info_score`
   * `rand_score`
   * `adjusted_rand_score`
   * `fowlkes_mallows_score`
   * `homogeneity_score`
   * `v_measure_score`

   Simpan hasil dalam DataFrame `cluster_scores` dengan kolom:

   * `dataset_type` (complete/moon/crash)
   * nama metrik
   * nilai

4. **Analisis hasil**

   * Cell markdown untuk menjelaskan:

     * Dataset mana yang punya skor rata-rata tertinggi → diharapkan **Crash**.
     * Hubungkan dengan temuan paper.

---

#### 4.4.2. Deteksi Diamond Crash & Perhitungan D/M ROI

Adaptasi ide **Diamond Crash**:
hari di mana DOGE mencapai kondisi “crash” (PTR tinggi, PLR rendah) yang dianggap sebagai sinyal potensial pembelian.

**Definisi praktis untuk DOGE (boleh disesuaikan di kode):**

* Diamond Crash terjadi pada hari `t` bila:

  * `condition == "crash"` DAN
  * misalnya `PTR` berada di **top 10%** sepanjang periode.
* Jika dalam satu hari banyak baris (kalau pakai resolusi < 1D), pilih waktu pertama Diamond Crash hari itu.

**Langkah coding:**

1. **Identifikasi Diamond Crash harian**

   * Buat fungsi untuk:

     * Loop per hari.
     * Cari baris pertama yang memenuhi kondisi Crash (threshold).
     * Simpan:

       * `dc_date` (timestamp)
       * `dc_price_start` (close pada saat DC)
   * Hasilkan DataFrame `dc_events`.

2. **Perhitungan M Profit / M ROI (per bulan)**

   * Untuk setiap `dc_event`:

     * Beli 1 DOGE di `dc_price_start` (anggap 1 unit untuk kesederhanaan).
     * Cari **harga maksimum** DOGE (`max_price`) dari `dc_date` hingga **akhir bulan** yang sama.
     * `M_max_profit = max_price - dc_price_start`
     * `M_ROI(%) = (M_max_profit / dc_price_start) * 100`
     * Catat juga `M_profit_time` (selisih waktu dari DC ke saat max tercapai).
   * Agregasi per bulan:

     * `M_Profit_month = sum(M_max_profit)`
     * `M_ROI_month = sum(M_ROI)` atau metrik lain yang kamu definisikan dengan jelas (ikuti pendekatan paper sebisanya). 

3. **Perhitungan D Profit / D ROI (per hari → agregasi bulanan)**

   * Untuk tiap `dc_event`:

     * Interval waktu: dari `dc_date` sampai `dc_date + 24 jam`.
     * Cari `D_max_price` di interval itu.
     * `D_max_profit = D_max_price - dc_price_start`
     * `D_ROI(%) = (D_max_profit / dc_price_start) * 100`
   * Agregasi per bulan:

     * `D_Profit_month = sum(D_max_profit)`
     * `D_ROI_month = sum(D_ROI)`.

4. **Ringkasan & visualisasi**

   * Buat tabel seperti **Table VI** di paper:
     `year-month`, `M_Profit`, `M_ROI`, `D_Profit`, `D_ROI`, `T_DC`.
   * Plot:

     * Line chart `M_Profit` vs `D_Profit` per bulan.
     * Line chart `M_ROI` vs `D_ROI` per bulan.
   * Gunakan cell markdown untuk menafsirkan hasil:

     * Apakah M ROI untuk DOGE juga > D ROI (seperti Bitcoin di paper)?

---

### 4.5. Evaluation (CRISP-DM)

**Tujuan:**

* Menilai:

  * Kualitas model Patterned Dataset + K-means untuk DOGE.
  * Kelayakan strategi “beli DOGE saat Diamond Crash” secara historis.

**Tugas:**

1. Evaluasi teknis:

   * Diskusikan metrik clustering → apakah Crash tetap paling “terstruktur”.
   * Jelaskan asumsi Diamond Crash & threshold yang digunakan.

2. Evaluasi bisnis:

   * Bandingkan hasil dengan strategi **buy & hold**:

     * Misal: beli 1 DOGE di awal periode vs strategi Diamond Crash.
   * Tulis poin pro/kontra:

     * Risiko, frekuensi DC, periode drawdown, dll.

3. Keterbatasan:

   * Biaya transaksi & slippage tidak dimasukkan.
   * Hasil masa lalu bukan jaminan masa depan.
   * Sumber data & potensi bias.

Semua evaluasi ditulis dalam beberapa cell markdown, dengan referensi eksplisit ke angka & grafik di notebook.

---

### 4.6. Deployment (CRISP-DM)

**Hasil akhir yang harus ada:**

1. **Notebook rapi** dengan struktur:

   * 00 – Setup & imports
   * 01 – Business Understanding
   * 02 – Data Understanding
   * 03 – Data Preparation (Patterned Dataset)
   * 04 – Modeling (K-means + ROI)
   * 05 – Evaluation & Conclusion
   * (Opsional) 06 – Deep Learning (GRU vs ConvLSTM2D untuk patterned crash DOGE)

2. **Folder output**:

   * `data/raw/…`       → hasil scraping asli.
   * `data/processed/…` → patterned dataset & event DC.
   * `figures/…`        → grafik penting (ROI per bulan, dsb).
   * `models/…`         → jika ada model yang disimpan.

3. **Ringkasan untuk skripsi/laporan**:

   * 1 cell markdown di akhir berisi bullet:

     * Tujuan.
     * Metode.
     * Hasil utama (misal: “M ROI rata-rata 30% vs D ROI 8% untuk periode X pada DOGE”).
     * Rencana pengembangan lanjut.

---

## 5. Gaya Coding & Dokumentasi

* Gunakan **nama variabel deskriptif** (`df_daily`, `df_patterned`, `df_dc_events`).

* Beri **komentar singkat** di atas blok kode penting:

  * Contoh:

    ```python
    # Hitung fitur Patterned Dataset sesuai R, TR, LR, PTR, PLR
    ```

* Setiap fungsi penting harus punya docstring yang menjelaskan:

  * Input, output, asumsi.

* Gunakan markdown untuk menghubungkan setiap blok kode dengan konsep CRISP-DM & bagian dari paper yang diadaptasi.

---

## 6. Prinsip Penting

* **Selalu**: `kode → run → baca output → lanjut`.
* Kalau scraping:

  * **Selalu ambil data terbaru** (hingga `today`).
  * Tapi simpan cache agar percobaan ulang tidak membanjiri API.
* Tujuan utama bukan hanya *working code*, tapi notebook yang bisa langsung dipakai sebagai **bagian analisis skripsi/proyek** dengan struktur ilmiah yang jelas.
