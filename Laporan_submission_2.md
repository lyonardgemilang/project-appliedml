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
- Mengevaluasi efektivitas masing-masing model berdasarkan hasil rekomendasi yang dihasilkan.
- Memberikan rekomendasi film yang sesuai berdasarkan genre atau histori pengguna.

### Solution statements
- Membangun model Content-Based Filtering dengan TF-IDF dan linear kernel berdasarkan kolom genres untuk digunakan saat tidak ada data historis pengguna.
- Membangun model Collaborative Filtering berbasis neural network dengan layer embedding untuk user dan movie untuk digunakan jika data histori rating tersedia.
- Menggunakan precision untuk content-based filtering dan MAE dan RMSE untuk collaborative filtering sebagai metrik evaluasi model.

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
merge_df merupakan data gabungan dari dataset user_rating_history dan movies. Kedua dataset ini digabungkan menggunakan Left Join dengan dataset user_rating_history sebagai left table berdasarkan movieId agar menjaga semua baris dari df_user yang ada meskipun filmnya tidak ditemukan. Berikut merupakan tahap data preparation untuk merge_df:
- Menghapus missing value pada data (dropna())
- Mengubah userId dan movieId menjadi angka integer
- Menormalkan rating menjadi rentang 0-1
- Menyusun pasangan fitur user dan movie dalam bentuk array dan label rating_norm 
- Membagi data menjadi data latih dan data validasi dengan rasio 80:20 <br>

**Alasan Tahapan Data Preparation**
- Data yang bersih dari missing value akan tercegah dari error dan model dapat belajar dari data yang valid
- userId dan movieId yang diubah menjadi integer agar dapat dimasukkan ke dalam model embedding
- Rating yang dinormalisasi agar sesuai dengan fungsi aktivasi sigmoid dalam model neural network
- Pembagian data latih dan validasi penting untuk mengukur performa generalisasi model

## Modeling
Terdapat dua model yang digunakan dalam proyek ini, yaitu Content-based filtering dan Collaborative Filtering.

### Content-based filtering
Metode yang digunakan adalah TfidfVectorizer dan linear_kernel. Model ini menghitung kemiripan antar film berdasarkan genre.

#### Langkah Utama
- Menghitung similarity matrix antar film menggunakan linear_kernel(). linear_kernel() digunakan karena lebih ringan dan efisien dan memberikan hasil yang cukup identik dengan cosine similarity.
- Mendefinisikan fungsi movie_rec() untuk mengambil k film paling mirip dengan film input.

#### Kelebihan
- Tidak membutuhkan data pengguna
- Dapat langsung digunakan hanya dari metadata film

#### Kekurangan
- Tidak mempertimbangkan preferensi atau perilaku pengguna
- Kurang efektif jika metadata terbatas atau seragam

#### Output Top-N Rekomendasi (Top 10)
![Output ConBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/output_top_10_conb.png) <br>
Dari fungsi yang sudah didefinisikan sebelumnya bahwa nilai K adalah 10, maka ditampilkan 10 film yang mirip dengan film yang dicari (Tokyo Girls (2000)). Berikut merupakan lima film yang mirip dengan Tokyo Girls (2000):
1. I Called Him Morgan (2016) : Documentary
2. Elián : Documentary
3. China Not China (2018) : Documentary
4. What the Health (2017) : Documentary
5. Bobbi Jene (2017)  : Documentary
6. Canary (2023) : Documentary
7. Give Me Future: Major Lazer in Cuba (2017) : Documentary
8. OLIVIA RODRIGO: driving home 2 u (a SOUR film) (2022) : Documentary
9. Joshua: Teenager vs. Superpower (2017) : Documentary
10. Echo in the Canyon (2019) : Documentary

### Collaborative Filtering
Model yang digunakan adalah Neural Network berbasis user dan movie embedding dengan Tensorflow/Keras. <br>
Struktur model yang digunakan adalah embedding layer untuk user dan movie, dot product antara user dan movie vector ditambah dengan bias masing-masing, dan output dilewatkan ke fungsi aktivasi sigmoid. Dalam model ini digunakan juga dropout untuk mencegah overfitting.

#### Proses Training
- Menggunakan MeanSquaredError()
- Menggunakan optimizer Adam
- Menggunakan MAE dan RMSE sebagai metrik
  
#### Kelebihan
- Menyesuaikan rekomendasi secara personal untuk setiap user
- Dapat menangkap pola kompleks dalam preferensi pengguna

#### Kekurangan
- Tidak dapat memberikan rekomendasi ke user baru tanpa riwayat
- Butuh banyak data agar embedding terlatih dengan baik

#### Output Top-N Rekomendasi (Top 10)
![ColBased Result](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/hasil_colbased.png) <br>
Dari hasil yang dikeluarkan model dapat dilihat bahwa film dengan rating tertinggi yang diberikan user ada dari genre seperti Drama, Adventure, War, Crime, Horror, Film-Noir, dan Thriller. Lalu 10 film yang direkomendasikan oleh model rata-rata memiliki genre yang sama seperti movie dengan rating tertinggi yang diberikan user. Berikut merupakan 10 film rekomendasi teratas berdasarkan prediksi rating tertinggi untuk pengguna:
1. Ruby in Paradise (1993) : Drama
2. Glengarry Glen Ross (1992) : Drama
3. Everything Everywhere All at Once (2022) : Action Comedy Sci-Fi
4. Perfect Days (2023) : Drama
5. Dune: Part Two (2024) : Adventure Sci-Fi
6. Boy (2010) : Comedy Drama
7. Hunt for the Wilderpeople (2016) : Adventure Comedy
8. The Babushkas of Chernobyl (2015) : (no genres listed)
9. Little Forest (2018) : Drama
10. Parasite (2019) : Comedy Drama

## Evaluation
Hal yang diukur untuk model content-based filtering adalah presisi dan untuk model collaborative based filtering digunakan metrik Mean Absolute Error dan Root Mean Square Error (RMSE). 

### Content Based Filtering
![Precision ConBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/prec_conbased.png) <br>
Dari gambar di atas dapat dilihat output top 10 film yang mirip dengan film yang dicari, yaitu Tokyo Girls (2000). Genre yang dimiliki oleh 10 film tersebut sama seperti film yang dicari. Oleh karena itu dengan menggunakan rumus presisi: <br>
![Prec Formula](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/recsys_precision.png) <br>
didapat perhitungan sebagai berikut: <br>
$Precision = \frac{10}{10} = 1.0$<br>
Jadi, precision yang dihasilkan oleh metode ini adalah sebesar 100% yang menandakan bahwa model ini memberikan rekomendasi film dengan sangat tepat.

### Collaborative Based Filtering
#### MAE
Mean Absolute Error (MAE) adalah metrik yang mengukur rata-rata selisih absolut antara nilai prediksi dan nilai sebenarnya. MAE lebih stabil terhadap outlier. Berikut merupakan rumus dari MAE: <br>
$MAE = \frac{\sum_{i=1}^{n} \left| y_i - \lambda(x_i) \right|}{n}$ <br>
![MAE ColBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/MAE.png) <br>
Dari gambar di atas, dapat dilihat bahwa MAE train dan test sama-sama menurun yang menandakan bahwa model belajar dengan baik dan bagus. Tidak ada tanda-tanda bahwa model mengalami overfitting juga. <br>

#### RMSE
Root Mean Square Error (RMSE) adalah metrik yang mengukur seberapa baik model prediktif mendekati nilai aktual. RMSE digunakan untuk mengevaluasi seberapa jauh kesalahan besar yang dibuat model. Berbeda dengan MAE yang stabil terhadap outlier, RMSE lebih sensitif terhadap outlier. Berikut merupakan rumus RMSE: <br>
$RMSE = \sqrt{\frac{\sum_{i=1}^{n} (P_i - O_i)^2}{n}}$ <br>
![RMSE ColBased](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/RMSE.png) <br>
Dari gambar di atas, dapat dilihat bahwa RMSE train dan test menurun yang menandakan bahwa model berhasil mengurangi kesalahan prediksi. RMSE juga tidak terlihat meningkat sehingga model ini tidak mengalami overfitting. <br>

## Kesimpulan
Dari kedua model yang sudah dibuat, model content-based filtering berhasil memberikan rekomendasi yang relevan berdasarkan kemiripan genre film dan model collaborative-based filtering berhasil memberikan rekomendasi yang lebih personal berdasarkan pola perilaku pengguna. Sistem rekomendasi content-based yang dibuat menggunakan genre sebagai fitur utama berhasil menunjukkan kemiripan antar film, yaitu film dengan genre Documentary. Dari hasil evaluasi, precision yang dihasilkan oleh model content-based adalah sebesar 1.0 atau 100% sehingga memberikan rekomendasi film secara tepat dan untuk model collaborative filtering menghasilkan MAE dan RMSE yang rendah dan kedua metrik tersebut mengalami penurunan selama training dan validation, yang berarti model mampu belajar dari data dan membuat prediksi akurat. Secara keseluruhan, apabila metadata tersedia namun histori pengguna tidak ada, content-based filtering merupakan solusi yang tepat, begitu juga sebaliknya apabila data histori tersedia, maka collaborative rating akan memberi hasil rekomendasi yang lebih relevan.
