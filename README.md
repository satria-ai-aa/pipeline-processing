Penjelasan Pipeline Processing

Pipeline processing adalah sebuah teknik dalam desain prosesor yang memungkinkan beberapa instruksi dieksekusi secara bersamaan dalam tahap-tahap yang berbeda. Analogi sederhananya adalah seperti jalur perakitan mobil. Setiap mobil bergerak melalui stasiun-stasiun yang berbeda di mana tugas-tugas spesifik dilakukan (misalnya, pemasangan mesin, pemasangan roda, pengecatan). Demikian pula, dalam pipeline prosesor, setiap instruksi dibagi menjadi beberapa tahap (stage), dan saat satu instruksi berada di satu tahap, instruksi berikutnya dapat memasuki tahap sebelumnya.

Tahap-Tahap Umum dalam Pipeline Prosesor Sederhana:

Meskipun jumlah dan jenis tahap dapat bervariasi, pipeline dasar sering kali memiliki tahap-tahap berikut:

1. Instruction Fetch (IF): Mengambil instruksi dari memori.
2. Instruction Decode (ID): Menerjemahkan instruksi dan mengambil operand (data yang dibutuhkan).
3. Execute (EX): Melakukan operasi yang ditentukan oleh instruksi (misalnya, aritmatika, logika).
4. Memory Access (MEM): Mengakses memori jika instruksi melibatkan operasi memori (misalnya, load atau store).
5. Write Back (WB): Menulis hasil kembali ke register.
Bagaimana Pipeline Meningkatkan Kinerja?

Idealnya, dengan pipeline k tahap, setelah pipeline terisi penuh, satu instruksi selesai dieksekusi setiap siklus clock. Ini meningkatkan throughput (jumlah instruksi yang selesai per satuan waktu) secara signifikan dibandingkan dengan prosesor non-pipeline di mana satu instruksi harus menyelesaikan semua tahap sebelum instruksi berikutnya dimulai.
Contoh Operasi Pipeline:
Bayangkan kita memiliki tiga instruksi (I1, I2, I3) dan pipeline 5 tahap (IF, ID, EX, MEM, WB). Berikut adalah visualisasi idealnya:

| Siklus | IF1 | ID1 | EX1 | MEM1 | WB1 | IF2 | ID2 | EX2 | MEM2 | WB2 | IF3 | ID3 | EX3 | MEM3 | WB3 |
| :----- | :-- | :-- | :--- | :---- | :--- | :-- | :-- | :--- | :---- | :--- | :-- | :-- | :--- | :---- | :--- |
| 1      | I1  |     |      |       |      |     |     |      |       |      |     |     |      |       |      |
| 2      | I2  | I1  |      |       |      |     |     |      |       |      |     |     |      |       |      |
| 3      | I3  | I2  | I1   |       |      |     |     |      |       |      |     |     |      |       |      |
| 4      |     | I3  | I2   | I1    |      |     |     |      |       |      |     |     |      |       |      |
| 5      |     |     | I3   | I2    | I1   |     |     |      |       |      |     |     |      |       |      |
| 6      |     |     |      | I3    | I2   |     |     |      |       |      |     |     |      |       |      |
| 7      |     |     |      |       | I3   |     |     |      |       |      |     |     |      |       |      |

Dalam contoh ideal ini, setelah 4 siklus awal untuk mengisi pipeline, setiap siklus berikutnya satu instruksi selesai.


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

