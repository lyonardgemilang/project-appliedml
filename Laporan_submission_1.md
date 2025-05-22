# Laporan Proyek Machine Learning - Lyonard Gemilang Putra Merdeka Gustiansyah

## Domain Proyek

Jakarta merupakan kota yang dapat dikatakan memiliki kualitas udara terburuk di Indonesia. Dilansir dari IQAir, tidak jarang index AQI Jakarta berada di kategori tidak sehat untuk sensitif. Penyebab dari buruknya kualitas di Jakarta adalah banyaknya kendaraan dan pabrik yang membuat polusi udara semakin menumpuk. Akibatnya banyak seali masyarakat yang terkena penyakit pernapasan. Menurut laporan tahun 2021 yang diterbitkan oleh Organisasi Kesehatan Dunia (WHO), 13 orang di seluruh  dunia meninggal setiap menit akibat polusi udara dan penyakit serius seperti penyakit  kardiovaskular, stroke, dan kanker paru-paru (Perdana & Muklason, 2023).  Pada tahun 2002, sebuah studi dari Asian Development Bank memperkirakan bahwa polusi udara berdampak pada lebih dari 90 juta kasus gejala pernapasan dengan estimasi kerugian ekonomi sekitar 1,8 triliun Rupiah. Dari berbagai jenis polutan yang terdapat di udara ambien, ada dua polutan utama yang memiliki dampak merugikan paling besar pada kesehatan manusia, yaitu ozon permukaan (O3) dan PM2.5 (partikulat berdiameter kurang dari 2.5 mikrometer)  (Syuhada, 2022). Tidak hanya manusia yang terkena dampaknya, lingkungan sekitar pun menjadi terpengaruh akibat buruknya kualitas udara di Jakarta.

### Alasan
Masalah ini harus segara diatasi agar mengurangi atau bahkan dapat mencegah terjadinya penyakit pernafasan yang diakibatkan oleh polusi udara. Dengan diatasi masalah ini akan banyak masyarakat yang sehat bahkan negara pun tidak akan mengalami kerugian ekonomi. Salah satu cara yang dapat diterapkan untuk mengatasi masalah ini adalah dengan membuat model Machine Learning yang dapat mendeteksi kualitas udara yang memberikan peringatan kepada masyarakat sekitar untuk memakai masker agar mengurangi risiko terkena penyakit pernafasan.

### Referensi:
Perdana, D., & Muklason, A. (2023). Machine Learning untuk Peramalan Kualitas Indeks Standar Pencemar Udara DKI Jakarta dengan Metode Hibrid ARIMAX-LSTM. ILKOMNIKA: Journal of Computer Science and Applied Informatics, 5(3), 209–222. https://doi.org/10.28926/ilkomnika.v5i3.588 <br>
Syuhada, G. (2022, January 19). Dampak Polusi Udara bagi Kesehatan Warga Jakarta. https://rendahemisi.jakarta.go.id/article/174/dampak-polusi-udara-bagi-kesehatan-warga-jakarta#:~:text=Tingkat%20kerusakan%20atau%20keparahan%20dari,5%20mikrometer

## Business Understanding

### Problem Statements
- Bagaimana membangun model machine learning untuk memprediksi kualitas udara berdasarkan parameter pencemar udara?
- Algoritma apa yang paling efektif untuk memodelkan prediksi kualitas udara dengan akurasi tinggi?

### Goals
- Menghasilkan model prediktif yang mampu mengklasifikasikan kualitas udara dari data polutan yang tersedia.
- Mengevaluasi dan membandingkan performa dua model machine learning dalam mengklasifikasi kualitas udara.

### Solution statements
- Menggunakan dau algoritma, yaitu Random Forest Classifier dan Logistic Regression, lalu mengevaluasi kedua model dengan membandingkan akurasi dan F1-score
- Melakukan hyperparameter tuning pada kedua model untuk mengoptimalisasi performa.
- Memilih model terbaik berdasarkan hasil evaluasi metrik akurasi dan f1-score.

## Data Understanding
Data yang digunakan dalam proyek ini adalah "Air Quality Index in Jakarta" yang berisi Indeks Standar Pencemaran Udara yang diukur dari 5 stasiun pemantauan kualitas udara di Jakarta dari Januari 2010 sampai Februari 2025. Pada dataset ini terdapat 2 tipe file, yaitu ispu_dkix yang mengandung data AQI masing-masing stasiun dari Januari 2010 sampai Februari 2025 (kecuali 2022) dan ispu_dki_all yang mengandung data AQI gabungan dari keseluruhan stasiun dari Januari 2010 sampai Februari 2025 <a href="https://www.kaggle.com/datasets/senadu34/air-quality-index-in-jakarta-2010-2021">Air Quality Index in Jakarta</a>.

### Variabel-variabel pada Air Quality Index in Jakarta dataset adalah sebagai berikut:
- tanggal : Tanggal pencatatan kualitas udara
- stasiun : Lokasi stasiun pemantauan
- pm25 : Konsentrasi materi partikulat dengan diameter 2,5 mikrometer atau kurang (PM2.5), diukur dalam mikrogram per meter kubik (µg/m³).
- pm10 : Konsentrasi materi partikulat dengan diameter 10 mikrometer atau kurang (PM10), diukur dalam mikrogram per meter kubik (µg/m³).
- so2 : Konsentrasi sulfur dioksida (SO2), diukur dalam bagian per juta (ppm).
- co : Konsentrasi karbon monoksida, diukur dalam bagian per juta (ppm).
- o3 : Konsentrasi ozon (O3), diukur dalam bagian per juta (ppm).
- no2 : Konsentrasi nitrogen dioksida (NO2), diukur dalam bagian per juta (ppm).
- max : Nilai maksimum yang tercatat di antara polutan untuk tanggal dan stasiun tertentu. Nilai ini mewakili konsentrasi tertinggi di antara PM25, PM10, SO2, CO, O3, dan NO2.
- critical: Polutan yang mempunyai konsentrasi tertinggi pada tanggal dan stasiun tersebut.
- category : Kategori kualitas udara berdasarkan nilai 'maks' yang menggambarkan tingkat kualitas udara.

### Eksplorasi Data
- Boxplot <br>
![Outlier](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/bp_1.png) <br>
Gambar di atas merupakan boxplot dari kolom pm25. Data pada dataset ini memiliki banyak outlier. Tidak hanya pada kolom itu saja, kolom-kolom lainnya seperti pm10, so2, co, o3, no2, dan max memiliki outlier juga.
- Kualitas udara berdasarkan stasiun <br>
![Air Quality by Station](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/bar.png) <br>
Dari bar chart ini, dapat dilihat kualitas udara berdasarkan stasiun. Stasiun lubang buaya memiliki kualitas udara yang tidak sehat terbanyak diantara stasiun lainnya. Meskipun semua stasiun rata-rata memiliki kualitas udara yang sedang, tidak ada satu kota pun yang memiliki kualitas udara yang sangat baik. Hal ini cukup memprihatinkan. 
- Korelasi antar fitur <br>
![Correlation between each feature](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/corr.png) <br>
Dari correlation matrix ini, kolom pm25, pm10, dan max memiliki korelasi kuat terhadap label categori, yang menjadikan bahwa ketiga kolom tersebut sangat mungkin relevan untuk model klasifikasi. Kolom stasiun terhadap pm25/pm10/categori juga cukup memiliki hubungan yang menandakan bahwa stasiun juga memiliki peran terhadap kategori kualitas udara.
- Tren Historis <br>
![Historical Trend](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/tren.png) <br>
Dilihat dari line chart ini, PM2.5 merupakan polutan paling fluktuatif dan palin sering melonjak tinggi. Apabila dilihat dari pola Polutan seperti PM2.5 dan PM10, sangat memungkinkan bahwa pada 2025 akan mengalami kenaikan lagi dan mungkin akan mencapai puncak pada pertengahan 2025 seperti pada bulan Okteber 2023. 

## Data Preparation
Dalam pengerjaan proyek ini diterapkan beberapa teknik data preparation, diantara lain:
- Menghapus data duplikat dan missing value (dropna())
- Menerapkan Label Encoding untuk kolom stasiun, critical, dan categori (LabelEncoder)
- Drop kolom tanggal, max, dan critical karena tidak relevan untuk model klasifikasi
- Menerapkan metode IQR capping untuk menangani outlier
- Standarisasi fitur numerik menggunakan StandardScaler

### Alasan Tahapan Data Preparation
- Data yang bersih dari data duplikat dan missing value dan sudah distandarisasi dapat membuat model tidak bias dan dapat melakukan generalisasi dengan baik.
- Kolom tanggal hanya menunjukkan kapan data diambil sehingga tidak relevan untuk digunakan. Kolom seperti max di drop karena meskipun kolom tersebut memiliki korelasi yang kuat dengan categori, kolom ini hanya memberitahu ulang nilai polutan mana yang memiliki nilai paling tinggi. Dengan adanya kolom max, ditakutkan model hanya melihat max dan mengabaikan kontribusi polutan lain. Kolom critical juga di drop karena kolom tersebut juga hanya berisi data dari nama polutan yang paling tinggi konsentrasinya.
- Encoding perlu dilakukan agar fitur kategorikal dapat digunakan oleh algoritma machine learning yang akan dibuat.

## Modeling
Terdapat dua model yang digunakan dalam proyek ini, yaitu Random Forest Classifier dan Logistic Regression. Parameter default digunakan untuk baseline, setelah itu dilakukan hyperparameter tuning menggunakan RandomizedSearchCV.

### Kelebihan dan Kekurangan
- Random Forest memiliki performa tinggi pada data tabular dan mampu menangani data non linear dengan baik, hanya saja Random Forest merupakan model yang cukup kompleks dan membutuhkan tuning yang tepat.
- Logistic Regression merupakan model yang mudah diinterpreasi, namun kurang akurat untuk relasi non linear.

### Tuning yang Diterapkan:
- Random Forest: n_estimator, max_depth, min_samples_split
  - n_estimators: jumlah pohon dalam model
  - max_depth: kedalaman maksimum setiap pohon
  - min_samples_split: jumlah minimal sampel untuk membagi node
- Logistic Regression: tol, class_weight, max_iter
  - tol: batas toleransi konvergensi
  - class_weight: penyesuaian bobot untuk mengatasi class imbalance
  - max_iter: jumah iterasi maksimum

Dengan menggunakan RandomizedCV untuk mencari hyperparameter optimal secara acak, didapat hyperparameter terbaik seperti gambar di bawah ini
![Tuning Param](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/tuning.png) <br>

Dari 2 model yang sudah dibuat, model yang dipilih untuk klasifikasi kualitas udara adalah Random Forest. Random Forest menghasilkan akurasi dan F1 Score lebih tinggi dibandingkan Logistic Regression.

## Evaluation
Metrik yang digunakan untuk mengevaluasi model, yaitu:
- Akurasi: Proporsi prediksi benar terhadap seluruh data
- F1 Score: Gabungan precision dan recall yang mempertimbangkan ketidakseimbangan antar kelas <br>
Evaluasi dilakukan dua kali, yaitu evaluasi sebelum diterapkan tuning dan setelah diterapkan tuning.

### Evaluasi Sebelum Tuning
RF Accuracy: 0.9920634920634921 <br>
RF F1 Score: 0.9916796348168897 <br>
![CM RF](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMRF_BT.png) <br>
Akurasi model Random Forest pada data test adalah sebesar 0.99 begitu juga dengan F1-score. Dililhat pada gambar metrik di atas, model ini hanya memiliki 2 kesalahan prediksi, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1, dan label 2 yang diprediksi sebagai 1 sebanyak 1. Namun, perlu dicari tahu apakah model ini overfitting atau tidak. Salah satu caranya, yaitu dengan membandingkan akurasi test dengan train seperti di bawah ini: <br>
RF Train Accuracy: 1.0 <br>
RF Test Accuracy: 0.9920634920634921 <br>
RF Train F1 Score: 1.0 <br>
RF Test F1 Score: 0.9916796348168897 <br>
Ternyata model tersebut mengalami overfitting karena akurasi train lebih besar dibandingkan akurasi tes begitu juga dengan F1-scorenya. <br>

LR Accuracy: 0.9761904761904762 <br>
LR F1 Score: 0.9757963693965064 <br>
![CM LR](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMLR_BT.png) <br>
Akurasi model Logistic Regression pada data test adalah sebesar 0.97 begitu juga dengan F1-score. Dililhat pada gambar metrik di atas, model ini memiliki lebih banyak kesalahan prediksi dibandingkan Random Forest, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1 dan diprediksi sebagai 2 sebanyak 3, dan label 2 yang diprediksi sebagai 1 sebanyak 2. Dilakukan cara yan sama untuk memeriksa apakah model ini overfitting atau tidak. Berikut perbandingan akurasi dan F1-Score pada train dan tes pada model Logisic Regression: <br>
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
Setelah dilakukan tuning, akurasi model Random Forest pada data test tetap sebesar 0.99 begitu juga dengan F1-score. Dililhat pada gambar metrik di atas, sama seperti sebelumnya, model ini hanya memiliki 2 kesalahan prediksi, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1, dan label 2 yang diprediksi sebagai 1 sebanyak 1. Berikut perbandingan akurasi dan F1-Score pada train dan tes pada model Random Forest: <br>
RF Train Accuracy: 0.9890873015873016 <br>
RF Test Accuracy: 0.9920634920634921 <br> 
RF Train F1 Score: 0.987401310877358 <br>
RF Test F1 Score: 0.9916796348168897 <br>
Model lebih baik dari sebelumnya karena model ini tidak lagi menghasilkan akurasi dan F1-score train yang lebih tinggi dari pada tes. Hal ini menandakan bahwa model Random Forest ini tidak lagi overfitting. <br>

LR Accuracy: 0.9801587301587301 <br>
LR F1 Score: 0.9797413623201591 <br>
![CM LR AT](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/CMLR_AT.png) <br>
Setelah dilakukan tuning, akurasi model Logistic Regression mengalami peningkatan sehingga menjadi 0.98 begitu juga dengan F1-score yang menjadi 0.979. Dililhat pada gambar metrik di atas, kesalahan yang dibuat model ini berkurang, yaitu label 1 yang diprediksi sebagai 0 sebanyak 1 dan diprediksi sebagai 2 sebanyak 3, dan label 2 yang diprediksi sebagai 1 sebanyak 1. Berikut perbandingan akurasi dan F1-Score pada train dan tes pada model Logistic Regression: <br>
LR Train Accuracy: 0.9424603174603174 <br>
LR Test Accuracy: 0.9801587301587301 <br>
LR Train F1 Score: 0.9362806614769447 <br>
LR Test F1 Score: 0.9797413623201591 <br>
Model mengalami peningkatan baik secara akurasi dan F1-Score. Model ini juga tidak overfitting. <br>

Kesimpulannya, dari kedua model yang sudah dibuat, model terbaik yang akan dipilih adalah Random Forest karena model Random Forest memiliki performa yang lebih baik dibandingkan Logistic Regression dan model ini tidak lagi overfitting setelah tuning.
