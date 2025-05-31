# Laporan Proyek Machine Learning - Lyonard Gemilang Putra Merdeka Gustiansyah

## Project Overview

Platform streaming film menghadapi tantangan dalam memberikan rekomendasi yang sesuai bagi penggunanya. Dengan banyaknya film yang baru saat ini, banyak pengguna yang kebingungan dalam memilih film. Per 2018 angka jumlah penonton bioskop di Indonesia saja telah mencapai lebih dari 50 juta penonton dengan jumlah produksi film luar negeri hingga dalam negeri sebanyak hampir 200 judul film yang telah tayang di seluruh Indonesia (Fajriansyah et al., 2021). Memilih film memakan sangat banyak waktu, yang tentunya hal itu bahkan dapat membuat pengguna tidak jadi menonton. Hal itu tidak hanya merugikan penonton, tetapi perusahaan juga. 

### Alasan
Masalah ini harus diatasi oleh platform streaming film agar pengguna tetap menggunakan platform tersebut. Oleh karena itu, platform streaming film memerlukan sistem rekomendasi. Sistem  rekomendasi  merupakan sistem  perangkat  lunak  yang  dapat  membuat rekomendasi  ataupun  membuat  saran  akan  item-item  yang  sesuai  kepada  pengguna,  item adalah  istilah  umum  yang  digunakan  untuk  menunjukkan  apa  yang  direkomendasikan sistem  kepada  pengguna  (Wiputra & Shandi, 2021). Dengan dibuat sistem rekomendasi, pengguna dapat memilih film yang ingin ditonton selanjutnya yang relevan dengan preferensinya. Sistem rekomendasi dapat dibuat dengan menggunakan pendekatan Content-based filtering dan juga Collaborative Filtering. Content Based Filtering menggunakan kemiripan antar produk yang akan direkomendasikan dengan produk yang disukai pengguna dan Collaborative Filtering menggunakan kemiripan kueri dengan item pengguna dengan pengguna lain (Fajriansyah et al., 2021).

### Referensi
Fajriansyah, Muhammad, et al. “Sistem Rekomendasi Film Menggunakan Content Based Filtering.” Jurnal Pengembangan Teknologi Informasi dan Ilmu Komputer, vol. 5, no. 6, 2021, http://j-ptiik.ub.ac.id/. <br>
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
- Membangun model Content-Based Filtering dengan TF-IDF dan linear kernel berdasarkan kolom genres untuk digunakan saat tidak ada data historis pengguna.
- Membangun model Collaborative Filtering berbasis neural network untuk mempelajari preferensi pengguna dengan evaluasi menggunakan Root Mean Square Error (RMSE).
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
- movieId: ID film
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
- tstamp: Waktu pengiriman rating <br>

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
- Menghapus missing value pada data (dropna())
- Menghilangkan simbol "|" pada kolom genres sehingga setiap genre dipisah dengan menggunakan spasi saja
- Data diproses menggunakan TfidVectorizer
- Mengubah matriks sparse ke dense <br>

**Alasan Tahapan Data Preparation**
- Data yang bersih dari missing value dan simbol pemisah akan dapat diolah oleh TfidVectorizer.
- TfidVectorizer mengubah teks menjadi representasi numerik yang bertujuan agar model dapat mengukur kemiripan antar film berdasarkan genre.
- Matrix yang didapat setelah diproses dijadikan dense agar dapat ditampilkan dalam bentuk tabel.

### user_rating_history.csv
- Menghapus missing value pada data (dropna())
- Menghapus data duplikat (drop_duplicates()) <br>

**Alasan Tahapan Data Preparation** <br>
Data yang bersih dari missing value dan duplikat membuat data dapat dinormalisasi/embedding dan menghindari bias dalam pelatihan model.

### merge_df 
merge_df merupakan data gabungan dari dataset movies dan user_rating_history. Dataset inin digabungkan menggunakan Left Join agar menjaga semua baris dari df_user yang ada meskipun filmnya tidak ditemukan. Berikut merupakan tahap data preparation untuk merge_df:
- Menghapus missing value pada data(dropna())
- Mengubah userId dan movieId menjadi angka integer
- Menormalkan rating menjadi rentang 0-1
- Menyusun pasangan fitur user, movie, dan label rating_norm dalam bentuk array
- Membagi data menjadi data latih dan data validasi dengan rasio 80:20<br>

**Alasan Tahapan Data Preparation**
- Data yang bersih dari missing value akan tercegah dari error dan model dapat belajar dari data yang valid.
- userId dan movieId yang diubah menjadi integer agar dapat dimasukkan ke dalam model embedding
- Rating yang dinormalisasi agar sesuai dengan fungsi aktivasi sigmoid dalam model neural network
- Pembagian data latih dan validasi penting untuk mengukur performa generalisasi model

## Modeling
Terdapat dua model yang digunakan dalam proyek ini, yaitu Content-based filtering dan Collaborative Filtering.

### Content-based filtering
Metode yang digunakan adalah TfidfVectorizer dan linear_kernel. Model ini menghitung kemiripan antar film berdasarkan genre.

#### Langkah Utama
- Mengubah teks genres menjadi vektor menggunakan TfidfVectorizer.
- Menghitung similarity matrix antar film menggunakan linear_kernel(). linear_kernel() digunakan karena lebih ringan dan efisien dan memberikan hasil yang cukup identik dengan cosine similarity.
- Mendefinisikan fungsi movie_rec() untuk mengambil k film paling mirip dengan film input.

#### Kelebihan
- Tidak membutuhkan data pengguna
- Dapat langsung digunakan hanya dari metadata film

#### Kekurangan
- Tidak mempertimbangkan preferensi atau perilaku pengguna
- Kurang efektif jika metadata terbatas atau seragam

#### Output Top-N Rekomendasi (Top 5)
![Evaluation ConBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/eval_conbased.png) <br>
Dari fungsi yang sudah didefinisikan sebelumnya bahwa nilai K adalah 5, maka ditampilkan 5 film dengan nilai similarity (kesamaan) tertinggi. Dari gambar dapat dilihat bahwa semua film yang ditampilkan memiliki nilai similarity sebesar 1 yang menandakan bahwa 5 film tersebut serupa dengan film yang dicari (Tokyo Girls (2000)). Kelima film yang ditampilkan tersebut memiliki genre yang sama seperti film yang dicari, sehingga model ini dapat dikatakan bagus karena memberi rekomendasi yang sesuai.

### Collaborative Filtering
Model yang digunakan adalah Neural Network berbasis user dan movie embedding dengan Tensorflow/Keras. <br>
Struktur model yang digunakan adalah embedding layer untuk user dan movie, dot product antara user dan movie vector ditambah dengan bias masing-masing, dan output dilewatkan ke fungsi aktivasi sigmoid. 

#### Proses Training
- Menggunakan BinaryCrossentropy() karena rating telah dinormalisasi
- Menggunakan optimizer Adam
- Menggunakan RMSE sebagai metrik
- x_train dan x_val berisi pasangan (user_id, movie_id) sebagai input dan rating_norm sebagai target
  
#### Kelebihan
- Menyesuaikan rekomendasi secara personal untuk setiap user
- Dapat menangkap pola kompleks dalam preferensi pengguna

#### Kekurangan
- Tidak dapat memberikan rekomendasi ke user baru tanpa riwayat
- Butuh banyak data agar embedding terlatih dengan baik

#### Output Top-N Rekomendasi (Top 10)
![ColBased Result](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/hasil_colbased.png) <br>
Dari hasil yang dikeluarkan model dapat dilihat bahwa film dengan rating tertinggi yang diberikan user ada dari genre seperti Drama, Mystery, Thriller, Comedy, Horror, Action, Sci-fi, dan lain sebagainya. Lalu 10 film yang direkomendasikan oleh model rata-rata memiliki genre yang sama seperti movie dengan rating tertinggi yang diberikan user. Oleh karena itu, model yang dibuat ini cukup bagus dalam merekomendasikan film, hanya saja perlu dilakukan tuning kembali untuk output yang lebih optimal dan konsisten. 

## Evaluation
Hal yang diukur untuk model content-based filtering adalah nilai similarity dan untuk model collaborative based filtering digunakan metrik Root Mean Square Error (RMSE). Berikut merupakan rumus RMSE: <br>
$RMSE = \sqrt{\frac{\sum_{i=1}^{n} (P_i - O_i)^2}{n}}$

### Content Based Filtering
Presisi

### Collaborative Based Filtering
![Evaluation ColBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/rmse.png) <br>
Dari gambar di atas, dapat dilihat bahwa performa train cukup stabil, hanya saja validation RMSE sangat fluktuatif. Hal ini menunjukkan ketidakkonsistenan generalisasi model. Hal ini juga menunjukkan bahwa model sepertinya overfitting. <br>




Kesimpulannya, dari kedua model yang sudah dibuat, model content-based filtering berhasil memberikan rekomendasi yang relevan berdasarkan kemiripan genre film dan model collaborative-based filtering berhasil memberikan rekomendasi yang lebih personal berdasarkan pola perilaku pengguna. Kedua model ini memiliki kelebihan masing-masing tergantung pada ketersediaan data. Jika metadata film tersedia tetapi histori pengguna tidak ada, maka content-based filtering merupakan solusi yang tepat. Begitu juga sebaliknya, apabila data rating pengguna tersedia, collaborative rating mampu memberikan rekomendasi yang lebih akurat dan personal. 
