# Hotel Booking Demand Cancellation Prediction

**Oleh** : Delta Group
- Adnan Alfath Santoso
- Febby Putri Milenia Piter
- Rieval Erlangga Wijanarko
- Kristina Larissa Tjandrawijaya

## Business Problem Understanding

**Context**

Sebuah perusahaan memiliki 2 jenis penginapan (resort dan hotel). Penginapan yang ada memiliki banyak pelanggan. Diantara pelanggan yang memesan kamar, terdapat pelanggan yang membatalkan (cancelling) pesanan kamar mereka. Hotel-hotel tersebut membutuhkan analisis dan perencanaan dalam mengatasi kamar yang telah dibatalkan pemesanannya karena akan mempengaruhi pendapatan hotel. Oleh karena itu, analisis dan strategi yang baik akan membantu mengurangi biaya dan waktu serta kualitas pelayanan hotel.

Target:
- 0 = Pemesanan kamar tidak dibatalkan (is_canceled = 0)
- 1 = Pemesanan kamar dibatalkan (is_canceled = 1)

**Stakeholder**
- ⁠Manajemen Hotel: Mereka bertanggung jawab untuk mengambil keputusan strategis terkait kebijakan pembatalan dan mengoptimalkan pendapatan hotel.
- ⁠Tim Operasional: Tim tsb perlu informasi untuk mengatur sumber daya dengan efisien berdasarkan prediksi pembatalan kamar.
- ⁠Tim Pemasaran: Tim tsb dapat menggunakan wawasan untuk merancang promosi yang mengurangi pembatalan dan meningkatkan kepuasan pelanggan.

**Problem Statement:**

Proses persiapan kamar sebelum siap digunakan oleh pelanggan (customer) akan memakan banyak biaya yang harus ditanggung oleh hotel dan resort (penginapan). Perusahaan ingin meningkatkan efisiensi cost dengan mengetahui pelanggan mana yang akan melakukan pembatalan pesanan. Jika hotel langsung memproses semua pesanan terhadap kamar penginapan, maka biaya tersebut akan sia-sia jika ada kamar dibatalkan oleh pelanggan.

**Goals:**
Oleh karena itu, perusahaan ingin dapat memprediksi kemungkinan sebuah pesanan menginap akan dibatalkan atau tidak, sehingga dapat memfokuskan pelayanan pada pesanan yang tidak dibatalkan. Dan juga, perusahaan ingin mengetahui apa faktor atau variabel yang membuat sebuah pesanan menginap dibatalkan, sehingga perusahaan dapat membuat strategi yang lebih baik untuk mempersuasi/mendekati pelanggan yang tidak jadi memesan (customer potensial).

**Analytic Approach:**
Tujuan utama yang akan dikerjakan pada permasalah ini adalah menganalisis data untuk menemukan pola yang membedakan customer yang mau menginap di hotel/resort dan yang tidak mau. Kemudian kita akan membangun model klasifikasi yang akan membantu perusahaan untuk dapat memprediksi probabilitas seorang customer akan/ingin menginap di hotel/resort atau tidak.

**Type 1 Error (false positive):**

Konsekuensi: sia-sianya biaya maintenance, waktu dan sumber daya

**Type 2 Error (false negative):**

Konsekuensi: kehilangan customer potensial

**Evaluation Matrix**
- **Recall:** Mengukur proporsi pembatalan yang benar yang teridentifikasi.
Karena penting untuk memastikan bahwa sistem tidak melewatkan terlalu banyak pembatalan (false negatives), yang bisa mengakibatkan kamar tidak digunakan.

- **F1-Score:** Kombinasi dari precision dan recall.
Karena jika ingin keseimbangan antara precision dan recall, F1-score adalah matriks yang bisa digunakan untuk dipertimbangkan karena memberikan gambaran yang lebih menyeluruh.

## Data Understanding

**Dataset Source**: https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand?resource=download

Dataset ini berisi informasi pemesanan untuk city hotel dan hotel resort, termasuk kapan pemesanan dibuat, durasi menginap, jumlah orang dewasa, anak-anak, dan/atau bayi, serta jumlah tempat parkir yang tersedia, dan lain-lain.

Semua informasi yang dapat mengidentifikasi secara pribadi telah dihapus dari data ini.

### Attribute Information

| Attribute                       | Data Type | Length | Description |
|---------------------------------|-----------|--------|-------------|
| hotel                           | object    | --     | Jenis hotel (Resort Hotel atau City Hotel). |
| is_canceled                     | int64     | --     | Menunjukkan apakah pemesanan dibatalkan (1) atau tidak (0). |
| lead_time                       | int64     | --     | Jumlah hari antara pemesanan dan kedatangan. |
| arrival_date_year               | int64     | --     | Tahun tanggal kedatangan. |
| arrival_date_month              | object    | --     | Bulan tanggal kedatangan. |
| arrival_date_week_number        | int64     | --     | Kedatangan pada minggu keberapa |
| arrival_date_day_of_month       | int64     | --     | Hari di bulan tanggal kedatangan. |
| stays_in_weekend_nights         | int64     | --     | Jumlah malam akhir pekan (Sabtu/Minggu) yang tamu pesan untuk menginap di hotel. |
| stays_in_week_nights            | int64     | --     | Jumlah malam hari kerja (Senin-Jumat) tamu pesan untuk menginap di hotel. |
| adults                          | int64     | --     | Jumlah orang dewasa. |
| children                        | float64   | --     | Jumlah anak. |
| babies                          | int64     | --     | Jumlah bayi. |
| meal                            | object    | --     | Jenis makanan yang dipesan. |
| country                         | object    | --     | Negara asal pelanggan. |
| market_segment                  | object    | --     | Penunjukan segmen pasar. |
| distribution_channel            | object    | --     | Saluran distribusi pemesanan. |
| is_repeated_guest               | int64     | --     | Menunjukkan apakah tamu tersebut adalah pelanggan yang pernah memesan sebelumnya (1) atau tidak (0). |
| previous_cancellations          | int64     | --     | Jumlah pemesanan sebelumnya yang dibatalkan oleh pelanggan. |
| previous_bookings_not_canceled  | int64     | --     | Jumlah pemesanan sebelumnya yang tidak dibatalkan oleh pelanggan. |
| reserved_room_type              | object    | --     | Kode tipe kamar yang dipesan. |
| assigned_room_type              | object    | --     | Kode untuk tipe kamar yang diberikan pada pemesanan. |
| booking_changes                 | int64     | --     | Jumlah perubahan yang dilakukan pada pemesanan. |
| deposit_type                    | object    | --     | Jenis deposit yang dilakukan. |
| agent                           | float64   | --     | ID agen perjalanan yang melakukan pemesanan. |
| company                         | float64   | --     | ID perusahaan yang melakukan pemesanan atau bertanggung jawab membayar pemesanan. |
| days_in_waiting_list            | int64     | --     | Jumlah hari pemesanan berada di daftar tunggu sebelum dikonfirmasi. |
| customer_type                   | object    | --     | Jenis pemesanan, dengan asumsi salah satu dari beberapa kategori. |
| adr                             | float64   | --     | Tarif Harian Rata-rata, yang didefinisikan dengan membagi jumlah seluruh transaksi penginapan dengan jumlah total malam menginap. |
| required_car_parking_spaces     | int64     | --     | Jumlah tempat parkir mobil yang dibutuhkan pelanggan. |
| total_of_special_requests       | int64     | --     | Jumlah permintaan khusus yang dibuat oleh pelanggan. |
| reservation_status              | object    | --     | Status terbaru reservasi. |
| reservation_status_date         | object    | --     | Tanggal saat status terakhir ditetapkan. |

## Data Cleaning

Masih terdapat data yang kosong (nan) pada tiap fitur/kolom dan duplikat data. Selanjutnya, kita akan membersihkan dataset menjadi siap pakai agar siap dipakai untuk melakukan analisat terhadap masalah dan juga pembuatan model machine learning.

### Handling Duplicates Data
Dikarenakan pada tahap ini masih ada duplikat data sebanyak 31994, maka treatment yang akan dilakukan adalah mengambil contoh pertama dari baris-baris duplikat.

### Handling Missing Values

**Missing Value**

- Beberapa kolom, seperti `children`, `country`, `agent`,`company`,  berisi nilai yang hilang (nan).
- Missing value pada kolom `company` adalah yang terbanyak. Sebanyak 112593 baris, berarti 94.31% data missing.
- kolom `agent` menyusul sebanyak 16430 baris (13.69%) data yang hilang.
- Dan missing value yang tidak banyak berada pada kolom `country` dan `children`, sebesar 488 (0.41%) dan 4 (<0.1%).
- Pada kolom `meal` terdapat value 'undefined'. Oleh karena itu, value tersebut akan dianggap SC (Self Catering)
  
**Missing Value Handling**
- Asumsikan jika tidak ada nilai pada kolom `company`, maka pemesanan mungkin dilakukan oleh customer yang tidak memiliki/tidak dengan atas nama perusahaan (tidak ada company id). Maka akan mengisi missing value dengan '0'.
- Asumsikan jika tidak ada nilai pada kolom `agent`, maka pemesanan mungkin dilakukan oleh customer mandiri atau tanpa melalui agen (tidak ada agent id). Maka missing value akan diisi dengan '0'.
- Jika tidak ada value dalam kolom `country` maka akan dianggap/diisi tidak diketahui 'unknown'.
- Missing value pada kolom `children`, maka akan dicari rata-rata pada jumlah anak (children) dan dibulatkan kebawah.

### Handling Unreasonable Data

Setelah handling missing value dan duplikat data, kita akan checking data-data yang tidak masuk akal. Data yang dimaksud adalah data yang sangat jauh berbeda dari kumpulan data-data yang ada (dianggap outlier). Ada beberapa angka yang sangat besar yang terlihat pada dataset.

**List data yang perlu dicek datanya:**
- lead_time = waktu kedatangan yang sangat jauh (Berdasarkan aplikasi booking hotel seperti Traveloka dan Trivago, customer hanya dapat memilih tanggal check-in sejauh 365 hari sebelum menginap).
- stays_in_week_nights = lama hari menginap customer di hari kerja (week day). Dari riset yang dilakukan, tidak ada hotel yang membatasi waktu menginap (stay) selama customer membayar biaya tambahan sesuai jumlah hari tambahan menginap. Oleh karena itu, akan dicari rata-ratanya dan membuat batasan berdasarkan rata-rata.
- days_in_waiting_list = hari dimana hotel menaruh customer di waiting list hotel (asumsi: karena keterbaasan fasilitas atau jasa). Berdasarkan riset, customer yang ditempatkan di waiting list diengaruhi dengan popularitas suatu tempat, season (musim liburan) dan ketersediannya fasilitas atau jasa. Dikarenakan tidak ada informasi mengenai 3 hal diatas, kami akan membuat asumsi dari rata-rata waiting list.
- required_car_parking_spaces = jumlah lahan parkir yang dibutuhkan per pesanan. Nantinya kita akan melihat keterkaitan jumlah guest dalam satu pesanan (total dari adults, children dan babies).
- Cek kembali apakah ada customer yang tidak menginap (stays_in_week_nights + stays_in_weekend_nights = 0)
- Cek Customer segment yang sekiranya tidak perlu.

## EDA

Sebelumnya, kita akan mengecek keseimbangan dataset antara jumlah cancel dengan yang tidak cancel.

Diketahui dataset tidak seimbang, 72.7% data adalah customer yang tidak melakukan pembatalan. Sedangkan data customer yang melakukan pembatalan hanya sebesar 27.3%.

Setelah data dibersihkan kita dapat memulai analisis datanya, kita akan mulai dengan menghitung berapa jumlah pemesanan yang dibatalkan per hotel (City dan Resort).

### Berapa banyak pemesanan yang dibatalkan?
