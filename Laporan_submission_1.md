# Laporan Proyek Machine Learning - Lyonard Gemilang Putra Merdeka Gustiansyah

## Domain Proyek

Jakarta merupakan kota yang dapat dikatakan memiliki kualitas udara terburuk di Indonesia. Dilansir dari IQAir, tidak jarang index AQI Jakarta berada di kategori tidak sehat untuk sensitif. Penyebab dari buruknya kualitas di Jakarta adalah banyaknya kendaraan dan pabrik yang membuat polusi udara semakin menumpuk. Akibatnya banyak seali masyarakat yang terkena penyakit pernapasan. Dilansir dari rendahemisi.jakarta.go.id, polusi udara menyebabkan lebih dari 90 juta kasus gejala pernapasan dengan kerugian ekonomi 1.8 triliun rupiah. Polutan udara yang berbahaya adalah PM2.5 dan Ozon. Tidak hanya manusia yang terkena dampaknya, lingkungan sekitar pun menjadi terpengaruh akibat buruknya kualitas udara di Jakarta.

Masalah ini harus segara diatasi agar mengurangi atau bahkan dapat mencegah terjadinya penyakit pernafasan yang diakibatkan oleh polusi udara. Dengan diatasi masalah ini akan banyak masyarakat yang sehat bahkan negara pun tidak akan mengalami kerugian ekonomi. Salah satu cara yang dapat diterapkan untuk mengatasi masalah ini adalah dengan membuat model Machine Learning yang dapat mendeteksi kualitas udara yang memberikan peringatan kepada masyarakat sekitar untuk memakai masker agar mengurangi risiko terkena penyakit pernafasan.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Jelaskan mengapa dan bagaimana masalah tersebut harus diselesaikan
- Menyertakan hasil riset terkait atau referensi. Referensi yang diberikan harus berasal dari sumber yang kredibel dan author yang jelas.
- Format Referensi dapat mengacu pada penulisan sitasi [IEEE](https://journals.ieeeauthorcenter.ieee.org/wp-content/uploads/sites/7/IEEE_Reference_Guide.pdf), [APA](https://www.mendeley.com/guides/apa-citation-guide/) atau secara umum seperti [di sini](https://penerbitdeepublish.com/menulis-buku-membuat-sitasi-dengan-mudah/)
- Sumber yang bisa digunakan [Scholar](https://scholar.google.com/)

## Business Understanding

Pada bagian ini, kamu perlu menjelaskan proses klarifikasi masalah.

Bagian laporan ini mencakup:

### Problem Statements

- Bagaimana membangun model machine learning untuk memprediksi kualitas udara berdasarkan parameter pencemar udara?
- Algoritma apa yang paling efektif untuk memodelkan prediksi kualitas udara dengan akurasi tinggi?

### Goals

- Menghasilkan model prediktif yang mampu mengklasifikasikan kualitas udara dari data polutan yang tersedia.
- Mengevaluasi dan membandingkan performa dua model machine learning dalam mengklasifikasi kualitas udara.

### Solution statements
- Menggunakan dau algoritma, yaitu Random Forest Classifier dan Logistic Regression, lalu mengevaluasi kedua model dengan membandingkan akurasi dan f1-score
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
- Boxplot
![Outlier](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/bp_1.png)
- Kualitas udara berdasarkan stasiun
![Air Quality by Station](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/bar.png)
- Korelasi antar fitur
![Correlation between each feature](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/corr.png)
- Tren Historis
![Historical Trend](https://raw.githubusercontent.com/lyonardgemilang/project-appliedml/picture/tren.png)

## Data Preparation
Dalam pengerjaan proyek ini diterapkan beberapa teknik data preparation, diantara lain:
- Menghapus data duplikat dan missing value (dropna())
- Menerapkan Label Encoding untuk kolom stasiun, critical, dan categori (LabelEncoder)
- Drop kolom tanggal karena tidak relevan untuk model klasifikasi
- Menerapkan metode IQR capping untuk menangani outlier
- Normalisasi fitur numerik menggunakan StandardScaler

### Alasan Tahapan Data Preparation
- Data yang bersih dari data duplikat dan missing value dan sudah distandarisasi dapat membuat model tidak bias dan dapat melakukan generalisasi dengan baik.
- Encoding perlu dilakukan agar fitur kategorikal dapat digunakan oleh algoritma machine learning yang akan dibuat.

## Modeling
Terdapat dua model yang digunakan dalam proyek ini, yaitu Random Forest Classifier dan Logistic Regression. Parameter default digunakan untuk baseline, setelah itu dilakukan hyperparameter tuning menggunakan RandomizedSearchCV.

### Kelebihan dan Kekurangan
- Random Forest memiliki performa tinggi pada data tabular dan mampu menangani data non linear dengan baik, hanya saja Random Forest merupakan model yang cukup kompleks dan membutuhkan tuning yang tepat.
- Logistic Regression merupakan model yang mudah diinterpreasi, namun kurang akurat untuk relasi non linear.

### Tuning yang Diterapkan:
- Random Forest: n_estimatos, max_depth, min_samples_split
- Logistic Regression: tol, class_weight, max_iter

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan kelebihan dan kekurangan dari setiap algoritma yang digunakan.
- Jika menggunakan satu algoritma pada solution statement, lakukan proses improvement terhadap model dengan hyperparameter tuning. **Jelaskan proses improvement yang dilakukan**.
- Jika menggunakan dua atau lebih algoritma pada solution statement, maka pilih model terbaik sebagai solusi. **Jelaskan mengapa memilih model tersebut sebagai model terbaik**.

## Evaluation
Metrik yang digunakan untuk mengevaluasi model, yaitu:
- Akurasi: Proporsi prediksi benar terhadap seluruh data
- F1 Score: Gabungan precision dan recall yang mempertimbangkan ketidakseimbangan antar kelas
Evaluasi dilakukan dua kali, yaitu evaluasi sebelum diterapkan tuning dan setelah diterapkan tuning.

### Evaluasi Sebelum Tuning

Sebagai contoh, Anda memiih kasus klasifikasi dan menggunakan metrik **akurasi, precision, rec
all, dan F1 score**. Jelaskan mengenai beberapa hal berikut:
- Penjelasan mengenai metrik yang digunakan
- Menjelaskan hasil proyek berdasarkan metrik evaluasi

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.

