# Implementasi Pipeline ETL Multi-Source Menggunakan Python dan SQLite untuk Integrasi Data Siswa

Pada proyek ini dilakukan implementasi proses ETL (Extract, Transform, Load) menggunakan Python untuk mengintegrasikan data siswa dari tiga sumber data yang berbeda, yaitu file CSV, SQL Dump, dan JSON. Hasil integrasi kemudian disimpan ke dalam database SQLite sebagai data warehouse sederhana. Selain itu, proses ETL juga dilengkapi dengan fitur logging dan scheduler untuk mendukung otomatisasi serta monitoring proses pengolahan data.

## Deskripsi Dataset

Dataset yang digunakan terdiri dari tiga sumber data berbeda. Sumber pertama adalah file CSV yang berisi data siswa dari Sekolah A dengan format tabular. Sumber kedua adalah file SQL Dump yang berasal dari database Sekolah B dan berisi perintah SQL untuk membuat tabel serta data siswa. Sumber ketiga adalah file JSON yang berasal dari Sekolah C dengan struktur data bertingkat (nested JSON).

Masing-masing sumber memiliki nama kolom dan struktur data yang berbeda. Oleh karena itu, diperlukan proses transformasi agar seluruh data dapat disatukan ke dalam satu skema yang seragam sebelum dimasukkan ke data warehouse.

## Perancangan Arsitektur ETL

Proses ETL yang dibangun pada proyek ini terdiri dari tiga tahapan utama, yaitu Extract, Transform, dan Load. Pada tahap Extract, data dibaca dari ketiga sumber yang berbeda menggunakan Python. Setelah data berhasil diperoleh, dilakukan tahap Transform untuk menyamakan struktur data, melakukan pembersihan data, serta menambahkan metadata yang diperlukan. Tahap terakhir adalah Load, yaitu memasukkan data yang telah terintegrasi ke dalam database SQLite.

Alur proses ETL yang diterapkan dapat digambarkan sebagai berikut:

CSV + SQL Dump + JSON → Extract → Transform → Integrasi Data → SQLite Data Warehouse → Logging → Scheduler

Arsitektur tersebut memungkinkan data dari berbagai sumber untuk diproses secara otomatis dan tersimpan dalam satu repositori data yang terpusat.

## Tahap Extract

Tahap Extract bertujuan untuk mengambil data dari seluruh sumber yang tersedia. File CSV dibaca menggunakan library Pandas sehingga menghasilkan DataFrame yang mudah diolah. File JSON diproses dengan melakukan parsing terhadap struktur nested JSON untuk mengambil informasi identitas siswa dan informasi sekolah. Sementara itu, file SQL Dump diekstraksi menggunakan teknik Regular Expression (Regex) untuk mengambil data yang terdapat pada perintah INSERT INTO.

Hasil dari tahap Extract menghasilkan tiga DataFrame terpisah, yaitu data dari CSV, data dari JSON, dan data dari SQL Dump. Ketiga DataFrame tersebut kemudian diproses pada tahap berikutnya.

## Tahap Transform

Tahap Transform dilakukan untuk menyamakan struktur data dari seluruh sumber. Perbedaan nama kolom yang ditemukan pada masing-masing dataset disesuaikan agar mengikuti skema target yang telah ditentukan. Kolom seperti nama_siswa diubah menjadi nama, alamat_sekolah menjadi alamat, serta berbagai nama kolom lainnya disesuaikan agar memiliki format yang konsisten.

Selain penyamaan nama kolom, dilakukan pula penambahan kolom metadata berupa source_file untuk menunjukkan asal data dan created_at untuk mencatat waktu proses ETL dilakukan. Data yang memiliki nilai kosong ditangani sesuai kebutuhan dan dilakukan proses penghapusan data duplikat berdasarkan kolom NISN.

Setelah proses transformasi selesai, seluruh data dari ketiga sumber berhasil memiliki struktur yang sama sehingga siap untuk diintegrasikan.

## Integrasi Data

Data hasil transformasi dari ketiga sumber kemudian digabungkan menggunakan fungsi concat() pada Pandas. Proses ini menghasilkan satu DataFrame utama yang berisi seluruh data siswa dari berbagai sekolah.

Hasil integrasi menunjukkan bahwa total data yang berhasil digabungkan berjumlah 11 data siswa. Seluruh data telah memiliki struktur yang seragam sehingga dapat diproses lebih lanjut pada tahap penyimpanan ke data warehouse.

## Implementasi Data Warehouse Menggunakan SQLite

Data warehouse pada proyek ini dibangun menggunakan SQLite dengan nama database warehouse_siswa.db. SQLite dipilih karena ringan, mudah digunakan, dan tidak memerlukan instalasi server database tambahan.

Di dalam database dibuat tabel master_siswa yang berfungsi sebagai tabel utama penyimpanan data hasil integrasi. Kolom NISN ditetapkan sebagai Primary Key untuk menjamin keunikan data siswa. Selain itu, diterapkan mekanisme UPSERT menggunakan ON CONFLICT sehingga apabila terdapat data dengan NISN yang sama, maka data akan diperbarui secara otomatis tanpa menimbulkan duplikasi.

Setelah proses Load dilakukan, seluruh data siswa berhasil tersimpan ke dalam tabel master_siswa dengan total 11 record.

## Logging dan Monitoring

Untuk mendukung monitoring proses ETL, digunakan library logging yang mencatat seluruh aktivitas ETL ke dalam file etl_log.log. Informasi yang dicatat meliputi waktu proses ETL, sumber data yang diproses, jumlah data yang berhasil diolah, serta status keberhasilan proses.

Hasil logging menunjukkan bahwa data dari CSV sebanyak 4 record, data JSON sebanyak 3 record, dan data SQL sebanyak 4 record berhasil diproses sehingga menghasilkan total 11 data. Status ETL juga tercatat sebagai sukses yang menunjukkan bahwa seluruh proses berjalan tanpa kendala.

## Implementasi Scheduler

Agar proses ETL dapat berjalan secara otomatis, digunakan library schedule sebagai scheduler. Scheduler dikonfigurasi untuk menjalankan proses ETL setiap satu menit sebagai simulasi otomatisasi pipeline data.

Ketika scheduler dijalankan, sistem secara otomatis mengeksekusi proses ETL tanpa perlu intervensi pengguna. Setiap eksekusi akan menghasilkan log baru yang dapat digunakan untuk memantau kondisi sistem.

Implementasi scheduler menunjukkan bahwa proses ETL dapat berjalan secara berkala sesuai jadwal yang telah ditentukan.

## Simulasi Penambahan Data Baru

Sebagai bagian dari pengujian, dilakukan simulasi penambahan satu data siswa baru ke dalam file CSV. Setelah data ditambahkan, proses ETL dijalankan kembali sehingga sistem membaca perubahan pada sumber data.

Hasil pengujian menunjukkan bahwa data baru berhasil terdeteksi dan dimasukkan ke dalam database SQLite secara otomatis. Mekanisme UPSERT memastikan bahwa data baru dapat ditambahkan tanpa menyebabkan konflik dengan data yang sudah ada sebelumnya.

Pengujian ini membuktikan bahwa pipeline ETL yang dibangun mampu menangani perubahan data secara dinamis.

## Hasil dan Evaluasi

Implementasi pipeline ETL berhasil mengintegrasikan data dari tiga sumber yang berbeda ke dalam satu data warehouse berbasis SQLite. Seluruh proses mulai dari Extract, Transform, Load, Logging, hingga Scheduler berjalan sesuai dengan kebutuhan yang telah ditentukan pada studi kasus.

Data warehouse yang dihasilkan mampu menyimpan data siswa secara terpusat dan konsisten sehingga memudahkan proses pengelolaan serta analisis data di masa mendatang.

## Kesimpulan

Berdasarkan hasil implementasi, dapat disimpulkan bahwa proses ETL berhasil dibangun menggunakan Python untuk mengintegrasikan data dari file CSV, SQL Dump, dan JSON. Seluruh data berhasil ditransformasikan ke dalam format yang seragam dan disimpan ke dalam database SQLite sebagai data warehouse sederhana.

Selain itu, fitur logging dan scheduler berhasil diimplementasikan sehingga proses ETL dapat dimonitor dan dijalankan secara otomatis. Dengan adanya sistem ini, pengelolaan data menjadi lebih terstruktur, efisien, dan mudah dikembangkan untuk kebutuhan analitik yang lebih kompleks di masa depan.
