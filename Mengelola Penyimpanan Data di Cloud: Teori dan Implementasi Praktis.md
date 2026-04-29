# Mengelola Penyimpanan Data di Cloud: Teori dan Implementasi Praktis

Pengelolaan penyimpanan data di era digital telah bergeser dari infrastruktur fisik lokal menuju infrastruktur cloud untuk mendukung kebutuhan bisnis yang dinamis dan skalabel. Cloud database merupakan database yang dijalankan di atas infrastruktur cloud dengan karakteristik utama berupa kapasitas yang dapat ditingkatkan secara fleksibel (scalable), pengelolaan penuh oleh penyedia layanan (managed service), ketersediaan data yang tinggi (high availability), serta model pembayaran berdasarkan penggunaan (pay-as-you-go). Dalam perancangan arsitektur penyimpanan data, pemahaman mengenai perbedaan SQL dan NoSQL menjadi hal penting. SQL menggunakan model relasional dengan skema tetap sehingga cocok untuk transaksi terstruktur, pelaporan, dan kebutuhan data warehouse seperti PostgreSQL. Sebaliknya, NoSQL menggunakan model dokumen dengan skema fleksibel yang lebih sesuai untuk data semi-terstruktur, log, maupun data operasional seperti MongoDB.

Dalam implementasi praktis, pengelolaan database cloud biasanya diawali dengan pemrosesan data hasil ETL dalam format CSV menggunakan Python dengan library Pandas. Untuk penyimpanan SQL, layanan PostgreSQL cloud seperti Neon, Supabase, atau Railway dapat dimanfaatkan sebagai database terkelola tanpa perlu konfigurasi server manual. Struktur database umumnya menggunakan pendekatan dimensional dengan tabel dimensi seperti `dim_date`, `dim_customer`, `dim_product`, serta tabel fakta `fact_sales`. Proses pemuatan data dilakukan menggunakan Python dengan library SQLAlchemy untuk menghubungkan file CSV ke database cloud dan memasukkan data ke masing-masing tabel. Setelah data tersimpan, analisis dapat dilakukan menggunakan query SQL seperti segmentasi pelanggan dengan menjumlahkan profit berdasarkan segmen dan analisis performa produk dengan menghitung rata-rata penjualan per kategori.

Contoh query analisis segmentasi pelanggan:

```sql
SELECT 
    c.segment, 
    SUM(f.profit) AS total_profit
FROM fact_sales f
JOIN dim_customer c 
ON f.customer_id = c.customer_id
GROUP BY c.segment
ORDER BY total_profit DESC;
```

Contoh query performa produk per kategori:

```sql
SELECT 
    p.category, 
    AVG(f.sales) AS average_sales
FROM fact_sales f
JOIN dim_product p 
ON f.product_id = p.product_id
GROUP BY p.category;
```

Selain SQL, pengelolaan penyimpanan data di cloud juga dapat menggunakan NoSQL untuk kebutuhan data operasional yang lebih fleksibel. MongoDB Atlas menyediakan layanan free cluster yang memungkinkan penyimpanan data dalam bentuk koleksi seperti `raw_transactions`. Proses pemuatan data dilakukan dengan mengonversi file CSV menjadi dictionary menggunakan Python, kemudian mengirimkannya ke database menggunakan pymongo. Setelah data tersimpan, analisis dapat dilakukan menggunakan pipeline agregasi MongoDB, misalnya untuk menghitung rata-rata penjualan berdasarkan wilayah.

Contoh agregasi MongoDB:

```python
pipeline_avg = [
    {
        "$group": {
            "_id": "$region",
            "avg_sales": {"$avg": "$sales"}
        }
    }
]
```

Dengan mengombinasikan penggunaan SQL untuk data terstruktur dan NoSQL untuk data operasional yang fleksibel, pengelolaan penyimpanan data di cloud menjadi lebih efisien, skalabel, dan mudah dikembangkan. Pendekatan ini memungkinkan perusahaan membangun arsitektur data modern yang mampu menangani kebutuhan analitik sekaligus operasional dalam satu ekosistem cloud yang terintegrasi.
