# Laporan Proyek Sistem Rekomendasi Film Menggunakan Content-Based Filtering dan Collaborative Filtering (SVD)
---
Nama: Irma Rohmatillah

---

## Project Overview

Seiring dengan meningkatnya jumlah film dan serial yang tersedia secara daring, pengguna sering kali kesulitan dalam memilih tontonan yang sesuai dengan preferensi mereka. Untuk itu, sistem rekomendasi menjadi alat yang sangat penting dalam membantu pengguna menemukan konten yang relevan dengan cepat dan efisien. Platform besar seperti Netflix, Amazon Prime, dan Disney+ telah memanfaatkan sistem rekomendasi untuk meningkatkan pengalaman pengguna serta keterlibatan pelanggan.

Namun, pengembangan sistem rekomendasi memiliki tantangan tersendiri, terutama dalam menghadapi masalah **cold start**, yaitu kondisi ketika pengguna baru belum memiliki cukup riwayat interaksi untuk dijadikan dasar rekomendasi. Oleh karena itu, perlu dikembangkan pendekatan yang mampu memberikan rekomendasi berkualitas baik kepada pengguna aktif maupun pengguna baru.

### Tujuan Proyek

Proyek ini bertujuan untuk mengembangkan sistem rekomendasi film yang dapat memberikan **Top-N rekomendasi** berdasarkan dua pendekatan berbeda:

1. **Content-Based Filtering**: Menggunakan informasi deskriptif seperti genre dan sinopsis film untuk merekomendasikan film yang mirip dengan yang pernah disukai pengguna.
2. **Collaborative Filtering (SVD)**: Menggunakan teknik matriks faktorisasi dari pustaka `Surprise` untuk memprediksi rating dan memberikan rekomendasi berdasarkan pola interaksi pengguna lain yang serupa.

Selain membangun model, proyek ini juga bertujuan untuk **mengevaluasi dan membandingkan kedua pendekatan** berdasarkan metrik tertentu seperti akurasi prediksi dan relevansi rekomendasi.

### Pentingnya Proyek Ini

Sistem rekomendasi memiliki peran strategis dalam banyak industri, tidak hanya di bidang hiburan. Pemahaman terhadap metode rekomendasi, serta tantangan-tantangan seperti cold start, sangat penting untuk mengembangkan sistem yang adaptif dan efektif dalam berbagai konteks aplikasi digital.

Proyek ini juga memberikan wawasan tentang efektivitas dua pendekatan populer dalam dunia sistem rekomendasi dan bagaimana keduanya dapat digunakan secara komplementer.

### Referensi dan Riset Terkait

- **Content-Based Filtering**:
  Lops, P., De Gemmis, M., & Semeraro, G. (2011). Content-based Recommender Systems: State of the Art and Trends. In *Recommender Systems Handbook*.
  [Link PDF Chapter](https://link.springer.com/chapter/10.1007/978-0-387-85820-3_3)

- **Collaborative Filtering dengan SVD**:
  Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix Factorization Techniques for Recommender Systems. *IEEE Computer*, 42(8), 30–37.  
  [https://ieeexplore.ieee.org/document/5197422](https://ieeexplore.ieee.org/document/5197422)

- **Cold Start Problem**:
  Schein, A. I., Popescul, A., Ungar, L. H., & Pennock, D. M. (2002). Methods and Metrics for Cold-Start Recommendations. In *Proceedings of the 25th Annual International ACM SIGIR Conference on Research and Development in Information Retrieval*.
  [https://dl.acm.org/doi/10.1145/564376.564421](https://dl.acm.org/doi/10.1145/564376.564421)


Proyek ini akan menghasilkan sistem rekomendasi film yang mampu memberikan saran yang personal dan relevan kepada pengguna, baik yang sudah memiliki riwayat interaksi maupun pengguna baru. Dengan membandingkan dua pendekatan utama, proyek ini juga memberikan kontribusi praktis terhadap pengembangan sistem rekomendasi yang lebih adaptif dan responsif.

---
## Business Understanding

### Problem Statements
- Bagaimana cara menyarankan film yang relevan kepada pengguna berdasarkan preferensi sebelumnya?
- Bagaimana mengatasi masalah cold start atau pengguna baru dengan sedikit data?

### Goals
- Mengembangkan sistem rekomendasi film yang mampu memberikan Top-N rekomendasi bagi pengguna.
- Menggunakan dua pendekatan berbeda untuk mengevaluasi dan membandingkan hasilnya.

### Solution Approach
- **Content-Based Filtering:** Menggunakan deskripsi film dan genre untuk menyarankan item serupa.
- **Collaborative Filtering:** Menggunakan teknik SVD dari pustaka Surprise untuk memprediksi rating dan memberikan rekomendasi.

---
## Data Understanding

Dataset MovieLens 100k berisi 100.000 rating yang diberikan oleh 943 pengguna terhadap 1682 film. Terdapat tiga file utama:
- `u.data`: Data rating (user_id, item_id, rating, timestamp)
- `u.item`: Metadata film (item_id, title, genre, dsb)
- `u.user`: Informasi pengguna

Link sumber data: [https://grouplens.org/datasets/movielens/100k](https://grouplens.org/datasets/movielens/100k)

#### Data yang Digunakan

Pada proyek sistem rekomendasi ini, beberapa file dataset dari **MovieLens 100k** akan digunakan. Masing-masing file memuat informasi penting yang dibutuhkan dalam proses rekomendasi, baik berbasis konten (*Content-Based Filtering*) maupun kolaboratif (*Collaborative Filtering*).

#### 1. Users
Data pengguna memuat informasi demografis dasar mengenai setiap pengguna yang memberikan rating:
- `user_id`: ID unik pengguna  
- `age`: Usia pengguna  
- `sex`: Jenis kelamin pengguna  
- `occupation`: Pekerjaan pengguna  
- `zip_code`: Kode pos tempat tinggal pengguna  

#### 2. Genres
File ini memuat daftar genre yang tersedia:
- `genre_id`: ID genre  
- `genre_name`: Nama genre (misalnya: Action, Comedy, Drama)  

#### 3. Movies
Data film memuat metadata tentang film yang tersedia dalam sistem:
- `movie_id`: ID unik film  
- `movie_title`: Judul film  
- `release_date`: Tanggal rilis film  
- `video_release_date`: Tanggal rilis video (jika tersedia)  
- `IMDb_URL`: Link ke halaman IMDb film  
- **Genre Encoding (One-hot)**: 19 kolom genre yang menunjukkan apakah film termasuk dalam genre tertentu (bernilai 1 jika ya, 0 jika tidak)  

#### 4. Ratings
Data interaksi antara pengguna dan film dalam bentuk rating:
- `user_id`: ID pengguna yang memberikan rating  
- `movie_id`: ID film yang diberikan rating  
- `rating`: Nilai rating (skala 1–5)  
- `unix_timestamp`: Waktu saat rating diberikan (dalam format UNIX timestamp)  

### Exploratory Data Analysis (EDA) 
Merupakan tahap awal analisis data untuk mengenali sifat-sifat utama, susunan, dan elemen penting dalam dataset sebelum analisis statistik atau prediksi yang lebih mendalam.

Berikut adalah tahapan EDA yang telah dilakukan:

#### Users
![image](https://github.com/user-attachments/assets/734cef16-79a5-457f-85cb-5ce08d83bb8c)

ringkasan informasi Users: 
- Total pengguna: Ada 943 pengguna yang memberikan rating, ini menunjukkan ukuran komunitas pengguna yang cukup besar.

- Total rating: Terdapat 100,000 rating yang telah diberikan, berarti rata-rata setiap pengguna cukup aktif dalam memberikan feedback.

- Rata-rata rating per pengguna: Setiap pengguna rata-rata memberikan sekitar 106 rating. Ini adalah indikasi bahwa sebagian besar pengguna aktif dan memberi cukup banyak rating.

- Minimum rating per pengguna: Setiap pengguna memberikan minimal 20 rating, artinya data tidak mengandung pengguna yang hanya memberikan sedikit rating, yang bagus untuk kestabilan model rekomendasi.

- Maksimum rating per pengguna: Ada pengguna yang sangat aktif, dengan 737 rating. Ini menunjukkan adanya pengguna power-user yang sangat aktif berinteraksi dengan sistem.

- Skewness sebesar 1.91: Distribusi jumlah rating per pengguna miring ke kanan (positif)

#### Genres
![image](https://github.com/user-attachments/assets/0c75f8a7-3634-4c89-8791-0ebedb514fc0)

Ringkasan Informasi genre:
- Total genre assignments: Terdapat 2,893 penugasan genre di seluruh film, artinya setiap film bisa punya lebih dari satu genre.

- Rata-rata genre per film: Setiap film memiliki rata-rata 1.72 genre, yang menunjukkan sebagian besar film menggabungkan satu atau dua genre sekaligus.

- Genre paling umum: Genre Drama adalah yang paling banyak muncul dengan 725 film, menunjukkan bahwa drama adalah tema yang sangat populer di dataset ini.

- Genre paling sedikit: Genre unknown hanya muncul pada 2 film, yang menunjukkan data hampir lengkap terkait kategori genre untuk film-film di dataset.

#### Ratings
![image](https://github.com/user-attachments/assets/bc4e9c9e-1794-4922-b150-106fdb138cb1)

Distribusi rating menunjukkan kecenderungan pengguna yang lebih sering memberikan penilaian positif (di atas 3). Ini dapat mengindikasikan:

- Film-film dalam dataset cenderung disukai pengguna, atau

- Bias positif dalam sistem rating, di mana pengguna lebih terdorong untuk menilai film yang mereka sukai.

Distribusi ini penting untuk diperhatikan dalam pengembangan sistem rekomendasi, agar model tidak hanya mengandalkan popularitas film dengan rating tinggi, tetapi juga mempertimbangkan keragaman preferensi pengguna.

---
## Data Preparation

Dalam tahap ini dilakukan beberapa langkah seperti:
- **Merge** data rating dengan metadata film
- **Preprocessing** pada judul/genre
- **TF-IDF vectorization** untuk konten film
- **Mapping** ID pengguna dan film

- Feature Extraction:
  - Untuk Content-Based: TF-IDF dari genre (atau sinopsis jika tersedia).
  - Untuk Collaborative: Dataset Surprise dari data rating.
- Cleaning: Hapus data duplikat dan cek missing value.
  
Langkah ini penting agar data siap digunakan dalam model rekomendasi baik berbasis konten maupun kolaboratif.




---
## Modelling

Tahapan ini membahas mengenai model sistem rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan Top‑N recommendation sebagai output.

Dua model dibangun untuk sistem rekomendasi:

### 1. Content‑Based Filtering
- **Penjelasan**: Menggunakan representasi TF‑IDF dari atribut genre film untuk membangun profil konten, kemudian menghitung cosine similarity antar film.  
- **Proses**:  
  1. Ekstraksi fitur genre dan deskripsi film.  
  2. Vectorization menggunakan TF‑IDF.  
  3. Penghitungan cosine similarity untuk mencari film serupa.  
  4. Mengambil Top‑N film dengan skor similarity tertinggi sebagai rekomendasi.  
- **Kelebihan**:  
  - Rekomendasi langsung berdasarkan konten, cocok untuk item baru.  
  - Interpretasi mudah karena berbasis fitur film.  
- **Kekurangan**:  
  - Kurang memperhatikan preferensi pengguna lain.  
  - Terbatas pada fitur konten yang tersedia.  

### 2. Collaborative Filtering (SVD)
- **Penjelasan**: Menggunakan algoritma Singular Value Decomposition (SVD) dari pustaka Surprise untuk memprediksi rating pengguna terhadap film, lalu memberikan rekomendasi berdasarkan prediksi rating tertinggi.  
- **Proses**:  
  1. Menyiapkan data rating dalam format Surprise `Dataset`.  
  2. Split menjadi data train dan test.  
  3. Melatih model SVD (misal `n_factors=50, n_epochs=20`).  
  4. Memprediksi rating film yang belum ditonton pengguna.  
  5. Mengambil Top‑N film dengan estimated rating tertinggi.  
- **Kelebihan**:  
  - Memanfaatkan pola interaksi pengguna lain (kolaborasi).  
  - Dapat memberikan rekomendasi personal meski fitur film terbatas.  
- **Kekurangan**:  
  - Memerlukan cukup data rating untuk performa optimal (cold‑start issue).  
  - Model kurang transparan dibanding content‑based.  



- Testing Content-Based Filtering
Rekomendasi untuk **Toy Story (1995)**  
Target genres: *Children's, Animation, Comedy*  
Precision@10: **1.00**

| No. | Title                                                      | Genre       | Similarity |
|-----|------------------------------------------------------------|-------------|------------|
| 1   | Aladdin and the King of Thieves (1996)                     | Children's  | 1.0000     |
| 2   | The Aristocats (1970)                                      | Children's  | 0.9370     |
| 3   | Pinocchio (1940)                                           | Children's  | 0.9370     |
| 4   | The Sword in the Stone (1963)                              | Children's  | 0.9370     |
| 5   | The Fox and the Hound (1981)                               | Children's  | 0.9370     |
| 6   | Winnie the Pooh and the Blustery Day (1968)                | Children's  | 0.9370     |
| 7   | Balto (1995)                                               | Children's  | 0.9370     |
| 8   | Oliver & Company (1988)                                    | Children's  | 0.9370     |
| 9   | The Swan Princess (1994)                                   | Children's  | 0.9370     |
| 10  | The Land Before Time III: The Time of the Great Giving (1995) (V) | Children's  | 0.9370     |

![image](https://github.com/user-attachments/assets/27a0cd68-5866-43a0-b0f7-0d1278a2c298)

Insight Rekomendasi untuk "Toy Story (1995)"
Target Genre: Film ini termasuk genre Children's, Animation, dan Comedy. Sistem merekomendasikan film-film dengan genre serupa, yang menunjukkan model berhasil menangkap profil genre film target dengan tepat.

Precision@10 = 1.00: Artinya semua 10 film teratas yang direkomendasikan sangat relevan dengan genre film Toy Story. Ini menunjukkan performa yang sangat baik dalam hal kesesuaian genre.

Film-film direkomendasikan sebagian besar adalah film anak-anak klasik seperti Aladdin and the King of Thieves, The Aristocats, dan Pinocchio, yang juga memiliki elemen animasi dan cerita anak yang mirip.

Similarity Score tinggi (~0.937 - 1.000) menunjukkan bahwa model menggunakan fitur konten (genre) dengan sangat efektif untuk mengukur kemiripan antar film.

Rekomendasi ini akan membantu pengguna yang menyukai film keluarga dan animasi untuk menemukan film lain yang memiliki tema dan genre yang serupa, meningkatkan pengalaman pengguna dalam menemukan konten yang relevan dan menarik.

- Testing Collaborative Filtering (SVD)
  
Top‑10 Rekomendasi untuk **User 196**

| No. | Title                                                        | Main Genre | Estimated Rating |
|-----|--------------------------------------------------------------|------------|------------------|
| 1   | The Wrong Trousers (1993)                                    | Animation  | 4.6135           |
| 2   | The Shawshank Redemption (1994)                              | Drama      | 4.5700           |
| 3   | A Close Shave (1995)                                         | Animation  | 4.5570           |
| 4   | Schindler’s List (1993)                                      | Drama      | 4.5445           |
| 5   | Casablanca (1942)                                            | Drama      | 4.5216           |
| 6   | Rear Window (1954)                                           | Mystery    | 4.4628           |
| 7   | Wallace & Gromit: The Best of Aardman Animation (1996)       | Animation  | 4.4508           |
| 8   | 12 Angry Men (1957)                                          | Drama      | 4.4472           |
| 9   | The Usual Suspects (1995)                                    | Crime      | 4.4402           |
| 10  | North by Northwest (1959)                                    | Comedy     | 4.4264           |

![image](https://github.com/user-attachments/assets/329f4ef2-f9e2-4aae-ac9d-03646d5609d7)
Insight Rekomendasi Collaborative Filtering untuk User 196
Variasi genre film yang direkomendasikan cukup beragam, mulai dari Animation, Drama, Mystery, Crime, hingga Comedy. Hal ini menunjukkan model mampu menangkap preferensi pengguna yang mungkin tidak hanya terpaku pada satu genre saja.

Estimasi rating tertinggi (~4.4 hingga 4.6) menunjukkan bahwa model memprediksi User 196 akan sangat menyukai film-film ini berdasarkan pola rating pengguna lain yang mirip.

Beberapa film rekomendasi seperti The Shawshank Redemption, Schindler's List, dan 12 Angry Men merupakan film klasik dan sangat populer, mengindikasikan bahwa pengguna mungkin menyukai film dengan kualitas tinggi dan cerita yang mendalam.

Rekomendasi animasi seperti Wrong Trousers, The dan Wallace & Gromit menunjukkan preferensi User 196 juga terhadap film animasi berkualitas, yang mungkin relevan dengan minat film anak-anak atau animasi stop-motion.

Model Collaborative Filtering memanfaatkan pola rating pengguna lain untuk memberikan rekomendasi personal, yang memungkinkan menemukan film-film berkualitas yang belum pernah ditonton oleh User 196 tapi berpotensi disukai.

Rekomendasi ini memberikan pengalaman yang dipersonalisasi dengan memperhitungkan interaksi pengguna secara langsung, sehingga lebih adaptif terhadap selera individual.

---
## Evaluation

Evaluasi performa kedua model dilakukan menggunakan metrik yang sesuai:

- **Content-Based Filtering:** Precision@K untuk mengukur relevansi rekomendasi berdasarkan genre.
- **Collaborative Filtering (SVD):** RMSE dan MAE untuk mengukur akurasi prediksi rating.

Hasil evaluasi akan digunakan untuk membandingkan kekuatan kedua pendekatan dan menentukan strategi terbaik untuk sistem rekomendasi.

### Hasil Evaluasi Model

Evaluasi dilakukan untuk mengukur performa dua pendekatan sistem rekomendasi:

1. **Content-Based Filtering**
2. **Collaborative Filtering (SVD)**

### 🔹 Content-Based Filtering

Evaluasi menggunakan metrik **Precision@10**, yang mengukur proporsi item relevan dalam 10 rekomendasi teratas berdasarkan genre film.

| Judul Film              | Precision@10 |
|-------------------------|--------------|
| Toy Story (1995)        | 1.00         |
| Star Wars (1977)        | 1.00         |
| Fargo (1996)            | 1.00         |
| Pulp Fiction (1994)     | 1.00         |
| Return of the Jedi (1983) | 1.00       |

📌 **Rata-rata Precision@10: 1.00**

Hasil ini menunjukkan bahwa pendekatan Content-Based mampu memberikan rekomendasi yang sangat relevan secara konten (genre).

---

### 🔹 Collaborative Filtering (SVD)

Evaluasi menggunakan metrik berikut:

- **RMSE (Root Mean Squared Error)**: 0.9336  
- **MAE (Mean Absolute Error)**: 0.7378

Metrik ini digunakan untuk menilai akurasi prediksi rating model terhadap data uji. Nilai RMSE dan MAE yang cukup rendah menunjukkan bahwa prediksi model cukup dekat dengan rating aktual.

---

### Kesimpulan

- **Content-Based Filtering** sangat baik dalam menghasilkan rekomendasi relevan berdasarkan kemiripan konten.
- **Collaborative Filtering (SVD)** efektif dalam memprediksi rating pengguna dengan tingkat kesalahan yang dapat diterima.
- Kombinasi kedua pendekatan ini dapat saling melengkapi dan meningkatkan kualitas rekomendasi secara keseluruhan.

---
