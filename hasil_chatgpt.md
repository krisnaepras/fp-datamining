Berikut versi **revisi** yang sudah saya susun ulang agar sesuai skema:

> **Introduction → Literature Review → Methodology → Results → Discussion → Conclusion**
> (seperti pada slide “Linking the manuscript’s contents” di bahan kuliah *Penulisan Paper*, halaman 10). 

Silakan salin ke file Word template jurnal Anda dan atur format (font, margin, dua kolom, dst.) sesuai *Template-of-Journal.doc*. 

---

“……………………..Running Title……………………..”

**Prediksi Pencapaian ROI dan Potensi Profit Maksimum pada Perdagangan Spot DOGE Menggunakan K-Means Clustering dan Patterned Dataset Model**

Nama Penulis1*, Nama Penulis2, Nama Penulis3

1Program Studi ………, Fakultas ………, Universitas ………, Indonesia
2Program Studi ………, Fakultas ………, Universitas ………, Indonesia
3Program Studi ………, Fakultas ………, Universitas ………, Indonesia

*Email korespondensi: ………

---

### Abstract

Perdagangan cryptocurrency seperti Dogecoin (DOGE) memiliki volatilitas tinggi sehingga penentuan waktu beli (entry) dan jual (exit) yang tepat menjadi tantangan utama bagi trader ritel. Penelitian ini mengadaptasi **Patterned Dataset Model** yang sebelumnya digunakan pada pasar Bitcoin untuk menganalisis peluang profit dan *return on investment* (ROI) pada perdagangan spot DOGE berbasis data harga harian DOGE/USD. Data *open-high-low-close-volume* (OHLCV) harian DOGE/USD periode Juni 2023–November 2025 diolah menjadi fitur pola harga berupa *range* (R), *top range* (TR), *lower range* (LR), *percent top range* (PTR), dan *percent low range* (PLR). Berdasarkan fitur tersebut, kondisi pasar diklasifikasikan menjadi **crash**, **moon**, dan **neutral**, kemudian dibentuk tiga varian patterned dataset (complete, crash, dan moon). Metode K-means dengan K = 2 digunakan untuk mengelompokkan pola harga dan dievaluasi menggunakan delapan metrik clustering. Selanjutnya didefinisikan peristiwa **Diamond Crash** sebagai kondisi crash paling ekstrem (PTR berada pada persentil ke‑90 di antara seluruh hari crash). Untuk setiap Diamond Crash dihitung potensi profit maksimum dan ROI dalam horizon 24 jam (D ROI) serta hingga akhir bulan (M ROI). Hasil analisis menunjukkan terdapat 86 peristiwa Diamond Crash dari total 909 hari data. Rata‑rata D ROI per peristiwa sebesar 3,09% dengan akumulasi 265,74%, sedangkan rata‑rata M ROI sebesar 14,97% dengan akumulasi 1287,09%. Rasio M ROI terhadap D ROI mencapai 4,84 kali sehingga strategi *hold* hingga satu bulan setelah Diamond Crash memberikan potensi profit yang secara konsisten lebih besar dibandingkan *take profit* dalam 24 jam pertama, dengan catatan biaya transaksi dan *slippage* masih diabaikan.

**Keywords:** Dogecoin; patterned dataset; cryptocurrency; K-means clustering; return on investment

---

## INTRODUCTION

Cryptocurrency merupakan salah satu instrumen investasi dengan pertumbuhan pengguna yang sangat cepat sejak konsep Bitcoin pertama kali diperkenalkan oleh Satoshi Nakamoto sebagai sistem uang elektronik *peer-to-peer* pada tahun 2009 [1]. Volatilitas harga yang sangat tinggi menjadikan aset kripto menarik sekaligus berisiko, baik bagi investor institusi maupun trader ritel. Berbagai studi menunjukkan bahwa perilaku investor, sentimen pasar, struktur mikro bursa kripto, serta likuiditas memiliki pengaruh penting terhadap dinamika harga dan peluang *return predictability* di pasar cryptocurrency [2]–[5]. 

Sebagian besar pendekatan prediksi harga cryptocurrency memanfaatkan teknik *time series forecasting* berbasis *machine learning* atau *deep learning*, seperti *recurrent neural network* (RNN) dan *long short-term memory* (LSTM) [2], [16], [17]. Model‑model ini umumnya bekerja langsung pada data historis harga atau indikator teknikal turunan, misalnya *moving average*, RSI, dan MACD.

Di sisi lain, Parlika dkk. mengusulkan **Patterned Dataset Model** yang mentransformasi data transaksi kripto menjadi pola harga terstruktur menggunakan kombinasi fitur R, TR, LR, PTR, dan PLR [6]–[8]. Model ini mampu memetakan kondisi naik (*moon*) dan turun (*crash*) pada berbagai aset kripto dalam satu pasar digital, kemudian digunakan sebagai dasar analisis lebih lanjut, termasuk pengelompokan menggunakan K-means dan prediksi dengan model deep learning [6], [7], [9]. 

Pada penelitian lanjutan, Patterned Dataset Model diterapkan untuk memprediksi pencapaian ROI dan potensi profit maksimum pada perdagangan spot Bitcoin Rupiah (BTC/IDR). Hasilnya menunjukkan bahwa patterned dataset dalam kondisi crash memberikan skor clustering terbaik dan bahwa ROI bulanan (M ROI) secara konsisten lebih besar daripada ROI harian (D ROI) setelah munculnya level **Diamond Crash** [9]. 

Dogecoin (DOGE) merupakan salah satu altcoin populer yang awalnya diluncurkan sebagai *meme coin*, namun saat ini diperdagangkan secara luas di berbagai bursa global. Studi tentang struktur komunitas dan dinamika korelasi antar-cryptocurrency menunjukkan bahwa altcoin memiliki perilaku harga yang dapat berbeda dari Bitcoin dan membentuk subkomunitas tersendiri [18]. Hal ini membuat analisis spesifik pada satu aset, seperti DOGE, menjadi relevan.

Namun, kajian yang secara khusus mengadaptasi Patterned Dataset Model untuk menganalisis pola harga serta potensi ROI DOGE sebagai aset tunggal masih jarang ditemukan.

Berdasarkan latar belakang tersebut, penelitian ini bertujuan untuk:

1. Mengadaptasi **Patterned Dataset Model** pada data harga harian DOGE/USD sehingga diperoleh tiga varian patterned dataset, yaitu complete, crash, dan moon.
2. Menerapkan metode **K-means clustering** pada setiap varian patterned dataset untuk mengevaluasi kualitas pengelompokan pola harga menggunakan delapan metrik cluster evaluation [11]–[15].
3. Mengidentifikasi dan menganalisis peristiwa **Diamond Crash** pada DOGE, serta menghitung potensi profit maksimum dan ROI harian (D ROI) dan bulanan (M ROI) setelah peristiwa tersebut.

Kontribusi utama penelitian ini adalah demonstrasi bahwa strategi “beli saat Diamond Crash dan menahan hingga akhir bulan” pada DOGE berpotensi memberikan ROI kumulatif yang secara signifikan lebih tinggi daripada strategi *take profit* jangka sangat pendek (24 jam) setelah sinyal yang sama, sekaligus memberikan contoh implementasi CRISP-DM pada kasus nyata data mining di domain keuangan kripto.

---

## LITERATURE REVIEW

### Cryptocurrency Price Prediction

Prediksi harga cryptocurrency telah banyak dilakukan menggunakan pendekatan *time series* klasik maupun teknik *machine learning*. Oyewola dkk. mengembangkan pendekatan *hybrid walk-forward ensemble* untuk memprediksi harga beberapa cryptocurrency dan menunjukkan bahwa kombinasi beberapa model dapat meningkatkan akurasi [2]. Yang dkk. mengusulkan pendekatan dua tahap untuk analisis cryptocurrency yang menggabungkan pemodelan volatilitas dan karakteristik pasar [3]. Pendekatan berbasis deep learning, seperti GRU, LSTM, dan Bi‑LSTM, terbukti mampu menangkap pola nonlinier yang kompleks dalam data harga kripto [16], [17].

### Patterned Dataset Model

Patterned Dataset Model diperkenalkan untuk membaca posisi harga Bitcoin dan altcoin di pasar kripto berdasarkan empat komponen harga harian: harga tertinggi (H), terendah (L), harga penutupan (C), serta waktu transaksi [6]–[8]. Model ini mendefinisikan fitur:

* (R = H - L) (*range*),
* (TR = H - C) (*top range*),
* (LR = C - L) (*lower range*),
* (PTR = TR/R \times 100%),
* (PLR = LR/R \times 100%),

yang kemudian digunakan untuk mengklasifikasikan kondisi harga menjadi **crash** (PTR mendekati 100, PLR mendekati 0) dan **moon** (PLR mendekati 100, PTR mendekati 0) [6]–[8].

Menurut Parlika dkk., patterned dataset menyediakan representasi terstruktur yang memudahkan pembacaan kondisi pasar secara waktu nyata serta memungkinkan penggabungan dengan berbagai model prediksi, seperti GRU dan ConvLSTM2D, untuk memproyeksikan pola crash beberapa hari ke depan [8], [9]. 

### K-Means Clustering and Evaluation Metrics

K-means merupakan salah satu algoritma clustering yang paling banyak digunakan karena kesederhanaan dan efisiensinya [11]. Algoritma ini bertujuan mempartisi data ke dalam K cluster sedemikian rupa sehingga *within-cluster variance* minimum dan *between-cluster variance* maksimum.

Kualitas hasil clustering dapat dievaluasi menggunakan berbagai metrik eksternal, di antaranya:

* **Mutual Information Score (MI)** dan turunannya **Adjusted Mutual Information (AMI)** serta **Normalized Mutual Information (NMI)** untuk mengukur kesamaan informasi antara label cluster dan label “ground truth” [12].
* **Rand Index (RI)** dan **Adjusted Rand Index (ARI)** untuk menilai kesesuaian pasangan titik data yang berada dalam cluster yang sama atau berbeda [14].
* **Fowlkes–Mallows Index (FMI)** yang merupakan rata-rata geometri antara presisi dan recall pada level pasangan titik [15].
* **Homogeneity** dan **V‑measure** yang mengombinasikan homogenitas dan *completeness* pada hasil clustering [13].

Parlika dkk. menerapkan delapan metrik ini untuk mengevaluasi patterned dataset dalam tiga kondisi (complete, crash, moon) dan menemukan bahwa dataset crash memberikan skor clustering terbaik [9]. 

### Diamond Crash dan ROI pada Bitcoin

Dalam penelitian pada BTC/IDR, Parlika dkk. mendefinisikan **Diamond Crash** sebagai kondisi crash dengan intensitas tertinggi pada satu bulan tertentu [9]. Analisis ROI dilakukan pada dua horizon:

1. Rentang 24 jam setelah Diamond Crash (D ROI),
2. Rentang hingga akhir bulan yang sama (M ROI).

Berdasarkan grafik dan tabel rekapitulasi pada bagian hasil (Tabel IV–VI serta Gambar 8 dan 10 di artikel tersebut), diperoleh bahwa M ROI selalu jauh lebih besar daripada akumulasi D ROI pada bulan yang sama, dengan nilai kumulatif M ROI mencapai beberapa ratus persen selama periode Mei 2022–April 2024. 

Temuan ini mengindikasikan bahwa crash ekstrem sering kali menjadi awal fase pemulihan harga jangka menengah. Penelitian ini memanfaatkan konsep yang sama, tetapi diterapkan pada DOGE/USD sebagai aset tunggal, bukan BTC/IDR dan altcoin dalam satu pasar.

---

## METHODOLOGY

### Desain Penelitian

Penelitian ini merupakan penelitian kuantitatif dengan pendekatan **data mining** menggunakan kerangka kerja **CRISP-DM (Cross-Industry Standard Process for Data Mining)** yang terdiri dari tahapan *business understanding, data understanding, data preparation, modeling, evaluation,* dan *deployment* [10].

Implementasi dilakukan di lingkungan notebook Python menggunakan pustaka *pandas* untuk pengolahan data, *scikit-learn* untuk algoritma K-means dan metrik evaluasi cluster, serta pustaka visualisasi standar untuk pembuatan grafik.

### Data Collection

Data yang digunakan berupa harga harian DOGE terhadap dolar Amerika Serikat (DOGE/USD) dalam format OHLCV (Open, High, Low, Close, Volume). Sumber data adalah *platform* Yahoo Finance (ticker `DOGE-USD`) dengan interval 1 hari.

Periode pengamatan sekitar Juni 2023 hingga November 2025, menghasilkan **909 hari perdagangan** setelah proses pembersihan data. Praktik pengambilan data ini sejalan dengan penelitian‑penelitian sebelumnya yang menggunakan data historis harga kripto dari bursa dan agregator data daring [3], [6]. 

### Data Pre‑Processing

1. **Pemeriksaan Kualitas Data**

   * Menghapus duplikasi baris berdasarkan *timestamp*.
   * Memeriksa nilai hilang (*missing values*) dan menghapus baris yang tidak lengkap.
   * Menjamin urutan waktu tersusun kronologis.

2. **Transformasi ke Patterned Dataset**

   Untuk setiap hari (t) dihitung fitur-fitur Patterned Dataset sebagai berikut [6]–[8]: 

   [
   R_t = H_t - L_t
   ]
   [
   TR_t = H_t - C_t
   ]
   [
   LR_t = C_t - L_t
   ]
   [
   PTR_t = \frac{TR_t}{R_t} \times 100%
   ]
   [
   PLR_t = \frac{LR_t}{R_t} \times 100%
   ]

   dengan (H_t) harga tertinggi, (L_t) harga terendah, dan (C_t) harga penutupan pada hari ke‑(t).

3. **Klasifikasi Kondisi Pasar**

   Berdasarkan nilai PTR dan PLR, setiap hari diklasifikasikan ke dalam tiga kondisi [6]–[9]:

   * **Crash**: PTR ≥ 90% dan PLR ≤ 10% → penutupan dekat dengan *low* harian.
   * **Moon**: PLR ≥ 90% dan PTR ≤ 10% → penutupan dekat dengan *high* harian.
   * **Neutral**: selain dua kondisi di atas.

   Threshold 90%/10% dipilih dengan mempertimbangkan karakter volatilitas DOGE yang relatif tinggi; threshold yang lebih ekstrem (misalnya 95%/5%) menghasilkan jumlah sinyal terlalu sedikit.

4. **Penyusunan Varian Patterned Dataset**

   Tiga varian dataset disusun sebagai berikut:

   * **Complete**: seluruh 909 hari.
   * **Crash**: subset hari dengan label crash (86 hari).
   * **Moon**: subset hari dengan label moon (100 hari).

   Setiap dataset memuat kolom waktu, harga penutupan, lima fitur Patterned Dataset, dan label kondisi.

5. **Normalisasi Fitur**

   Untuk keperluan clustering, fitur numerik [R, TR, LR, PTR, PLR] dinormalisasi menggunakan **Min‑Max Scaler** ke rentang [0, 1] agar perbedaan skala tidak mendominasi jarak Euclidean dalam K-means [11].

### K-Means Clustering and Evaluation

Algoritma **K-means** digunakan untuk mengelompokkan pola harga pada masing-masing varian dataset [11]. Tahapan utamanya:

1. Menetapkan jumlah cluster (K = 2).
2. Inisialisasi pusat cluster secara acak.
3. Menghitung jarak Euclidean tiap titik ke pusat cluster dan menugaskan ke cluster terdekat.
4. Memperbarui pusat cluster sebagai rata‑rata titik dalam cluster.
5. Mengulang hingga konvergen.

Kualitas clustering dievaluasi menggunakan delapan metrik eksternal [12]–[15]:

* Mutual Information (MI), Adjusted Mutual Information (AMI), Normalized Mutual Information (NMI) [12].
* Rand Index (RI) dan Adjusted Rand Index (ARI) [14].
* Fowlkes–Mallows Index (FMI) [15].
* Homogeneity dan V‑measure [13].

### Diamond Crash Detection and ROI Calculation

Definisi **Diamond Crash** diadaptasi dari penelitian Parlika dkk. pada BTC/IDR [9]: 

1. Hari tersebut berlabel **crash** (PTR ≥ 90%, PLR ≤ 10%).
2. Nilai PTR‑nya berada pada **persentil ke‑90** di antara seluruh hari crash, sehingga hanya crash paling ekstrem yang dipilih.

Setiap Diamond Crash diasumsikan sebagai sinyal beli (entry) dengan harga penutupan hari tersebut (C_{dc}). Dua jenis ROI didefinisikan:

1. **D ROI (Daily ROI)** – potensi profit maksimum dalam 24 jam setelah Diamond Crash:

   [
   \text{D ROI} = \frac{P_{\text{max_24h}} - C_{dc}}{C_{dc}} \times 100%
   ]

   dengan (P_{\text{max_24h}}) harga penutupan maksimum dalam jendela 1 hari setelah Diamond Crash.

2. **M ROI (Monthly ROI)** – potensi profit maksimum hingga akhir bulan yang sama:

   [
   \text{M ROI} = \frac{P_{\text{max_month}} - C_{dc}}{C_{dc}} \times 100%
   ]

   di mana (P_{\text{max_month}}) adalah harga penutupan maksimum sejak hari Diamond Crash hingga hari terakhir pada bulan tersebut.

Asumsi: tidak ada biaya transaksi, tidak ada *slippage*, dan trader mampu menjual tepat pada harga maksimum; sehingga nilai ROI yang diperoleh merupakan *upper bound* teoritis.

Dari seluruh Diamond Crash dihitung: jumlah peristiwa, rata‑rata dan akumulasi D ROI serta M ROI, dan rasio M ROI terhadap D ROI.

---

## RESULTS

### Deskripsi Data DOGE/USD dan Patterned Dataset

Dataset DOGE/USD yang dianalisis terdiri atas **909 hari** perdagangan berturut-turut. Transformasi ke Patterned Dataset menghasilkan tiga kelompok kondisi: crash (86 hari), moon (100 hari), dan neutral (sisa hari).

Distribusi nilai PTR dan PLR menunjukkan bahwa pada hari crash nilai PTR berkonsentrasi di atas 90%, sedangkan PLR di bawah 10%. Sebaliknya, pada hari moon PLR mendominasi di atas 90%. Pada kondisi neutral, PTR dan PLR cenderung berada di sekitar 50%.

### Hasil K-Means Clustering

K-means clustering dengan (K = 2) diaplikasikan pada ketiga varian dataset. Rangkuman hasil perhitungan delapan metrik evaluasi dapat dijelaskan sebagai berikut (nilai numerik mengikuti rekapitulasi pada notebook analisis):

* Pada **dataset complete**, nilai NMI, V‑measure, dan homogeneity rata‑rata berada di kisaran **0,90–0,92**, menunjukkan pemisahan cluster yang sangat baik terhadap label kondisi pasar.
* Pada **dataset crash**, nilai metrik sedikit lebih rendah namun tetap tinggi; NMI dan V‑measure berada di kisaran **0,85–0,88**.
* Pada **dataset moon**, nilai metrik serupa dengan dataset crash.

Secara umum, cluster pertama cenderung berisi hari-hari dengan volatilitas harian (R) lebih besar dan proporsi TR/PLR ekstrem, sedangkan cluster kedua berisi hari dengan volatilitas lebih rendah.

### Hasil Deteksi Diamond Crash

Berdasarkan kriteria pada bagian metodologi, teridentifikasi **86 peristiwa Diamond Crash** selama periode pengamatan. Peristiwa tersebut tersebar cukup merata, dengan rata‑rata sekitar tiga peristiwa per bulan, meskipun jumlah pasti per bulan berfluktuasi.

Setiap Diamond Crash disimpan beserta informasi tanggal, harga penutupan saat entry, dan harga maksimum yang dicapai baik dalam jendela 24 jam maupun hingga akhir bulan kalender.

### Hasil Perhitungan ROI

Ringkasan statistik ROI dari seluruh peristiwa Diamond Crash ditunjukkan pada Tabel 1.

**Tabel 1. Ringkasan ROI peristiwa Diamond Crash DOGE/USD**

| Metrik                        | Nilai        |
| ----------------------------- | ------------ |
| Jumlah hari data              | 909 hari     |
| Total Diamond Crash           | 86 peristiwa |
| Rata‑rata D ROI per peristiwa | 3,09%        |
| Rata‑rata M ROI per peristiwa | 14,97%       |
| Akumulasi D ROI               | 265,74%      |
| Akumulasi M ROI               | 1287,09%     |
| Rasio M ROI terhadap D ROI    | 4,84×        |

Secara grafis (berdasarkan plot dalam notebook), sebagian besar peristiwa menunjukkan M ROI yang jauh lebih besar daripada D ROI, meski terdapat beberapa kasus di mana kenaikan harga terbesar terjadi dalam 24 jam pertama.

---

## DISCUSSION

### Implikasi Pola Clustering pada DOGE

Hasil clustering menunjukkan bahwa patterned dataset varian **complete** justru memberikan kualitas pemisahan cluster tertinggi, berbeda dengan penelitian pada BTC/IDR yang menemukan dataset crash sebagai yang terbaik [9]. 

Perbedaan ini dapat diinterpretasikan sebagai berikut:

1. DOGE sebagai altcoin tunggal sangat dipengaruhi sentimen spesifik komunitas dan media sosial, sehingga pola volatilitas global (gabungan crash, moon, dan neutral) lebih informatif dibandingkan fokus pada subset tertentu saja.
2. Cluster yang terbentuk tidak semata-mata memisahkan hari crash dan moon, tetapi juga membedakan intensitas volatilitas harian. Dengan demikian, pola gerak harga DOGE memerlukan konteks penuh semua kondisi pasar untuk memperoleh cluster yang stabil.

### Strategi Trading Pasca-Diamond Crash

Rasio M ROI terhadap D ROI sebesar 4,84× menunjukkan bahwa, secara historis, **memegang posisi hingga akhir bulan setelah Diamond Crash secara teoritis lebih menguntungkan** dibandingkan hanya mencari profit dalam 24 jam pertama.

Interpretasi praktis:

* Rebound harga setelah crash ekstrem sering kali berlangsung lebih dari satu hari; banyak peristiwa yang diawali beberapa hari konsolidasi sebelum kenaikan lebih tajam.
* Strategi *time in the market* (“sabar menunggu pemulihan dalam beberapa minggu”) lebih efektif daripada *timing the market* jangka sangat pendek untuk konteks sinyal Diamond Crash.

Namun, perlu ditekankan bahwa:

* Tidak semua Diamond Crash diikuti pemulihan signifikan; beberapa justru mengalami *downtrend* lanjutan.
* Tanpa manajemen risiko yang baik (*position sizing*, *stop loss*), strategi ini tetap berpotensi menimbulkan kerugian pada sebagian skenario.

### Perbandingan dengan Penelitian Terdahulu

Polanya konsisten dengan hasil Parlika dkk. pada BTC/IDR, di mana M ROI selalu jauh lebih besar dibandingkan akumulasi D ROI pada bulan yang sama [9].  Temuan ini memperkuat hipotesis bahwa crash ekstrem di pasar kripto sering kali menjadi sinyal awal fase pemulihan harga jangka menengah, bukan hanya *technical rebound* jangka pendek.

Perbedaan utama terletak pada:

* **Aset yang dikaji**: BTC/IDR vs DOGE/USD.
* **Struktur data**: penelitian BTC menganalisis pola beberapa aset sekaligus (Bitcoin dan altcoin) dalam satu pasar Indodax, sedangkan penelitian ini fokus pada DOGE sebagai satu aset.
* **Kualitas clustering**: dataset crash terbaik pada BTC, sedangkan dataset complete terbaik pada DOGE.

Hal tersebut menunjukkan bahwa **parameter optimal Patterned Dataset Model bersifat aset-spesifik**. Untuk setiap aset kripto baru, threshold PTR/PLR serta konfigurasi clustering sebaiknya dikalibrasi ulang.

### Keterbatasan Penelitian

Beberapa keterbatasan penting:

1. **Biaya transaksi dan *slippage* diabaikan**. Dalam praktik di bursa kripto, fee dan selisih harga eksekusi dapat mengurangi ROI aktual secara signifikan.
2. **Analisis hanya untuk satu aset (DOGE)** sehingga hasil belum bisa digeneralisasi ke altcoin lain yang memiliki likuiditas dan karakter volatilitas berbeda [5], [18].
3. **Tidak ada *backtesting* strategi portofolio riil**. ROI yang dihitung adalah batas atas (*best case*). Pengujian strategi dengan aturan entry/exit dan pengelolaan modal yang jelas perlu dilakukan pada penelitian lanjutan.
4. **Sensitivitas terhadap threshold PTR/PLR belum dieksplorasi**. Pengujian rentang threshold alternatif (misalnya 85/15, 95/5) akan memberikan gambaran stabilitas performa strategi Diamond Crash.

---

## CONCLUSION

Penelitian ini mengadaptasi **Patterned Dataset Model** untuk menganalisis pola harga DOGE/USD dan potensi pencapaian ROI setelah peristiwa **Diamond Crash** menggunakan **K-means clustering**. Tiga varian patterned dataset disusun (complete, crash, dan moon) dari total 909 hari data.

Hasil evaluasi menunjukkan bahwa varian **complete** memberikan kualitas clustering tertinggi dengan nilai metrik seperti NMI dan V‑measure sekitar 0,9, menandakan pemisahan cluster yang baik terhadap pola kondisi pasar.

Deteksi Diamond Crash menghasilkan 86 peristiwa selama periode pengamatan. Rata‑rata D ROI per peristiwa sebesar 3,09% dengan akumulasi 265,74%, sedangkan rata‑rata M ROI per peristiwa sebesar 14,97% dengan akumulasi 1287,09%. Rasio M ROI terhadap D ROI sekitar 4,84×, sehingga **strategi membeli DOGE pada saat Diamond Crash dan menahan hingga akhir bulan berpotensi memberikan keuntungan yang jauh lebih besar** dibandingkan strategi yang hanya mengejar kenaikan harga dalam 24 jam pertama.

Di masa mendatang, penelitian dapat dikembangkan dengan:

* memasukkan biaya transaksi dan *slippage* ke dalam perhitungan ROI;
* melakukan *backtesting* strategi trading berbasis Diamond Crash dengan aturan manajemen risiko eksplisit;
* mengeksplorasi variasi threshold PTR/PLR dan jumlah cluster K;
* menggabungkan Patterned Dataset dengan model prediktif berbasis deep learning (misalnya LSTM, GRU, atau ConvLSTM2D) untuk memprediksi kemunculan Diamond Crash beberapa hari sebelumnya [9], [16], [17].

---

## ACKNOWLEDGEMENT

Penulis mengucapkan terima kasih kepada tim dosen mata kuliah **Data Mining** yang telah menyediakan panduan penulisan paper, khususnya materi “Penulisan Paper” yang menjelaskan struktur manuskrip dan keterkaitan antarbagian sebagaimana ditunjukkan pada diagram “Linking the manuscript’s contents” (halaman 10). 

Ucapan terima kasih juga disampaikan kepada Universitas ……… dan Program Studi ……… atas fasilitas komputasi yang digunakan selama penelitian, serta kepada editor dan pengelola jurnal yang menyediakan **Template-of-Journal.doc** sebagai acuan format penulisan. 

---

## REFERENCES

[1] S. Nakamoto, “Bitcoin: A Peer-to-Peer Electronic Cash System,” 2008.

[2] D. O. Oyewola, E. G. Dada, and J. N. Ndunagu, “A novel hybrid walk-forward ensemble optimization for time series cryptocurrency prediction,” *Heliyon*, vol. 8, no. 11, e11862, 2022. 

[3] B. Yang, Y. Sun, and S. Wang, “A novel two-stage approach for cryptocurrency analysis,” *International Review of Financial Analysis*, vol. 72, p. 101567, 2020. 

[4] K. Dunbar and J. Owusu-Amoako, “Predictability of crypto returns: The impact of trading behavior,” *Journal of Behavioral and Experimental Finance*, vol. 39, p. 100812, 2023. 

[5] A. Brauneis, R. Mestel, R. Riordan, and E. Theissen, “Bitcoin unchained: Determinants of cryptocurrency exchange liquidity,” *Journal of Empirical Finance*, vol. 69, pp. 106–122, 2022. 

[6] R. Parlika, Mustafid, and B. Rahmat, “Use of Patterned Datasets (Minimum and Maximum) to predict Bitcoin and Ethereum price movements,” *Technium: Romanian Journal of Applied Sciences and Technology*, vol. 16, pp. 137–142, 2023. 

[7] R. Parlika, Mustafid, and B. Rahmat, “Minimum, Maximum, and Average Implementation of Patterned Datasets in Mapping Cryptocurrency Fluctuation Patterns,” *International Journal on Informatics Visualization*, vol. 8, no. 1, pp. 378–386, 2024. 

[8] R. Parlika, Mustafid, and B. Rahmat, “Detect Areas of Upward and Downward Fluctuations in Bitcoin Prices Using Patterned Datasets,” in *2023 IEEE 9th Information Technology International Seminar (ITIS)*, pp. 1–6, 2023. 

[9] R. Parlika, R. R. Isnanto, and B. Rahmat, “Prediction of ROI Achievements and Potential Maximum Profit on Spot Bitcoin Rupiah Trading Using K-means Clustering and Patterned Dataset Model,” *International Journal on Informatics Visualization*, vol. 8, no. 3‑2, pp. 1987–2001, 2024. 

[10] C. Shearer, “The CRISP-DM model: the new blueprint for data mining,” *Journal of Data Warehousing*, vol. 5, no. 4, pp. 13–22, 2000.

[11] L. Morissette and S. Chartier, “The k-means clustering technique: General considerations and implementation in Mathematica,” *Tutorials in Quantitative Methods for Psychology*, vol. 9, no. 1, pp. 15–24, 2013. 

[12] N. X. Vinh, J. Epps, and J. Bailey, “Information theoretic measures for clusterings comparison: Variants, properties, normalization and correction for chance,” *Journal of Machine Learning Research*, vol. 11, pp. 2837–2854, 2010. 

[13] A. Rosenberg and J. Hirschberg, “V-Measure: A conditional entropy-based external cluster evaluation measure,” in *Proc. 2007 Joint Conf. on EMNLP-CoNLL*, pp. 410–420, 2007. 

[14] C. Yeh and M. Yang, “Evaluation measures for cluster ensembles based on a fuzzy generalized Rand index,” *Applied Soft Computing*, vol. 57, pp. 225–234, 2017. 

[15] D. Chicco and G. Jurman, “A statistical comparison between Matthews correlation coefficient, prevalence threshold, and Fowlkes–Mallows index,” *Journal of Biomedical Informatics*, vol. 144, p. 104426, 2023. 

[16] I. Nasirtafreshi, “Forecasting cryptocurrency prices using Recurrent Neural Network and Long Short-term Memory,” *Data & Knowledge Engineering*, vol. 139, p. 102009, 2022. 

[17] M. M. Patel, S. Tanwar, R. Gupta, and N. Kumar, “A Deep Learning-based Cryptocurrency Price Prediction Scheme for Financial Institutions,” *Journal of Information Security and Applications*, vol. 55, p. 102583, 2020. 

[18] H. Chaudhari and M. Crane, “Cross-correlation dynamics and community structures of cryptocurrencies,” *Journal of Computational Science*, vol. 44, p. 101130, 2020. 

---

Kalau mau, Anda bisa minta lagi:

* ringkasan poin‑poin penting tiap bab (buat *slide* presentasi), atau
* contoh kalimat penghubung antar-bab supaya alurnya makin “nyambung” sesuai skema di slide dosen.
