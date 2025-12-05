# Script Presentasi (Awal s.d. Modeling)

> Catatan: 3 orang, sebut saja **Pembicara 1 (P1)**, **Pembicara 2 (P2)**, **Pembicara 3 (P3)**. Silakan ganti dengan nama asli saat presentasi.

---
## Slide 1 – Judul & Tim (P1)

Selamat [pagi/siang/sore], perkenalkan kami dari kelompok [X].  
Hari ini kami akan mempresentasikan hasil proyek data mining berjudul:
“**Prediksi Pencapaian ROI dan Potensi Profit Maksimum pada Trading Spot DOGE Menggunakan K-Means Clustering dan Patterned Dataset Model**”.

Metodologi yang kami gunakan adalah **CRISP-DM (Cross-Industry Standard Process for Data Mining)** yang terdiri dari tahapan: Business Understanding, Data Understanding, Data Preparation, Modeling, Evaluation, dan Deployment.  
Pada sesi kali ini, kami akan fokus dulu pada bagian **Business Understanding** dan **Data Understanding**.

---
## Slide 2 – Latar Belakang (1) (P1)

Latar belakang penelitian kami berangkat dari karakteristik **cryptocurrency DOGE** yang sangat volatil.  
Bagi trader spot, volatilitas tinggi ini sering menimbulkan dua masalah utama:
1. Sulit menentukan **waktu beli yang optimal**, dan  
2. Sering **terlambat masuk** sehingga peluang profit terlewat, atau justru membeli di harga yang kurang menguntungkan.

Karena itu, kami ingin membangun sebuah pendekatan berbasis data mining yang dapat membantu mengidentifikasi **kondisi pasar tertentu** yang layak dijadikan **sinyal entry**.

---
## Slide 3 – Latar Belakang (2): Patterned Dataset Model (P1)

Untuk menjawab permasalahan tersebut, kami mengadopsi konsep **Patterned Dataset Model** yang mengubah data harga harian DOGE menjadi **fitur-fitur pola harga**.  
Tujuannya adalah menangkap posisi harga penutupan harian relatif terhadap harga tertinggi dan terendah pada hari yang sama.

Dengan pendekatan ini, kami bisa mengklasifikasikan apakah pada hari tertentu harga DOGE cenderung:
- mendekati **low harian** (kondisi **Crash**),
- mendekati **high harian** (kondisi **Moon**), atau
- berada di **zona tengah** (Neutral).

---
## Slide 4 – Tujuan Penelitian (P1)

Tujuan penelitian kami dirumuskan menjadi empat poin utama:
1. **Membangun Patterned Dataset Model** yang mentransformasi data OHLC harian DOGE menjadi fitur pola harga: **R, TR, LR, PTR, dan PLR**.
2. **Menerapkan K-Means Clustering** untuk mengidentifikasi kondisi pasar: **Crash** dan **Moon** berdasarkan kombinasi fitur tersebut.
3. **Mendeteksi kondisi Diamond Crash**, yaitu kondisi crash yang paling ekstrem dan diperlakukan sebagai **sinyal beli potensial**.
4. **Menghitung serta membandingkan ROI harian (D ROI) dan bulanan (M ROI)** setelah terjadi Diamond Crash.

---
## Slide 5 – Ruang Lingkup Penelitian (P1)

Ruang lingkup penelitian kami sebagai berikut:
- **Aset**: DOGE terhadap USD pada **spot market** (bukan futures atau leverage).
- **Periode Data**: sekitar **Juni 2023 sampai November 2025**, kurang lebih **2,5 tahun** data harian.
- **Sumber Data**: **Yahoo Finance** dengan ticker **DOGE-USD**.

Dengan ruang lingkup ini, hasil yang kami peroleh spesifik untuk DOGE pada periode tersebut, sehingga interpretasi ROI harus disesuaikan dengan konteks waktu dan aset yang sama.

---
## Slide 6 – Pertanyaan Riset (P1)

Dari tujuan tadi, kami merumuskan beberapa **pertanyaan riset utama**:
1. **Apakah M ROI (Monthly ROI) secara konsisten lebih besar daripada D ROI (Daily ROI) setelah Diamond Crash?**
2. **Bagaimana frekuensi munculnya kondisi Diamond Crash pada DOGE per bulan?**
3. **Apakah strategi “beli saat Diamond Crash” lebih menguntungkan dibandingkan strategi buy & hold biasa?**

Pertanyaan-pertanyaan ini akan dijawab melalui analisis pola harga, clustering, serta perhitungan dan komparasi ROI.

---
## Slide 7 – Definisi Istilah Kunci (1) (P1)

Sebelum masuk ke data, kami jelaskan dulu beberapa **istilah kunci** yang kami gunakan:
- **R (Range)**: selisih antara harga tertinggi dan harga terendah harian, yaitu `H - L`.
- **TR (Top Range)**: jarak dari **high** ke **close**, yaitu `H - C`.
- **LR (Lower Range)**: jarak dari **close** ke **low**, yaitu `C - L`.

Tiga komponen ini menggambarkan seberapa besar rentang pergerakan harga dan posisi harga penutupan di dalam rentang tersebut.

---
## Slide 8 – Definisi Istilah Kunci (2) (P1)

Berikutnya, kami definisikan dua persentase penting:
- **PTR (Percent Top Range)**: `(TR / R) × 100`, yaitu persentase jarak close terhadap bagian atas range.
- **PLR (Percent Low Range)**: `(LR / R) × 100`, yaitu persentase jarak close terhadap bagian bawah range.

Secara teori, **PTR + PLR = 100%**.  
Jika **PTR tinggi dan PLR rendah**, artinya harga close sangat dekat dengan **low** harian → indikasi **Crash**.  
Sebaliknya, **PLR tinggi dan PTR rendah** menandakan harga close mendekati **high** harian → indikasi **Moon**.

---
## Slide 9 – Definisi Crash, Moon, dan Diamond Crash (P1)

Berdasarkan kombinasi fitur tadi, kami mendefinisikan:
- **Crash**: kondisi ketika harga mendekati low harian, ditandai **PTR tinggi** dan **PLR rendah**.
- **Moon**: kondisi ketika harga mendekati high harian, ditandai **PLR tinggi** dan **PTR rendah**.

Lalu, **Diamond Crash** kami definisikan sebagai **subset dari Crash yang paling ekstrem**:
- Tetap memenuhi kriteria Crash, dan  
- Nilai PTR berada di **top 10%** tertinggi.  
Diamond Crash inilah yang kami anggap sebagai **sinyal beli potensial** untuk strategi masuk pasar.

---
## Slide 10 – Tahap CRISP-DM yang Dibahas (P1)

Sesuai framework **CRISP-DM**, pekerjaan kami terbagi menjadi beberapa fase utama:
1. **Business Understanding** – mendefinisikan masalah bisnis, tujuan, ruang lingkup, pertanyaan riset, dan istilah kunci.
2. **Data Understanding** – mengumpulkan, memeriksa, dan mengeksplorasi data yang akan digunakan.
3. **Data Preparation** – menyiapkan dan merekayasa fitur dari data mentah.
4. **Modeling** – membangun dan melatih model data mining atau machine learning.
5. **Evaluation** – mengevaluasi hasil model terhadap tujuan bisnis.
6. **Deployment** – menyiapkan hasil agar bisa dimanfaatkan secara praktis.

Pada bagian yang sedang kami jelaskan sekarang, fokus kami ada pada dua tahap pertama: **Business Understanding** dan **Data Understanding**, sebelum nanti dilanjutkan ke **Data Preparation** dan **Modeling**.

---
## Slide 11 – Data Acquisition (P1)

Pada tahap **Data Understanding**, langkah pertama adalah **data acquisition**.
Kami menggunakan library **`yfinance`** untuk mengambil **data historical OHLCV DOGE/USD** dari Yahoo Finance.

Detail pengambilan data:
Detail pengambilan data:
- **Ticker**: `DOGE-USD`  
- **Interval**: `1d` (daily, data harian)  
- **Periode**: kurang lebih **Juni 2023 sampai November 2025**  
- Kolom yang digunakan: `timestamp`, `open`, `high`, `low`, `close`, dan `volume`.

Setelah diunduh, data disimpan ke file CSV di **`data/raw/doge_ohlc_daily.csv`** sebagai **cache**, sehingga notebook tidak perlu selalu mengunduh ulang dari internet.

---
## Slide 12 – Informasi Dasar Dataset (P2)

Setelah data berhasil diambil, kami melakukan **pemeriksaan awal** terhadap struktur dataset.
Langkah-langkah yang kami lakukan di notebook antara lain:
1. Menampilkan **5 baris pertama** data (`head`) untuk memastikan kolom dan format sudah sesuai.
2. Menyusun **tabel ringkas tipe data** per kolom, jumlah nilai non-null, dan jumlah nilai yang hilang.
3. Menghitung **statistik deskriptif** seperti mean, min, max, dan quartile untuk kolom numerik.

Dari sini kami memastikan bahwa:
- Kolom-kolom OHLCV sudah lengkap,  
- Tipe data sudah sesuai (timestamp dan numerik), dan  
- Tidak ada missing value yang signifikan.

---
## Slide 13 – Data Quality Check (P2)

Berikutnya, kami melakukan **Data Quality Check** yang lebih sistematis.  
Ada beberapa hal yang kami cek:
1. **Missing values per kolom** beserta persentasenya.
2. **Duplicate timestamp**, untuk memastikan tidak ada duplikasi hari.
3. **Konsistensi OHLC**, misalnya:
   - `low` tidak boleh lebih besar dari `open` atau `close`,  
   - `close` dan `open` tidak boleh melebihi `high`.
4. Pemeriksaan nilai **negatif atau nol** pada harga dan volume.

Hasil quality check ini menunjukkan bahwa dataset **relatif bersih**, tanpa duplikasi timestamp dan tanpa anomali OHLC yang serius, sehingga layak digunakan untuk proses selanjutnya.

---
## Slide 14 – Visualisasi Time Series Harga & Volume (P2)

Masih di tahap Data Understanding, kami juga melakukan **visualisasi time series** untuk memahami pola pergerakan harga dan volume DOGE secara intuitif.

Di notebook, kami menampilkan dua grafik utama:
Di notebook, kami menampilkan dua grafik utama:
1. Grafik **harga close harian** dengan area **range high–low**, dilengkapi anotasi titik **harga maksimum** dan **minimum** selama periode observasi.
2. Grafik **volume trading harian** DOGE.

Dari visualisasi ini, kita dapat melihat:
- Periode-periode ketika harga DOGE mengalami lonjakan atau penurunan tajam, dan  
- Hubungan kasar antara **lonjakan volume** dengan **pergerakan harga**.

---
## Slide 15 – Ringkasan Tahap Awal (P2)

Sebagai ringkasan untuk tahap awal ini:
- Pada **Business Understanding**, kami mendefinisikan masalah, tujuan, ruang lingkup, pertanyaan riset, dan istilah-istilah kunci seperti R, TR, LR, PTR, PLR, Crash, Moon, dan Diamond Crash.
- Pada **Data Understanding**, kami mengakuisisi data DOGE harian dari Yahoo Finance, memeriksa struktur dan kualitas data, serta melakukan visualisasi awal harga dan volume.

Tahap-tahap ini memastikan bahwa **masalah bisnis dan data yang digunakan sudah jelas dan valid** sebelum kami lanjut ke tahap berikutnya seperti **Data Preparation** dan **Modeling**.

---
## Slide 16 – Transition ke Tahap Berikutnya (P2)

Demikian penjelasan kami untuk bagian **Business Understanding** dan **Data Understanding**.

Selanjutnya, kami akan masuk ke tahap **Data Preparation**, di mana kami mulai membangun **Patterned Dataset**, mengkategorikan kondisi Crash, Moon, dan Neutral, serta menyiapkan data untuk proses clustering dan perhitungan ROI.
Untuk bagian tersebut akan dijelaskan lebih lanjut pada sesi berikutnya.

---
## Slide 17 – Overview Data Preparation & Modeling (P3)

Pada bagian berikutnya, kami masuk ke tahap **Data Preparation** dan **Modeling** dalam kerangka CRISP-DM.
Di tahap ini, kami:
1. Mengubah data OHLC mentah menjadi **Patterned Dataset** dengan fitur R, TR, LR, PTR, dan PLR.
2. Mengklasifikasikan kondisi pasar menjadi **Crash**, **Moon**, dan **Neutral**.
3. Menyiapkan beberapa varian dataset untuk **clustering**.
4. Menerapkan **K-Means Clustering** dan mengevaluasi kualitas cluster.

Tahap ini menjadi jembatan antara data mentah dengan model yang akan digunakan untuk analisis ROI dan strategi trading.

---
## Slide 18 – Patterned Dataset Model: Fitur yang Digunakan (P3)

Pertama, kami membangun **Patterned Dataset** dengan menambahkan fitur-fitur berikut ke data harga harian:
- **R (Range)** = `H - L` → jarak antara harga tertinggi dan terendah harian.
- **TR (Top Range)** = `H - C` → jarak dari harga tertinggi ke harga penutupan.
- **LR (Lower Range)** = `C - L` → jarak dari harga penutupan ke harga terendah.
- **PTR (Percent Top Range)** = `(TR / R) × 100` → persentase TR terhadap range.
- **PLR (Percent Low Range)** = `(LR / R) × 100` → persentase LR terhadap range.

Secara matematis, ketika tidak ada error pada data, **PTR + PLR akan selalu sama dengan 100%**, sehingga keduanya memberikan pandangan komplementer tentang posisi harga penutupan di dalam range harian.

---
## Slide 19 – Interpretasi Fitur Patterned Dataset (P3)

Interpretasi dari kombinasi fitur tersebut adalah sebagai berikut:
- Jika **PTR tinggi dan PLR rendah**, berarti harga penutupan mendekati **low harian** → indikasi **Crash**.
- Jika **PLR tinggi dan PTR rendah**, berarti harga penutupan mendekati **high harian** → indikasi **Moon**.
- Jika nilai PTR dan PLR berada di tengah, harga penutupan cenderung berada di **zona tengah range** → kondisi **Neutral**.

Di notebook, kami juga melakukan validasi bahwa **PTR + PLR ≈ 100** untuk seluruh baris data, memastikan rumus yang digunakan sudah benar sebelum melangkah ke modeling.

---
## Slide 20 – Klasifikasi Kondisi: Crash, Moon, Neutral (P3)

Berikutnya, kami mengklasifikasikan setiap hari ke dalam tiga kondisi pasar berdasarkan nilai PTR dan PLR dengan threshold sebagai berikut:
- **CRASH**: `PTR ≥ 90%` dan `PLR ≤ 10%` → harga penutupan sangat dekat dengan low.
- **MOON**: `PLR ≥ 90%` dan `PTR ≤ 10%` → harga penutupan sangat dekat dengan high.
- **NEUTRAL**: kondisi lainnya.

Threshold 90%/10% dipilih sebagai kompromi yang **cukup ketat** untuk mendefinisikan ekstrem, namun masih realistis untuk karakteristik volatilitas DOGE. Hasil klasifikasi ini menandai hari-hari yang berpotensi menjadi **sinyal ekstrem** untuk strategi trading.

---
## Slide 21 – Ringkasan Distribusi Kondisi Pasar (P3)

Setelah klasifikasi, kami menghitung **distribusi jumlah hari** untuk masing-masing kondisi:
- Berapa banyak hari yang masuk kategori **Crash**.
- Berapa banyak hari yang masuk kategori **Moon**.
- Berapa banyak hari yang **Neutral**.

Di notebook, kami menampilkan tabel jumlah dan persentase tiap kondisi.
Dari sini terlihat bahwa:
- Kondisi **Neutral** mendominasi sebagian besar hari,
- Sementara **Crash** dan **Moon** terjadi jauh lebih jarang namun sangat penting, 
   karena keduanya mewakili kondisi **ekstrem** yang relevan untuk sinyal entry maupun exit.

---
## Slide 22 – Varian Patterned Dataset: Complete, Crash, Moon (P3)

Untuk keperluan analisis lebih lanjut, kami membentuk **tiga varian dataset**:
1. **`df_complete`**: berisi **seluruh hari** observasi dengan fitur Patterned Dataset.
2. **`df_crash`**: hanya berisi hari-hari dengan kondisi **Crash**.
3. **`df_moon`**: hanya berisi hari-hari dengan kondisi **Moon**.

Ketiga varian ini kemudian disimpan ke folder `data/processed` dalam bentuk CSV dan akan digunakan untuk membandingkan **kualitas clustering** antar kondisi pasar yang berbeda.

---
## Slide 23 – Tujuan Modeling dengan K-Means (P3)

Pada tahap **Modeling**, kami menggunakan algoritma **K-Means Clustering** dengan jumlah cluster **K = 2**.
Tujuannya adalah:
1. Mengelompokkan hari-hari trading berdasarkan pola fitur **R, TR, LR, PTR, dan PLR**.
2. Menguji seberapa baik K-Means mampu membedakan **pola harga ekstrem** dibandingkan pola harga yang lebih biasa.
3. Membandingkan performa clustering pada tiga dataset: **Complete**, **Crash**, dan **Moon**.

Dengan demikian, kami tidak hanya melihat apakah pola ekstrem itu ada, tetapi juga apakah pola tersebut dapat **diidentifikasi secara konsisten** oleh algoritma unsupervised.

---
## Slide 24 – Fitur & Preprocessing untuk K-Means (P3)

Untuk proses clustering, kami menggunakan fitur berikut sebagai input model:
- `R`, `TR`, `LR`, `PTR`, dan `PLR`.

Sebelum masuk ke K-Means, kami melakukan **scaling** menggunakan **MinMaxScaler** agar semua fitur berada dalam skala yang sebanding.
Hal ini penting karena:
- K-Means sensitif terhadap skala fitur,
- Fitur dengan rentang nilai besar dapat mendominasi jarak Euclidean jika tidak dinormalisasi.

Setelah scaling, barulah K-Means dijalankan untuk membentuk dua cluster pada masing-masing varian dataset.

---
## Slide 25 – Evaluasi Kualitas Clustering (P3)

Untuk mengevaluasi hasil K-Means, kami menggunakan beberapa **metrik clustering** yang umum digunakan, antara lain:
- **Mutual Information Score**, **Adjusted Mutual Information**, dan **Normalized Mutual Information**.
- **Rand Score** dan **Adjusted Rand Score**.
- **Fowlkes–Mallows Score**.
- **Homogeneity Score** dan **V-Measure Score**.

Dengan kombinasi metrik ini, kami dapat menilai:
- Seberapa baik cluster yang dihasilkan **selaras** dengan label referensi sederhana yang diturunkan dari fitur,
- Dan seberapa konsisten pemisahan pola harga pada masing-masing varian dataset.

---
## Slide 26 – Hasil Clustering pada Tiga Dataset (P3)

Dari hasil perhitungan metrik, kami menyusun sebuah tabel perbandingan untuk tiga dataset: **Complete**, **Crash**, dan **Moon**.
Hasil utamanya:
- Dataset **Complete** justru memiliki **rata-rata skor clustering tertinggi**.
- Dataset **Crash** dan **Moon** juga memiliki skor yang cukup baik, namun sedikit di bawah Complete.

Temuan ini menarik karena berbeda dengan beberapa studi pada aset lain (misalnya Bitcoin) di mana subset Crash justru sering memberikan skor tertinggi.

---
## Slide 27 – Visualisasi Cluster pada Ruang PTR–PLR (P3)

Untuk membantu interpretasi, kami juga memvisualisasikan hasil clustering pada bidang **PTR vs PLR** untuk masing-masing dataset:
- Setiap titik mewakili satu hari trading,
- Warna menunjukkan cluster yang dihasilkan oleh K-Means,
- Tanda khusus menandai **pusat cluster (centroid)**.

Dari visualisasi ini terlihat bahwa:
- K-Means mampu memisahkan area dengan **PTR tinggi – PLR rendah** dan sebaliknya,
- Pusat cluster pada umumnya bergerak ke wilayah yang konsisten dengan pola **Crash** dan **Moon** yang sudah kita definisikan sebelumnya.

---
## Slide 28 – Ringkasan Tahap Data Preparation & Modeling (P3)

Sebagai penutup untuk tahap Data Preparation dan Modeling:
- Kami membangun **Patterned Dataset** dengan fitur R, TR, LR, PTR, dan PLR, serta memvalidasi bahwa PTR + PLR ≈ 100.
- Kami mengklasifikasikan kondisi pasar menjadi **Crash**, **Moon**, dan **Neutral**, lalu membentuk tiga varian dataset: `df_complete`, `df_crash`, dan `df_moon`.
- Kami menerapkan **K-Means Clustering (K=2)** pada masing-masing dataset, dengan fitur yang sudah di-scaling.
- Kami mengevaluasi hasil clustering menggunakan berbagai metrik dan menemukan bahwa **dataset Complete** memberikan skor rata-rata tertinggi, meskipun Crash dan Moon tetap menunjukkan kualitas cluster yang baik.

Tahapan ini menyiapkan fondasi untuk analisis berikutnya, yaitu bagaimana memanfaatkan pola **Crash ekstrem (Diamond Crash)** dan hasil clustering untuk menghitung serta membandingkan **ROI harian dan bulanan** dalam strategi trading DOGE.
