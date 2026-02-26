# Peranan Data dalam Keputusan Bisnis 
# Landasan Teori 
Definisi Data
Data adalah fakta mentah yang belum diolah, seperti angka penjualan, jumlah pelanggan, atau log aktivitas pengguna. Data belum memiliki makna sebelum diproses lebih lanjut.

Alur Data menjadi Insight
Data mentah diolah menjadi informasi melalui proses pembersihan dan analisis. Informasi yang dianalisis lebih dalam akan menghasilkan insight.
Alurnya adalah: Data → Informasi → Insight.

Peranan Data dalam Bisnis
Data membantu bisnis dalam:
Menentukan strategi pemasaran.
Mengukur KPI (Key Performance Indicator).
Memprediksi tren dan perilaku pelanggan.
Dengan data, keputusan bisnis menjadi lebih terukur dan objektif.

Data-Driven Business
Data-driven business adalah bisnis yang mengambil keputusan berdasarkan data, bukan hanya intuisi.
Contohnya adalah Netflix yang menggunakan data untuk merekomendasikan film, dan Gojek yang memanfaatkan data untuk mengatur harga dan layanan.

Peran Infrastruktur Data
Infrastruktur data berperan dalam menyimpan, mengintegrasikan, dan mengolah data. Tanpa infrastruktur yang baik, data sulit dianalisis dan dimanfaatkan secara maksimal.

# Praktik sederhana menggunakan Google Colab sebagai platform pengembangan berbasis Python.

Langkah-langkah Python

Inisialisasi dasar dengan fungsi:
print("Hello, Data World!")
Import library Pandas untuk pengolahan data:
import pandas as pd
Memuat dataset bisnis dari sumber publik menggunakan:
  data = pd.read_csv(url)
Hasil Inspeksi Data

data.head() digunakan untuk melihat 5 baris pertama dataset.

data.info() digunakan untuk melihat jumlah baris, nama kolom, serta tipe data.
