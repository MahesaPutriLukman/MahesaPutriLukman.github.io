---
title: "06 - Rat in a Maze"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Rat in a Maze]
tags: [Rat in a Maze, Backtracking, C++]
---

# Rat in a Maze

## 🐀 Materi 6 
**Rat in a Maze**
**Kelompok 6**
- Rizky Nur Fariid
- Muh. Anugrah Ashary P.
- Azizah Nurul Izzah
- Nur Atika Binti Ardi
- Shabrina Zahrah R.

## 📌 Deskripsi Singkat
**Rat in a Maze** merupakan salah satu algorithm problem di mana seekor tikus (rat) harus keluar dari sebuah labirin (maze) dengan mengikuti jalur tertentu, dari titik awal ke titik tujuan. Labirin tersebut diwakili oleh sebuah matriks atau grid yang terdiri dari sel-sel yang bisa dilewati atau terhalang. Solusi untuk masalah ini dapat menggunakan algoritma backtracking, di mana tikus mencoba bergerak satu per satu dalam langkah-langkah dan mundur (backtrack) jika jalan yang dipilih tidak berhasil

## 🧠 Konsep Utama
1. Tikus akan mencoba menemukan jalur dari posisi awal ke tujuan di dalam sebuah labirin berbentuk matriks
2. Tikus akan mencoba satu langkah ke suatu arah (kanan, bawah, kiri, atau atas)
3. Jika langkah tersebut valid (tidak keluar dari labirin dan tidak menabrak dinding), maka tikus lanjut ke sel tersebut
4. Jika dari posisi baru tidak ada jalan ke tujuan, ia mundur (backtrack) ke posisi sebelumnya dan mencoba arah lain

## 🧑‍💻 Konsep Bactracking
**Backtracking** adalah strategi yang sangat cocok untuk menyelesaikan masalah **Rat in a Maze** karena sifatnya yang berupa pencarian dan pengambilan keputusan. Dalam konteks tikus di labirin, konsep backtracking bekerja seperti ini: tikus memulai dari titik awal dan mencoba jalur yang berbeda untuk mencapai titik akhir. Setiap kali tikus bergerak ke sel baru, itu adalah "keputusan" yang diambil. Jika tikus mencapai sel yang buntu (misalnya, dinding, atau sel yang sudah pernah dikunjungi dalam jalur yang sama untuk menghindari putaran tak terbatas), atau jika jalur yang diambil tidak mengarah ke tujuan, maka tikus akan "mundur" (backtrack) ke sel sebelumnya dan mencoba jalur atau keputusan lain dari sel tersebut.

Proses ini berlanjut secara rekursif: tikus menandai sel yang sudah dikunjunginya (seringkali dengan nilai tertentu dalam matriks labirin) agar tidak mengunjungi sel yang sama berulang kali dalam satu jalur. Jika tikus berada di sel (x,y) dan mencoba bergerak ke empat arah (atas, bawah, kiri, kanan), ia akan memilih satu arah, misalnya ke (x′, y′). Jika (x′ ,y′) adalah sel yang valid (bukan dinding, belum dikunjungi), tikus akan bergerak ke sana dan mencoba mencari jalan dari (x′, y′). Jika dari (x′ ,y′) tikus tidak bisa mencapai tujuan (semua jalurnya buntu atau sudah dikunjungi), maka tikus akan kembali ke (x,y) (inilah inti dari backtracking), menghapus tanda bahwa (x′, y′) adalah bagian dari jalur yang berhasil (jika jalur ini gagal), dan mencoba arah lain dari (x,y). Proses "maju-mundur" ini terus berulang sampai tikus menemukan jalur menuju tujuan, atau semua kemungkinan jalur telah dicoba dan tidak ada solusi ditemukan.

## 📥 Input
- Sebuah matriks 2D maze[N][N] yang merepresentasikan peta labirin.
- Elemen bernilai 1 artinya jalan bisa dilalui, sedangkan 0 artinya tembok atau jalan buntu.
- Tikus mulai dari kiri atas (0, 0) dan ingin mencapai kanan bawah (N-1, N-1).

## 📤 Output
Jalur dari titik awal ke tujuan, biasanya dalam bentuk:
- Matriks solusi (1 jika jalur dilewati tikus)
- Daftar koordinat lintasan
- Jumlah semua solusi yang mungkin (jika diminta mencari semua jalur)

## 🧮 Langkah Penyelesaian
1. Mulai dari (0, 0)
    - Pastikan bahwa posisi ini adalah jalan (1), bukan tembok
2. Lakukan eksplorasi ke arah yang valid:
    - Biasanya: bawah (↓) dan kanan (→)
    - Bisa juga ke semua arah: atas (↑), kiri (←) jika eksplorasi total
3. Cek batasan (constraint):
    - Tidak keluar dari area labirin
    - Tidak melewati sel yang bernilai 0 atau sudah dikunjungi
4. Tandai posisi sebagai bagian dari solusi, lalu lanjutkan ke rekursi
5. Jika mencapai tujuan (N-1, N-1), simpan solusi
6. Backtrack jika jalur tersebut tidak membawa ke solusi → tandai kembali jadi 0

## 🧩 Problem and Solution Example
Seekor tikus yang ditempatkan di (0, 0) dalam matriks persegi berorde N * N. Tikus harus mencapai tujuan di (N — 1, N — 1) . Temukan semua kemungkinan jalur yang dapat diambil tikus untuk mencapai dari sumber ke tujuan. Dalam satu jalur, tidak ada sel yang dapat dikunjungi lebih dari satu kali. Nilai 0 pada sel dalam matriks menunjukkan bahwa sel tersebut terhalang dan tikus tidak dapat bergerak ke sana, sedangkan nilai 1 pada sel dalam matriks menunjukkan bahwa tikus dapat melewatinya

**Langkah Solusi**
1. Langkah 1: Coba gerakan ke arah
    - Bawah (D) → ke (1, 0)  = 1 ✅
    - Kanan (R) → ke (0, 1) = 0 ❌
2. Langkah 2: Dari (1, 0)
    - Tandai (1, 0)
    - Pilih arah:
        - Bawah (D) → (2, 0) = 1 ✅
        - Kanan (R) → (1, 1) = 1 ✅
    - Pilih salah satu  : R
3. Langkah 3: Dari (1, 1)
    - Pilih arah:
        - Bawah (D) → (2, 1) = 1 ✅
4. Langkah 4: Dari (2, 1)
    - Pilih arah:
        - Bawah (D) → (3, 1) = 1 ✅
5. Langkah 5: Dari (3, 1)
    - Pilih arah:
        - Kanan (R) → (3, 2) = 1 ✅
6. Langkah 6: Dari (3, 2)
    - Pilih arah:
        - Kanan (R) → (3, 3) = 1 ✅
**Representasi arah: DRDDRR**

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int N = 4;

// Fungsi untuk mengecek apakah langkah valid
bool isSafe(int maze[N][N], int x, int y) {
    return (x >= 0 && x < N && y >= 0 && y < N && maze[x][y] == 1);
}

// Fungsi rekursif untuk menyelesaikan maze
bool solveMazeUtil(int maze[N][N], int x, int y, int sol[N][N]) {
    // Jika (x,y) adalah tujuan
    if (x == N - 1 && y == N - 1 && maze[x][y] == 1) {
        sol[x][y] = 1;
        return true;
    }

    // Cek apakah maze[x][y] adalah langkah valid
    if (isSafe(maze, x, y)) {
        // Tandai bagian dari solusi
        sol[x][y] = 1;

        // Bergerak ke bawah
        if (solveMazeUtil(maze, x + 1, y, sol))
            return true;

        // Bergerak ke kanan
        if (solveMazeUtil(maze, x, y + 1, sol))
            return true;

        // Jika tidak berhasil, backtrack
        sol[x][y] = 0;
        return false;
    }

    return false;
}

// Fungsi utama
void solveMaze(int maze[N][N]) {
    int sol[N][N] = { {0} };

    if (!solveMazeUtil(maze, 0, 0, sol)) {
        cout << "Tidak ada solusi!" << endl;
        return;
    }

    // Cetak solusi
    cout << "Solusi Maze:" << endl;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++)
            cout << sol[i][j] << " ";
        cout << endl;
    }
}

// Main program
int main() {
    int maze[N][N] = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {0, 1, 0, 0},
        {1, 1, 1, 1}
    };

    solveMaze(maze);
    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Pada setiap sel (i, j), tikus memiliki hingga 4 arah kemungkinan untuk melangkah (atas, bawah, kiri, kanan).
- Dalam kasus terburuk, tikus bisa mencoba semua sel satu per satu (tergantung ukuran maze dan konfigurasi jalan/tembok).
- Untuk maze berukuran N x N, jumlah sel maksimum adalah N².
- Maka, kompleksitas waktu terburuk adalah:
    - O(4^(N×N)) — karena setiap sel bisa bercabang ke 4 arah (rekursi eksponensial).
    - Jika hanya dua arah (kanan dan bawah), kompleksitasnya lebih kecil: O(2^(N+N)) = O(2^(2N))

**Kompleksitas Ruang**
- Solusi menggunakan rekursi → stack rekursi akan memakan ruang sesuai kedalaman pencarian:
    - Kedalaman maksimal bisa mencapai N² (seluruh jalur).
    - Solusi disimpan dalam array N x N → O(N²)
- Maka total kompleksitas ruang adalah: O(N²)

## 🌟 Aplikasi Dunia Nyata
- Robot Navigasi
- Pencarian Jalur di GPS atau Navigasi
- Simulasi dan Permainan

## 💪 Kekuatan dan Keterbatasan
**Kekuatan**
- Sederhana dan Mudah Diimplementasikan
- Menemukan Semua Solusi
- Fleksibel
- Tidak Memerlukan Struktur Data Kompleks

**Keterbatasan**
- Kurang efisien dalam Kasus Besar
- Risiko Stack Overflow
- Tidak Optimal
- Mengulang Jalur yang Sama Jika Tidak Diatur

## 🏁 Kesimpulan
**Rat in a Maze** memberikan pemahaman yang mendalam tentang bagaimana algoritma backtracking bekerja untuk menyelesaikan masalah pencarian jalur. Dengan menerapkan prinsip coba-coba dan mundur ketika menemui jalan buntu, kita belajar menyusun solusi yang sistematis dan efisien untuk menjelajahi ruang kemungkinan yang kompleks. Selain itu, materi ini memperkenalkan konsep penting seperti rekursi, constraint checking, dan eksplorasi ruang solusi, yang sangat relevan dalam pengembangan algoritma pencarian jalan di bidang robotika, AI, dan game development. Secara keseluruhan, Rat in a Maze bukan hanya tentang menemukan jalan keluar dari labirin, tetapi juga tentang membangun pola pikir algoritmik yang kuat dan terstruktur.