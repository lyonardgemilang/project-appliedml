# Laporan Proyek Machine Learning - Nama Anda

## Project Overview

Platform streaming film menghadapi tantangan dalam memberikan rekomendasi yang sesuai bagi penggunanya. Dengan banyaknya film yang baru saat ini, banyak pengguna yang kebingungan dalam memilih film. Per 2018 angka jumlah penonton bioskop di Indonesia saja telah mencapai lebih dari 50 juta penonton dengan jumlah produksi film luar negeri hingga dalam negeri sebanyak hampir 200 judul film yang telah tayang di seluruh Indonesia (Fajriansyah et al., 2021). Memilih film memakan sangat banyak waktu, yang tentunya hal itu bahkan dapat membuat pengguna tidak jadi menonton. Hal itu tidak hanya merugikan penonton, tetapi perusahaan juga. 

### Alasan
Masalah ini harus diatasi oleh platform streaming film agar pengguna tetap menggunakan platoform tersebut. Oleh karena itu, platform streaming film memerlukan sistem rekomendasi. Sistem  rekomendasi  merupakansistem  perangkat  lunak  yang  dapat  membuat rekomendasi  ataupun  membuat  saran  akan  item-item  yang  sesuai  kepada  pengguna,  item adalah  istilah  umum  yang  digunakan  untuk  menunjukkan  apa  yang  direkomendasikan sistem  kepada  pengguna  (Wiputra & Shandi, 2021). Dengan dibuat sistem rekomendasi, pengguna dapat memilih film yang ingin ditonton selanjutnya yang relevan dengan preferensinya. Sistem rekomendasi dapat dibuat dengan menggunakan pendekatan Content-based filtering dan juga Collaborative Filtering. Content Based Filtering menggunakan kemiripan antar produk yang akan direkomendasikan dengan produk yang disukai pengguna dan Collaborative Filtering menggunakan kemiripan kueri dengan item pengguna dengan pengguna lain (Fajriansyah et al., 2021).

### Referensi:
Fajriansyah, Muhammad, et al. “Sistem Rekomendasi Film Menggunakan Content Based Filtering.” Jurnal Pengembangan Teknologi Informasi dan Ilmu Komputer, vol. 5, no. 6, 2021, http://j-ptiik.ub.ac.id/.
Wiputra, Michael Mahendra, and Yusup Jauhari Shandi. “PERANCANGAN SISTEM REKOMENDASI MENGGUNAKAN METODE COLLABORATIVE FILTERING DENGAN STUDI KASUS PERANCANGAN WEBSITE REKOMENDASI FILM.” Media Informatika, vol. 20, no. 1, 2021, https://journal.likmi.ac.id/index.php/media-informatika/article/view/54/48.

## Business Understanding

### Problem Statements
- Pendekatan rekomendasi apa yang paling sesuai untuk kondisi dengan atau tanpa informasi pengguna?
- Bagaimana membangun sistem rekomendasi film yang dapat memberikan saran tontonan yang relevan untuk pengguna berdasarkan metadata film atau histori rating pengguna?
- Bagaimana mengevaluasi performa model berdasarkan karakteristik dan keluaran masing-masing?

### Goals
- Mengembangkan dua pendekatan sistem rekomendasi, yaitu content-based filtering dan collaborative filtering.
- Menunjukkan efektivitas setiap pendekatan berdasarkan contoh hasil rekomendasi.
- Memberikan rekomendasi film yang sesuai dengan preferensi pengguna dan kesamaan konten film.

### Solution statements
- Membangun model Content-Based Filtering dengan TF-IDF dan cosine similarity berdasarkan kolom genres untuk digunakan saat tidak ada data historis pengguna.
- Membangun model Collaborative Filtering berbasis neural network untuk mempelajari preferensi pengguna dengan evaluasi mengguanakan Root Mean Square Error (RMSE).
- Menggunakan contoh hasil rekomendasi aktual untuk membandingkan efektivitas kedua pendekatan.

## Data Understanding
Data yang digunakan dalam proyek ini adalah "MovieLens Beliefs Dataset" yang berisi rekomendasi pengguna, rating, dan perkiraan rating yang diberikan user untuk film yang belum pernah ditonton. Dataset yang digunakan adalah movies.csv dan user_rating_history.csv.

### Dataset
Dataset: <a href="https://grouplens.org/datasets/movielens/ml_belief_2024/">MovieLens Beliefs Dataset</a> <br>

#### movies.csv
- Jumlah data: 105071 baris dan 3 kolom.
- Tidak ditemukan duplikat data berdasarkan hasil pemeriksaan df.duplicated().sum()
- Terdapat missing value di kolom genres. <br>

Variabel-variabel pada movies dataset adalah sebagai berikut:
- movieId: ID untuk film
- title: Judul film
- genres: Genre film yang terdiri dari Action, Adventure, Animation, Children's, Comedy, Crime, Documentary, Drama, Fantasy, Film-Noir, Horror, Musical, Mystery, Romance, Sci-Fi, Thriller, War, dan Western. <br>

Tipe Data Setiap Variabel
- movieId: numerik (int64)
- title, genres: string (object)

#### user_rating_history.csv
- Jumlah data: 2046124 baris dan 4 kolom.
- Terdapat data duplikat.
- Terdapat missing value pada kolom rating. <br>

Variabel-variabel pada user_rating_history dataset adalah sebagai berikut:
- userId: ID pengguna
- movieId: ID film
- rating: Penilaian pengguna terhadap film
- tstamp: Waktu pengiriman rating

Tipe Data Setiap Variabel
- userId, movieId: numerik (int64)
- rating: numerik (float64)
- tstamp: string (object)

### Eksplorasi Data
- Top 20 Film dengan Rating Terbanyak <br>
![Top 20 Movies](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/top_20.png) <br>
Gambar di atas merupakan bar chart mengenai top 20 film dengan rating terbanyak. Film yang memiliki rating terbanyak adalah The Shawshank Redemption. Dari top 20 film ini, rata-rata film memiliki rating di atas 3.5.
- Rating Film <br>
![Average Rating by Genre](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/avg_rating.png) <br>
Gambar di atas merupakan bar chart mengenai rata-rata rating setiap genre. Genre yang paling banyak diminati orang adalah film dengan genre Film-Noir.

## Data Preparation
Dalam pengerjaan proyek ini diterapkan beberapa teknik data preparation untuk kedua dataset yang dipakai.

### movies.csv
- Menghapus data missing value (dropna())
- Menghilangkan simbol "|" pada kolom genres sehingga setiap genre dipisah dengan menggunakan spasi saja
- Data diproses menggunakan TfidVectorizer <br>


**Alasan Tahapan Data Preparation**
- 
- <br>

### user_rating_history.csv
- Menghapus data missing value (dropna())
- Menghapus data duplikat (drop_duplicates())

**Alasan Tahapan Data Preparation**
- 
- <br>

## Modeling
Terdapat dua model yang digunakan dalam proyek ini, yaitu Content-based filtering (top-N) dan Collaborative Filtering (RecommenderNet).

### Content-based filtering
Random Forest adalah algoritma ensemble yang membangun banyak pohon keputusan (decision tree) selama proses training dan menggabungkan prediksi dari semua pohon untuk menentukan hasil akhir. Model ini bekerja dengan prinsip voting untuk klasifikasi dan rata-rata untuk regresi. Setiap pohon dilatih dengan subset data dan subset fitur yang dipilih secara acak.

#### Parameter yang Digunakan dalam Proses Development
Parameter yang dipakai adalah default.
- n_estimators=100: jumlah pohon yang dibuat
- criterion='gini': fungsi untuk mengukur kualitas split
- max_depth=None: pohon akan berkembang sampai semua daun sempurna
- min_samples_split=2: jumlah minimum sampel untuk membagi internal node
- min_samples_leaf=1: jumlah minimum sampel di daun
- max_features='sqrt': jumlah fitur yang dipertimbangkan pada tiap split
- max_leaf_nodes=None: Tidak ada batasan jumlah daun
- min_impurity_decrease=0.0: Minimum pengurangan impuritas untuk membagi node
- bootstrap=True: menggunakan sampling bootstrap
- oob_score=False: Tidak menghitung out-of-bag score
- n_jobs=None: Proses dijalankan dalam satu core
- random_state=None: tidak ada seed acak yang disetel
- verbose=0: Tidak ada logging
- warm_start=False: Tidak mempertahankan model sebelumnya
- class_weight=None: Tidak menyesuaikan bobot kelas
- ccp_alpha=0.0: Complexity pruning tidak digunakan
- max_samples=None: Menggunakan seluruh sampel

#### Kelebihan
- Cocok untuk data tabular dan kompleks
- Dapat menangani outlier dan missing dengan lebih stabil 

#### Kekurangan
- Interpretasi sulit
- Memerlukan komputasi lebih besar saat tuning

#### Tuning yang Diterapkan
Parameter yang dipilih untuk dituning adalah n_estimators, max_depth, min_samples_split, dan max_features. Keempat parameter ini dipilih karena parameter-parameter ini mengontrol kompleksitas model dan mampu membuat model menggeneralisasi dengan baik. Dilakukan tuning menggunakan RandomizedCV untuk mencari hyperparameter optimal dengan value setiap parameter sebagai berikut:
- n_estimators: Meningkatkan jumlah pohon diuji pada [50, 100, 150] untuk mengecek stabilitas prediksi agar tidak overfitting.
- max_depth: [None, 5, 10, 15] digunakan untuk membatasi overfitting.
- min_samples_split: [2, 5, 10] untuk mengatur kapan node dapat di-split lebih lanjut. Nilai lebih tinggi mengurangi overfitting.
- max_features: ['sqrt', 'log2'] menentukan jumlah maksimum fitur yang digunakan untuk mencari split terbaik di setiap node. sqrt cocok untuk dataset dengan banyak fitur dan log2 lebih konservatif yang dapat membantu generalisasi.

### Collaborative Filtering

#### Parameter yang Digunakan dalam Proses Development

#### Kelebihan
- 
- 

#### Kekurangan
- 

#### Tuning yang Diterapkan:



Dengan menggunakan RandomizedCV untuk mencari hyperparameter optimal secara acak, didapat hyperparameter terbaik seperti gambar di bawah ini: <br>
![Tuning Param](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/tuning.png) <br>

Dari 2 model yang sudah dibuat, model yang dipilih untuk klasifikasi kualitas udara adalah Random Forest. Random Forest menghasilkan akurasi dan F1 Score lebih tinggi dibandingkan Logistic Regression.

## Evaluation

### Content Based Filtering
![Evaluation ConBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/eval_conbased.png) 

### Collaborative Based Filtering
![Evaluation ColBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/rmse.png) 

Metrik yang digunakan untuk mengevaluasi model, yaitu:
- Akurasi
- F1 Score

### Akurasi
Akurasi mengukur proporsi prediksi yang benar dari keseluruhan jumlah prediksi, baik yang benar positif (True Positive/TP) maupun benar negatif (True Negative/TN). Berikut merupakan rumus akurasi: <br>
$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$ <br>
- TP (True Positive): Prediksi benar untuk kelas positif
- TN (True Negative): Prediksi benar untuk kelas negatif
- FP (False Positive): Prediksi salah, model memprediksi positif yang seharusnya negatif
- FN (False Negative): Prediksi salah, model memprediksi negatif yang seharusnya positif

### F1-Score
F1 Score adalah rata-rata harmonis dari Precision dan Recall. F1 Score sangat penting untuk kasus dengan distribusi kelas tidak seimbang karena mempertimbangkan baik prediksi positif yang tepat maupun semua prediksi positif yang seharusnya. Berikut merupakan rumus F1-Score: <br>
$F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}$ <br>
dengan rumus precision: <br>
$Precision = \frac{TP}{TP + FP}$ <br>
dan rumus Recall: <br>
$Recall = \frac{TP}{TP + FN}$

### Evaluasi Sebelum Tuning
RF Accuracy: 0.9920634920634921 <br>
RF F1 Score: 0.9916796348168897 <br>
![CM RF](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMRF_BT.png) <br>
Akurasi model Random Forest pada data test adalah sebesar 0.99 begitu juga dengan F1-score. Dilihat pada gambar metrik di atas, model ini hanya memiliki 2 kesalahan prediksi, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1, dan label 2 yang diprediksi sebagai 1 sebanyak 1. Namun, perlu dicari tahu apakah model ini overfitting atau tidak. Salah satu caranya, yaitu dengan membandingkan akurasi test dengan train seperti di bawah ini: <br>
RF Train Accuracy: 1.0 <br>
RF Test Accuracy: 0.9920634920634921 <br>
RF Train F1 Score: 1.0 <br>
RF Test F1 Score: 0.9916796348168897 <br>
Ternyata model tersebut mengalami overfitting karena akurasi train lebih besar dibandingkan akurasi tes begitu juga dengan F1-scorenya. <br>

LR Accuracy: 0.9761904761904762 <br>
LR F1 Score: 0.9757963693965064 <br>
![CM LR](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMLR_BT.png) <br>
Akurasi model Logistic Regression pada data test adalah sebesar 0.97 begitu juga dengan F1-score. Dilihat pada gambar metrik di atas, model ini memiliki lebih banyak kesalahan prediksi dibandingkan Random Forest, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1 dan diprediksi sebagai 2 sebanyak 3, dan label 2 yang diprediksi sebagai 1 sebanyak 2. Dilakukan cara yang sama untuk memeriksa apakah model ini overfitting atau tidak. Berikut perbandingan akurasi dan F1-Score pada train dan tes pada model Logistic Regression: <br>
LR Train Accuracy: 0.9424603174603174 <br>
LR Test Accuracy: 0.9761904761904762 <br>
LR Train F1 Score: 0.9356552509581937 <br>
LR Test F1 Score: 0.9757963693965064 <br>
Model ini tidak mengalami overfitting, bahkan akurasi dan F1-score tes lebih tinggi dibandingkan dengan train. <br>

Sejauh ini, Random Forest masih menjadi model yang unggul dibanding Random Forest.

### Evaluasi Setelah Tuning
RF Accuracy: 0.9920634920634921 <br>
RF F1 Score: 0.9916796348168897 <br>
![CM RF AT](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMRF_AT.png) <br>
Setelah dilakukan tuning, akurasi model Random Forest pada data test tetap sebesar 0.99 begitu juga dengan F1-score. Dilihat pada gambar metrik di atas, sama seperti sebelumnya, model ini hanya memiliki 2 kesalahan prediksi, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1, dan label 2 yang diprediksi sebagai 1 sebanyak 1. Berikut perbandingan akurasi dan F1-Score pada train dan tes pada model Random Forest: <br>
RF Train Accuracy: 0.9890873015873016 <br>
RF Test Accuracy: 0.9920634920634921 <br> 
RF Train F1 Score: 0.987401310877358 <br>
RF Test F1 Score: 0.9916796348168897 <br>
Model lebih baik dari sebelumnya karena model ini tidak lagi menghasilkan akurasi dan F1-score train yang lebih tinggi dari pada tes. Hal ini menandakan bahwa model Random Forest ini tidak lagi overfitting. <br>

LR Accuracy: 0.9801587301587301 <br>
LR F1 Score: 0.9797413623201591 <br>
![CM LR AT](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMLR_AT.png) <br>
Setelah dilakukan tuning, akurasi model Logistic Regression mengalami peningkatan sehingga menjadi 0.98 begitu juga dengan F1-score yang menjadi 0.979. Dilihat pada gambar metrik di atas, kesalahan yang dibuat model ini berkurang, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1 dan diprediksi sebagai 2 sebanyak 3, dan label 2 yang diprediksi sebagai 1 sebanyak 1. Berikut perbandingan akurasi dan F1-Score pada train dan tes pada model Logistic Regression: <br>
LR Train Accuracy: 0.9424603174603174 <br>
LR Test Accuracy: 0.9801587301587301 <br>
LR Train F1 Score: 0.9362806614769447 <br>
LR Test F1 Score: 0.9797413623201591 <br>
Model mengalami peningkatan baik secara akurasi dan F1-Score. Model ini juga tidak overfitting. <br>

Kesimpulannya, dari kedua model yang sudah dibuat, model terbaik yang akan dipilih adalah Random Forest karena model Random Forest memiliki performa yang lebih baik dibandingkan Logistic Regression dan model ini tidak lagi overfitting setelah tuning.
