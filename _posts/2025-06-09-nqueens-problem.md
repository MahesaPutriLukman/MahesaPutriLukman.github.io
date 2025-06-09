---
title: "04 - N-Queens Problem"
date: 2025-06-09 08:00:00 +0800
categories: [DAA, N-Queens Problem]
tags: [N-Queens Problem, Backtracking, C++]
---

# Huffman Coding

## 👑 Materi 4 
**N-Queens Problem**
**Kelompok 4**
- Aditya Hisbul Bahri
- Jonas Baka
- Akhmad Hidayat
- As'syiekril Fikryan Sarda
- M. Fadhil Mulyadi

## 📌 Deskripsi Singkat
**Masalah N-Queens** adalah sebuah permasalahan klasik dalam bidang ilmu komputer dan matematika kombinatorik yang melibatkan penempatan N buah ratu catur pada sebuah papan catur berukuran N × N, sedemikian rupa sehingga tidak ada dua ratu yang saling menyerang. Dalam aturan catur, seorang ratu dapat bergerak secara horizontal (baris), vertikal (kolom), dan diagonal. Oleh karena itu, solusi dari masalah ini harus memastikan bahwa tidak ada dua ratu yang berada pada baris, kolom, atau diagonal yang sama. Contoh paling terkenal adalah masalah 8-Queens, yaitu menempatkan 8 ratu di papan 8x8.

## 🧠 Konsep Utama
- Menemukan semua konfigurasi penempatan ratu yang memenuhi syarat tidak saling menyerang.
- Mengembangkan dan menguji algoritma pencarian dan optimasi, seperti backtracking, DFS, heuristic search, atau algoritma genetika.
- Melatih logika pemrograman dan pemecahan masalah, terutama dalam konteks constraint satisfaction problem (CSP).

## 🧑‍💻 Bactracking dalam N-Queens Problem

**Backtracking**
Backtracking adalah teknik penyelesaian masalah yang mencoba membangun solusi langkah demi langkah dan "mundur" (backtrack) ketika menemui jalan buntu (dead end). Ini adalah pendekatan brute-force yang sistematis dengan optimasi, di mana solusi yang tidak valid dibuang secepat mungkin

**Backtracking dalam N-Queens Problem**
1. **Pendekatan Rekursif dan Bertahap.** 
Dalam penyelesaian N-Queens, kita menempatkan ratu satu per satu, biasanya dimulai dari baris pertama. Untuk setiap baris, kita mencoba meletakkan ratu di setiap kolom satu per satu dan memeriksa apakah posisi tersebut aman (tidak diserang oleh ratu sebelumnya). Jika aman, kita lanjut ke baris berikutnya. Jika tidak ada posisi yang aman, kita kembali ke baris sebelumnya dan mencoba posisi lain—proses inilah yang disebut backtracking.
2. **Pohon Pencarian Solusi**
Setiap penempatan ratu mewakili satu simpul dalam pohon pencarian. Jalur dari akar ke daun mewakili satu kemungkinan solusi. Jika sebuah jalur tidak bisa menghasilkan solusi yang valid, kita prune (pangkas) jalur itu dan mencoba cabang lain.
3. **Sifat Non-deterministik**
Karena ada banyak kemungkinan posisi untuk setiap ratu, dan tidak diketahui posisi yang benar sejak awal, kita harus mencoba semua kemungkinan dengan sistematis. Ini menjadikan backtracking sebagai teknik yang sangat cocok

## 📥 Input
Biasanya berupa satu angka bulat positif N, yang menyatakan ukuran papan dan jumlah ratu.

## 📤 Output
Output berupa semua solusi valid (atau satu solusi saja), dengan format yang bisa bermacam-macam:
1. Sebagai daftar posisi ratu per baris
2. Sebagai papan visual (string 2D)
3. Jumlah total solusi

## 🧮 Langkah Penyelesaian
1. Pilih Keputusan (Decision Choice)
2. Batasan (Constraint Check)
3. Rekursi (Recursive Exploration)
4. Backtrack (Kembali jika Dead End)
5. Basis Kasus (Base Case)

## 🧩 Problem and Solution Example
**CARI SEMUA PERMUTASI DARI [1, 2, 3]**

**Langkah Solusi**
1. Pilih angka pertama: 1
    - Subproblem: Cari permutasi [2, 3] → [1, 2, 3], [1, 3, 2]
    - Backtrack: Kembali ke []
2. Pilih angka pertama: 2 
    - Subproblem: Cari permutasi [1, 3] → [2, 1, 3], [2, 3, 1]
    - Backtrack: Kembali ke []
3. Pilih angka pertama: 3
    - Subproblem: Cari permutasi [1, 2] → [3, 1, 2], [3, 2, 1]
Solusi Akhir:
[[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
using namespace std;

class NQueens {
private:
    int N;
    vector<vector<string>> solutions;

    bool isSafe(int row, int col, vector<string>& board) {
        // Cek kolom atas
        for (int i = 0; i < row; i++)
            if (board[i][col] == 'Q')
                return false;

        // Cek diagonal kiri atas
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] == 'Q')
                return false;

        // Cek diagonal kanan atas
        for (int i = row - 1, j = col + 1; i >= 0 && j < N; i--, j++)
            if (board[i][j] == 'Q')
                return false;

        return true;
    }

    void solve(int row, vector<string>& board) {
        if (row == N) {
            solutions.push_back(board);
            return;
        }

        for (int col = 0; col < N; col++) {
            if (isSafe(row, col, board)) {
                board[row][col] = 'Q';     // tempatkan ratu
                solve(row + 1, board);     // lanjut ke baris berikutnya
                board[row][col] = '.';     // backtrack
            }
        }
    }

public:
    void solveNQueens(int n) {
        N = n;
        solutions.clear();
        vector<string> board(N, string(N, '.'));

        solve(0, board);

        // Tampilkan semua solusi
        cout << "Jumlah solusi: " << solutions.size() << endl;
        for (const auto& sol : solutions) {
            for (const string& row : sol)
                cout << row << endl;
            cout << endl;
        }
    }
};

int main() {
    int n;
    cout << "Masukkan nilai N: ";
    cin >> n;

    NQueens solver;
    solver.solveNQueens(n);

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Di setiap baris, kita mencoba menempatkan 1 ratu di salah satu dari N kolom → awalnya N pilihan.
- Tapi karena satu ratu per kolom dan per baris, pilihan berkurang tiap langkah.
- Maka total kemungkinan: N × (N−1) × (N−2) × ... × 1 = N!
- Kompleksitas waktu -> **O(N!)**

**Kompleksitas Ruang**
1. Papan Catur (board[N][N])
Biasanya disimpan sebagai array 2D atau array string → O(N²).
2. Stack Rekursi (karena backtracking)
Kedalaman maksimum = N (karena kita menempatkan satu ratu per baris).
Setiap panggilan menyimpan posisi saat ini dan struktur lokal → O(N).
3. Penyimpanan Semua Solusi (opsional)
Jika kita menyimpan semua solusi, dan ada S solusi, total ruang:
**O(S x N^2)**
(karena tiap solusi adalah papan N×N)

## 🌟 Aplikasi Dunia Nyata
- Penjadwalan tugas (di mana konflik harus dihindari)
- Penempatan modul dalam sirkuit elektronik
- Pengalokasian sumber daya yang saling eksklusif
- Penempatan karyawan di ruang kerja

## 💪 Kelebihan dan Kekurangan
**Kelebihan**
- Studi kasus algoritma klasik
- Mengasah kemampuan pemecahan masalah
- Mudah dipahami secara visual
- Basis untuk masalah yang lebih kompleks
- Dapat digunakan untuk benchmarking algoritma

**Kekurangan**
- Kompleksitas eksponensial
- Skalabilitas terbatas
- Masalah artifisial
- Terlalu spesifik
- Tidak semua pendekatan efisien

## 🏁 Kesimpulan
**Masalah N-Queens** memberikan pemahaman yang mendalam tentang bagaimana algoritma bekerja dalam menyelesaikan masalah pencarian dan optimasi melalui pendekatan **backtracking**. Dengan mempelajari N-Queens, kita tidak hanya memahami cara kerja rekursi dan strategi pemangkasan ruang pencarian, tetapi juga mampu menerapkan logika algoritmik dalam konteks permasalahan yang memiliki banyak kemungkinan solusi. Walaupun bersifat teoritis, N-Queens sangat berguna sebagai sarana latihan untuk membangun fondasi berpikir komputasional yang kuat, serta membuka wawasan terhadap masalah-masalah sejenis yang lebih kompleks dalam dunia nyata, seperti penjadwalan, alokasi sumber daya, dan pemrograman kendala. 