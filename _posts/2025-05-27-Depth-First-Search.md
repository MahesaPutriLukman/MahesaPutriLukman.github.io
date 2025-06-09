---
title: "08 - Depth-First Search (DFS)"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Depth-First Search (DFS)]
tags: [Depth-First Search (DFS), C++]
---

# Depth-First Search (DFS)

## 📶 Materi 8
**Depth-First Search (DFS)**
**Kelompok 8**
- Bismillah Ghaniyyu Putra Darmin
- Maynova Christin Gabryela Simamora
- Nabila Salsabila
- Moh. Ichwanul Muslimin Mayang

## 📌 Deskripsi Singkat
**Depth-First Search (DFS)** adalah algoritma pencarian atau penelusuran pada struktur data graf atau pohon yang bekerja dengan menjelajahi satu cabang sedalam mungkin sebelum mundur (backtrack) dan melanjutkan ke cabang berikutnya.

## 🧠 Konsep Utama
1. Masukkan simpul (node) akar ke dalam stack
2. Ambil node dari stack teratas, lalu cek apakah node tersebut merupakan solusi
    - Jika node merupakan solusi, pencarian selesai dan hasil dikembalikan
    - Jika node bukan solusi, masukkan seluruh node anak ke dalam stack
3. Jika stack kosong, dan setiap node sudah dicek, pencarian berakhir
4. Jika stack tidak kosong, ulangi pencarian mulai dari langkah ke-2

## 🧑‍💻 Konsep Backtracking dalam Depth-First Search (DFS)
**Depth-First Search (DFS)** menggunakan strategi eksplorasi sedalam mungkin sebelum mundur (backtrack) ke node sebelumnya jika menemui jalan buntu. Dalam DFS, konsep backtracking muncul secara alami, terutama saat digunakan untuk menyelesaikan masalah pencarian jalur, kombinatorial, atau eksplorasi ruang solusi (seperti Sudoku, N-Queens, dan Rat in a Maze).

DFS bekerja dengan struktur stack, baik eksplisit (menggunakan struktur data stack) atau implisit melalui rekursi. Ketika DFS mencapai node tanpa jalur ke depan, ia akan mundur ke node sebelumnya — inilah inti backtracking.

## 📥 Input
- Representasi graf, bisa berupa:
    - Matriks adjacency (graf kecil)
    - Daftar adjacency (graf besar dan sparse)
    - Matriks 2D seperti maze (untuk kasus seperti pencarian jalur)
- Titik awal (source node)
- Titik tujuan (jika ingin menemukan jalur dari source ke target)

## 📤 Output
- Urutan simpul yang dikunjungi selama DFS
- Jalur dari simpul awal ke tujuan (jika diminta)
- Status pencapaian (apakah tujuan ditemukan)

## 🧮 Langkah Penyelesaian
1. Mulai dari node awal (start node)
2. Tandai node sebagai dikunjungi
3. Lihat semua tetangga dari node
4. Untuk setiap tetangga:
    - Jika belum dikunjungi, lakukan DFS secara rekursif ke sana
    - Jika buntu (semua tetangga sudah dikunjungi/tidak bisa diakses), kembali ke node sebelumnya (backtrack)
5. Berhenti jika semua node telah dieksplorasi atau tujuan tercapai

## 🧩 Problem and Solution Example
Contoh maze (4x4):

1 1 0 1  
0 1 0 1  
1 1 1 1  
0 0 1 1

Start: (0,0)
Goal : (3,3)

**Langkah Solusi**
1. Masuk ke (0,0)
2. Coba ke kanan (0,1) ✅
3. Coba ke bawah (1,1) ✅
4. Coba ke bawah (2,1) ✅
5. Coba ke kanan (2,2) ✅
6. Coba ke kanan (2,3) ✅
7. Coba ke bawah (3,3) ✅ — tujuan tercapai

Jalur DFS: (0,0) → (0,1) → (1,1) → (2,1) → (2,2) → (2,3) → (3,3)
**Representasi arah: RDRRDD**

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int N = 4;
int maze[N][N] = {
    {1, 1, 0, 1},
    {0, 1, 0, 1},
    {1, 1, 1, 1},
    {0, 0, 1, 1}
};

bool visited[N][N]; // Penanda sel yang sudah dikunjungi
vector<pair<int, int>> path; // Menyimpan jalur menuju tujuan

// Arah gerak: kanan, bawah, kiri, atas
int dx[] = {0, 1, 0, -1};
int dy[] = {1, 0, -1, 0};

// Cek apakah posisi valid
bool isSafe(int x, int y) {
    return (x >= 0 && x < N && y >= 0 && y < N &&
            maze[x][y] == 1 && !visited[x][y]);
}

bool dfs(int x, int y) {
    // Tandai sel sekarang sebagai dikunjungi
    visited[x][y] = true;
    path.push_back({x, y});

    // Jika mencapai tujuan
    if (x == N - 1 && y == N - 1)
        return true;

    // Coba ke 4 arah
    for (int i = 0; i < 4; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];

        if (isSafe(nextX, nextY)) {
            if (dfs(nextX, nextY))
                return true;
        }
    }

    // Backtrack jika buntu
    path.pop_back();
    return false;
}

int main() {
    memset(visited, false, sizeof(visited));

    if (dfs(0, 0)) {
        cout << "Jalur ditemukan:\n";
        for (auto p : path)
            cout << "(" << p.first << ", " << p.second << ") ";
        cout << endl;
    } else {
        cout << "Tidak ada jalur yang tersedia.\n";
    }

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- DFS menjelajahi semua node dan edge
- Untuk graf dengan V simpul dan E sisi:
    - O(V + E)
- Untuk grid N x N (seperti maze), worst case:
    - O(N²) — menjelajahi semua sel yang bisa dikunjungi

**Kompleksitas Ruang**
- Menggunakan stack (eksplisit atau rekursif): O(V)
- Visited array: O(V)
- Untuk maze/grid N x N:
    - O(N²)

## 🌟 Aplikasi Dunia Nyata
- Pencarian jalur di game dan labirin
- Penyusunan urutan (Topological Sort)
- Deteksi siklus dalam graf
- Pemecahan teka-teki (Sudoku, puzzle)
- Kompresi data (misalnya saat traversal pohon Huffman)

## 💪 Kekuatan dan Keterbatasan
**Kekuatan**
- Penggunaan memori lebih efisien
- Mudah diimplementasikan
- Cocok untuk menemukan solusi kedalaman maksimal
- Digunakan dalam banyak algoritma penting

**Keterbatasan**
- Tidak menjamin solusi terbaik
- Bisa masuk ke infinite loop (pada graf siklik)
- Kurang efisien di graf luas tapi dangkal
- Tidak cocok untuk semua masalah

## 🏁 Kesimpulan
**Depth-First Search (DFS)** adalah algoritma penelusuran graf atau grid yang mengeksplorasi simpul sedalam mungkin sebelum kembali ke simpul sebelumnya. DFS secara alami menerapkan konsep backtracking, sangat cocok untuk menyelesaikan masalah pencarian jalur dan kombinatorial. Efisien digunakan dalam banyak skenario seperti maze solving, deteksi siklus, hingga logika permainan dan AI.