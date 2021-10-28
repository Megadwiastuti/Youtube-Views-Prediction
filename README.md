# Youtube-Views-Prediction
Youtube-Views-Prediction merupakan salah satu tugas kelompok yang diberikan pada saat mengikuti bootcamp Data Science Rakamin Academy.

### Content
YouTube menyimpan daftar video trending teratas di platform, untuk menentukan video trending teratas, YouTube menggunakan kombinasi faktor termasuk mengukur interaksi pengguna (jumlah penayangan, pembagian, komentar, dan suka). 

### Goal
membuat model untuk memprediksi penayangan video berdasarkan angka statistik dan atribut lainnya.

### Tahapan Pengerjaan 
1. Pre-EDA Data Preprocessing
    * Pada tahapan ini dilakukan `Load Data "youtube_statistics.xlsx"` dan melihat df.info() untuk mengetahui jumlah baris dan juga jumlah kolom
    * Dilakukan pemisahan 18 kolom menjadi 2 jenis data yaitu `numerical` dan `categorical`
    * Dilakukan pengecekan `missing value` dan `outlier`
 
2. Feature Engineering
    Dilakukan feature engineering terlebih dahulu untuk memudahkan melihat EDA pada bagian feature/data kategorikal yang memiliki banyak variasi seperti:
    * Mengubah semua feature categorical yang masih memiliki beragam data type menjadi bertipe data category.
    * Melakukan segmentasi pada data views dan membagi menjadi 3 bagian yaitu low, mid dan high berdasarkan percentile.
    * Melakukan segmentasi pada publish_time dengan membagi menjadi 5 bagian yaitu morning, noon, afternoon, night, early morning.
      ●	Morning	: 4-10 am
      ●	Noon		: 10 am - 2 pm
      ●	Afternoon	: 2 pm - 6 pm
      ●	Night		: 6 pm - 12 am
      ●	Early morning	: 12 - 4 am
    * Membagi trending dan publish date menjadi 2 kolom terpisah yang terdiri dari:
      ■	Bulan-Tahun
      ■	Day


3. EDA (Exploratory Data Analysis)
   * Dari sumber yang ada ditemukan bahwa standar yang membuat sebuah video trending di youtube adalah: `Durasi menonton`, `Rata-rata persentase views`, `Durasi video`,             `Persentase penonton yang melihat video lalu keluar`, `Menonton ulang`, `Jumlah likes`, `dislikes`, `Jumlah viewers unik`, `Jumlah views per viewers unik`, `Darimana  
     audience menonton (termasuk dari luar youtube)`
  * Dilakukan pair plot untuk melihat korelasi fitur numerical dan target(views), dari pairplot dan informasi yang diperoleh terkait hal-hal yang mempengaruhi views, maka           dilakukan drop column pada column yang dianggap tidak berhubungan dengan views. Column yang didrop adalah no_tags, ratings_disabled, video_error_or_removed, desc_len, dan   
    len_title. 
    Yang diperoleh dari EDA adalah:
    a.	Video paling banyak di publish pada bulan Desember 2017 dan dari semua tahun paling banyak dipublish pada tanggal 16
    b.	Video paling banyak trending pada bulan December 2017 dan dari semua tahun paling banyak trending pada tanggal 14
    c.	Channel_title paling tinggi frekuensinya adalah Study IQ Education dengan frekuensi 172 
    d.	Publish_time paling tinggi yaitu pada pagi hari (morning) dengan frekuensi 5327
    e.	Paling banyak video yang tidak melakukan disable comment dengan frekuensi 15909
    f.	Video paling banyak berada pada kategori views “high” dengan frekuensi 6662
    2.	Dari heatmap terlihat bahwa views terlihat berkorelasi dengan likes dan comment_count, dimana melalui heatmap terlihat views dan likes nilai korelasinya 0,79 serta views 
        dan comment_count nilai korelasinya 0,50.
  * Dari barplot dengan views, dapat dilihat views paling tinggi berada pada segment malam hari (night), trending month pada April-2018 dan June-2018. Sedangkan dari barplot         trending day masih belum bisa diambil kesimpulan karena setiap bulan memiliki pola yang berbeda. 
  * Dari kategori dapat dilihat bahwa viewer yang tinggi viewsnya adalah video dengan kategori 1, 10, 24 dan 17. 


4. Data Preprocessing 
    * Sebelum melakukan modeling dilakukan preprocessing kembali untuk mengetahui publish time dan trending time termasuk pada jenis weekend atau tidak. Sehingga terdapat 4     
      kolom tambahan:
      -	publish_day_name: berisi nama hari publish
      -	trending_day_name: berisi nama hari trending
      -	is_weekend: bernilai 1 jika video dipublish pada weekend dan 0 jika dipublish pada weekdays
      -	is_weekend2: bernilai 1 jika video trending pada weekend dan 0 jika trending pada weekdays
    * Dilakukan feature encoding untuk beberapa kolom yaitu kolom publish_day_name, trending_day_name, dan segment_publish_time
    * Dilakukan normalisasi untuk mendukung modeling pada semua fitur kecuali fitur target views. 

5. Modeling
   Modeling dilakukan yaitu sebanyak 12 algoritma dari hasil tersebut diketahui bahwa **Hyperparameter Tuning dengan Randomized Search dan Grid Search** dinilai tidak jauh 
   berbeda, dengan waktu proses yang lebih lama dalam penggunaan Grid Search, sehingga kedepannya kami menggunakan `Randomized Search` untuk menghemat waktu proses.Berdasarkan 
   hasil pemodelan dan evaluasi yang dilakukan terhadap nilai RMSE dan R2 score, didapatkan metode pemodelan yang paling baik menggunakan `Random Forest` dengan `nilai RMSE 
   687689.89 dan nilai R2 score 0.67.` Berdasarkan hasil `hyperparameter tuning terhadap Random Forest`, didapatkan `5 feature dengan importance score tertinggi` untuk pemodelan 
   687690.ini secara berurutan yakni `likes`, `dislikes`, `category_id`, `comment_count`, dan `publish_hour`.
 
### Summary
Dari modeling yang dilakukan dan feature importance yang diperoleh dapat diambil beberapa rekomendasi bisnis berdasarkan fitur-fitur penting yaitu:
*	Dikarenakan likes dan comment berpengaruh ke views, maka direkomendasikan agar video yang diupload mengajak penonton untuk tidak lupa memberikan like dan comment.
*	Berdasarkan hasil analisis EDA dengan menggunakan bar plot untuk melihat hubungan antara publish time dan viewers, direkomendasikan untuk mengupload video pada waktu malam dan pagi hari, karena waktu tersebut memiliki penonton lebih banyak dibandingkan waktu lainnya.
*	Berdasarkan hasil analisis EDA, direkomendasikan untuk mengupload video dengan jenis video kategori (data diambil dari kaggle):
  -	Kategori 1: Film & Animation
  -	Kategori 10: Music
  -	Kategori 24: Entertainment
  -	Kategori 17: Sports
