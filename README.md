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

