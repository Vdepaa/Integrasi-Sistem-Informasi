Tabel Hasil Pengujian Benchmark

=================================================================
Data Count   | XML (Bytes)  | JSON (Bytes) | Efisiensi (%)  
=================================================================
10           | 1438         | 1166         | 18.92%
50           | 7058         | 5826         | 17.46%
100          | 14084        | 11652        | 17.27%
500          | 70684        | 58652        | 17.02%
=================================================================


# Analisis Pertanyaan Kritis 

## 1. Analisis Struktur Payload
XML menghasilkan ukuran data yang lebih besar karena menggunakan tag pembuka dan tag penutup pada setiap elemennya. Contohnya, XML menggunakan format <nama>Dewi Sartika</nama>, sedangkan JSON cukup menggunakan "nama": "Dewi Sartika". Selain itu, XML sering kali membutuhkan struktur elemen bertingkat seperti <mahasiswa><nama>...</nama><jurusan>...</jurusan></mahasiswa>. Tag-tag berulang tersebut menambah jumlah karakter yang harus dikirim melalui jaringan, sehingga overhead sintaksisnya menjadi sangat tinggi. Sebaliknya, JSON menggunakan pasangan key-value dan tanda kurung kurawal {} sehingga sintaksnya jauh lebih ringkas. Akibatnya, JSON umumnya memiliki ukuran payload yang jauh lebih kecil dibandingkan XML untuk data yang sama.

## 2. Analisis Deserialisasi / Parsing di Sisi Klien
JSON dapat diproses oleh JavaScript menggunakan JSON.parse() atau secara otomatis melalui response.json() pada Fetch API. Hasilnya berupa objek atau array JavaScript yang dapat langsung digunakan untuk menampilkan data tanpa proses yang rumit. Sementara itu, XML biasanya harus diproses menggunakan DOMParser dengan tipe dokumen application/xml. Setelah diproses, aplikasi perlu menelusuri elemen satu per satu dan mengambil isi tag untuk mendapatkan nilai yang dibutuhkan. Ekosistem web modern banyak menggunakan JSON karena sintaksnya lebih ringkas, sangat mudah dipadukan dengan JavaScript, dan umumnya membutuhkan lebih sedikit data untuk ditransmisikan. Hal ini membuat JSON jauh lebih praktis dan efisien untuk komunikasi antara front-end dan REST API

## 3. Bottleneck Arsitektur RESTful Synchronous
Pada komunikasi sinkron, setiap klien menunggu respons dari server sampai proses yang diperlukan selesai. Jika sistem legacy membutuhkan waktu 1,5 detik untuk melakukan file locking dan menulis data, maka jika terdapat 100 klien web yang memanggil POST /api/v1/students secara bersamaan, permintaan POST lainnya akan tertunda atau mengantre ketika berebut akses ke file data.txt tersebut.
Jika jumlah permintaan terus meningkat, waktu respons akan semakin panjang. Koneksi dan thread yang menunggu dapat menggunakan sumber daya server sehingga berpotensi menimbulkan bottleneck atau kehabisan sumber daya (resource exhaustion) akibat sifat komunikasi synchronous blocking pada HTTP/1.1 yang harus antre satu-satu.