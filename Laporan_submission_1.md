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

Menjelaskan pernyataan masalah latar belakang:
- Bagaimana model machine learning dapat mengatasi masalah polusi udara di Jakarta?
- 
- Pernyataan Masalah n

### Goals

Menjelaskan tujuan dari pernyataan masalah:
- Jawaban pernyataan masalah 1
- Jawaban pernyataan masalah 2
- Jawaban pernyataan masalah n

Semua poin di atas harus diuraikan dengan jelas. Anda bebas menuliskan berapa pernyataan masalah dan juga goals yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Menambahkan bagian “Solution Statement” yang menguraikan cara untuk meraih goals. Bagian ini dibuat dengan ketentuan sebagai berikut: 

    ### Solution statements
    - Mengajukan 2 atau lebih solution statement. Misalnya, menggunakan dua atau lebih algoritma untuk mencapai solusi yang diinginkan atau melakukan improvement pada baseline model dengan hyperparameter tuning.
    - Solusi yang diberikan harus dapat terukur dengan metrik evaluasi.

## Data Understanding
Data yang digunakan dalam proyek ini adalah "Air Quality Index in Jakarta" yang berisi Indeks Standar Pencemaran Udara yang diukur dari 5 stasiun pemantauan kualitas udara di Jakarta dari Januari 2010 sampai Februari 2025. Pada dataset ini terdapat 2 tipe file, yaitu ispu_dkix yang mengandung data AQI masing-masing stasiun dari Januari 2010 sampai Februari 2025 (kecuali 2022) dan ispu_dki_all yang mengandung data AQI gabungan dari keseluruhan stasiun dari Januari 2010 sampai Februari 2025.

Paragraf awal bagian ini menjelaskan informasi mengenai data yang Anda gunakan dalam proyek. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Restaurant+%26+consumer+data).

Selanjutnya uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

### Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- tanggal : The date when the AQI measurement was recorded.
- stasiun : The name or identifier of the monitoring station where the measurement was taken.
- pm25 : The concentration of particulate matter with a diameter of 2.5 micrometers or less (PM2.5), measured in micrograms per cubic meter (µg/m³).
- pm10 : The concentration of particulate matter with a diameter of 10 micrometers or less (PM10), measured in micrograms per cubic meter (µg/m³).
- so2 : The concentration of sulfur dioxide (SO2), measured in parts per million (ppm).
- co : The concentration of carbon monoxide (CO), measured in parts per million (ppm).
- o3 : The concentration of ozone (O3), measured in parts per million (ppm).
- no2 : The concentration of nitrogen dioxide (NO2), measured in parts per million (ppm).
- max : The maximum value recorded among the pollutants for that particular date and station. This value represents the highest concentration among PM25, PM10, SO2, CO, O3, and NO2.
- critical: The pollutant that had the highest concentration for that date and station.
- category : The air quality category based on the 'max' value that describes the air quality level.

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data atau exploratory data analysis.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model machine learning yang digunakan untuk menyelesaikan permasalahan. Anda perlu menjelaskan tahapan dan parameter yang digunakan pada proses pemodelan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan kelebihan dan kekurangan dari setiap algoritma yang digunakan.
- Jika menggunakan satu algoritma pada solution statement, lakukan proses improvement terhadap model dengan hyperparameter tuning. **Jelaskan proses improvement yang dilakukan**.
- Jika menggunakan dua atau lebih algoritma pada solution statement, maka pilih model terbaik sebagai solusi. **Jelaskan mengapa memilih model tersebut sebagai model terbaik**.

## Evaluation
Pada bagian ini anda perlu menyebutkan metrik evaluasi yang digunakan. Lalu anda perlu menjelaskan hasil proyek berdasarkan metrik evaluasi yang digunakan.

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

