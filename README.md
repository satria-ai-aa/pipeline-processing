- Contoh Masalah 1: Hazard Data (Ketergantungan Data)
Bayangkan sebuah pipeline dengan instruksi-instruksi berikut:

ADD R1, R2, R3 (Tambahkan isi register R2 dan R3, simpan hasilnya di R1)
SUB R4, R1, R5 (Kurangkan isi register R5 dari R1, simpan hasilnya di R4)
Masalah: Instruksi kedua (SUB) membutuhkan hasil dari instruksi pertama (ADD) yang belum selesai ditulis kembali ke register R1. Jika pipeline mencoba menjalankan instruksi kedua segera setelah instruksi pertama masuk ke tahap eksekusi, nilai R1 yang digunakan akan salah. Ini disebut data hazard atau ketergantungan data.

Penyelesaian:

Forwarding (Bypassing): Hasil dari tahap eksekusi instruksi pertama dapat langsung diteruskan (forward) ke unit eksekusi instruksi kedua tanpa harus menunggu penulisan kembali ke register file. Ini memungkinkan instruksi kedua untuk melanjutkan eksekusi lebih awal.
Stalling (Bubbling): Jika forwarding tidak memungkinkan (misalnya, ketergantungan melalui memori), pipeline dapat dihentikan (stall) selama satu atau beberapa siklus clock. Ini menciptakan "bubble" (instruksi kosong) dalam pipeline sampai data yang dibutuhkan tersedia.
Compiler Optimization: Kompiler dapat mengatur ulang urutan instruksi (jika memungkinkan tanpa mengubah hasil program) untuk mengurangi atau menghilangkan ketergantungan data. Misalnya, menyisipkan instruksi lain yang tidak bergantung pada R1 di antara kedua instruksi tersebut.

- Contoh Masalah 2: Hazard Kontrol (Percabangan)
Pertimbangkan sebuah instruksi percabangan (misalnya, BEQ R1, R2, target) yang memeriksa apakah isi register R1 dan R2 sama. Jika sama, program akan melompat ke alamat memori target.

Masalah: Ketika instruksi percabangan sedang diproses di tahap awal pipeline (misalnya, tahap dekode), hasil perbandingan R1 dan R2 belum diketahui. Namun, pipeline sudah mulai mengambil (fetch) instruksi berikutnya berdasarkan asumsi bahwa percabangan tidak terjadi. Jika ternyata percabangan terjadi, instruksi-instruksi yang sudah terambil harus dibuang, dan pipeline harus diisi ulang dengan instruksi dari alamat target. Ini menyebabkan control hazard atau hazard percabangan.

Penyelesaian:

Stalling: Pipeline dapat dihentikan sampai hasil percabangan diketahui. Ini adalah solusi paling sederhana tetapi dapat mengurangi kinerja.
Branch Prediction: Prosesor mencoba memprediksi apakah percabangan akan terjadi atau tidak. Jika prediksi benar, pipeline dapat terus berjalan tanpa penundaan. Jika prediksi salah, pipeline harus di-flush (dikosongkan) dan diisi ulang dengan instruksi yang benar. Teknik prediksi percabangan dapat berupa:
Static Prediction: Selalu menebak percabangan tidak terjadi (not taken) atau selalu terjadi (taken).
Dynamic Prediction: Menggunakan riwayat percabangan sebelumnya untuk membuat prediksi yang lebih akurat.
Delayed Branch: Beberapa arsitektur menggunakan konsep delayed branch, di mana instruksi setelah instruksi percabangan (dalam "slot penundaan") selalu dieksekusi, terlepas dari hasil percabangan. Kompiler bertanggung jawab untuk menempatkan instruksi yang aman untuk dieksekusi di slot penundaan.

- Contoh Masalah 3: Hazard Struktural (Konflik Sumber Daya)
Bayangkan sebuah pipeline di mana tahap fetch instruksi dan akses data memori keduanya menggunakan unit memori yang sama.

Masalah: Jika pada suatu waktu pipeline mencoba melakukan fetch instruksi dan mengakses data (misalnya, untuk instruksi load atau store) secara bersamaan, akan terjadi structural hazard atau konflik sumber daya. Hanya satu operasi yang dapat menggunakan unit memori pada satu waktu.

Penyelesaian:

Memisahkan Memori: Solusi yang paling umum adalah dengan menggunakan memori terpisah untuk instruksi (instruction cache) dan data (data cache). Ini memungkinkan fetch instruksi dan akses data terjadi secara bersamaan tanpa konflik.
Stalling: Jika sumber daya yang sama harus digunakan pada waktu yang bersamaan, salah satu operasi harus dihentikan (stalled) sampai sumber daya tersebut tersedia.
Ringkasan:

Pemrosesan pipeline dapat meningkatkan kinerja dengan memungkinkan beberapa instruksi diproses secara bersamaan dalam tahap yang berbeda. Namun, ketergantungan data, percabangan, dan keterbatasan sumber daya dapat menyebabkan masalah (hazard) yang mengurangi efisiensi pipeline. Berbagai teknik seperti forwarding, stalling, prediksi percabangan, dan pemisahan sumber daya digunakan untuk mengatasi masalah-masalah ini dan memaksimalkan kinerja pipeline.

Tutorialspoint - Computer Architecture: Tutorialspoint juga menyediakan materi yang mencakup organisasi komputer dan arsitektur, termasuk topik pipelining dan hazard. Cari bagian atau tutorial yang relevan.

link: https://www.tutorialspoint.com/computer_architecture/computer_architecture_pipeline_processing.htm

dan saya juga makai Ai Gemini
