---
title: "07 - Breadth-First Search (BFS)"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Breadth-First Search (BFS)]
tags: [Breadth-First Search (BFS), C++]
---

# Breadth-First Search (BFS)

## 📶 Materi 7 
**Breadth-First Search (BFS)**
**Kelompok 7**
- Muh. Hanif Nurmahdin
- Kevin Anugrah Somakila'
- Moch. Syech Yusuf. M
- Raihan Ramadhan

## 📌 Deskripsi Singkat
**Breadth-First Search (BFS)** adalah metode untuk menjelajahi graph atau pohon dengan menelusuri simpul-simpul berdasarkan jaraknya dari titik awal. Secara sederhana, BFS akan mengeksplorasi semua simpul yang paling dekat terlebih dahulu, baru kemudian beralih ke simpul yang lebih jauh. Jadi pendekatannya menyebar ke seluruh arah secara merata dari pusat. Sebagai ilustrasi, bayangkan kita sedang mencari ruang kelas di sebuah gedung kampus berlantai 3. Kita pasti akan memeriksa seluruh ruangan dari lantai pertama sebelum lanjut ke lantai atas. Itulah cara kerja BFS: menjelajah secara mendatar. BFS banyak digunakan dalam berbagai aplikasi, mulai dari pencarian rute tercepat dalam game, penelusuran jaringan sosial, hingga pencarian jalur optimal dalam sistem navigasi dan peta digital

## 🧠 Konsep Utama
1. BFS menggunakan struktur queue (antrian)
2. Mulai dari simpul awal -> kunjungi semua tetangga -> lanjut ke tetangga dari tetangga
3. Bersifat level-order traversal (per lapisan)

## 📥 Input
- Representasi graf, bisa berupa:
    - Matriks adjacency (graf kecil)
    - Daftar adjacency (graf besar dan sparse)
    - Matriks 2D seperti maze (untuk kasus seperti pencarian jalur)
- Titik awal (source node)
- Titik tujuan (jika ingin menemukan jalur dari source ke target)

## 📤 Output
- Urutan simpul yang dikunjungi dalam proses pencarian
- Jalur terpendek dari source ke target (jika diminta)
- Jarak minimum dari source ke semua simpul (jika graf tak berbobot)

## 🧮 Langkah Penyelesaian
1. Tentukan simpul awal (start node)
2. Masukkan simpul (node) ke dalam antrian (queue)
3. Ambil simpul dari queue terdepan, tandai bahwa simpul telah dikunjungi, dan cek apakah merupakan solusi/tujuan
4. Selama antrian tidak kosong atau simpul aktif bukan merupakan tujuan, lakukan:
    - Ulangi proses pencarian mulai langkah ke-2 sampai ke-3
    - Keluarkan simpul dari antrian (dequeue)
    - Untuk setiap tetangga dari simpul (jika ada):
        - Kunjungi semua tetangga yang belum dikunjungi lalu tandai sebagai dikunjungi
        - Tambahkan ke antrian (enqueue)
        - Ulangi sampai simpul tujuan (goal node) ditemukan atau queue sudah kosong

## 🧩 Problem and Solution Example
Misal kita memiliki graf tak berbobot atau maze grid:
Maze (4x4):
1 1 0 1
0 1 0 1
1 1 1 1
0 0 1 1

Start: (0,0)  
Goal : (3,3)

**Langkah Solusi**
1. Tambah (0, 0) ke antrian
2. Keluarkan (0, 0), masukkan tetangga valid → (0, 1)
3. Keluarkan (0, 1), masukkan (1, 1)
4. Keluarkan (1, 1), masukkan (2, 1)
5. Keluarkan (2, 1), masukkan (2, 2)
6. Keluarkan (2, 2), masukkan (2, 3)
7. Keluarkan (2, 3), masukkan (3, 3) ✅ tujuan tercapai

Jalur terpendek: (0,0) → (0,1) → (1,1) → (2,1) → (2,2) → (2,3) → (3,3)
**Representasi arah: RDRRDD**

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

// Fungsi BFS
void bfs(int start, const vector<vector<int>>& adjList, int V) {
    vector<bool> visited(V, false);
    queue<int> q;

    // Mulai dari simpul start
    visited[start] = true;
    q.push(start);

    cout << "Urutan kunjungan BFS dari node " << start << ": ";

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        cout << node << " ";

        // Telusuri tetangga
        for (int neighbor : adjList[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }

    cout << endl;
}

int main() {
    int V = 6; // jumlah simpul
    vector<vector<int>> adjList(V);

    // Tambahkan sisi (graf tidak berbobot dan tidak berarah)
    adjList[0] = {1, 2};
    adjList[1] = {0, 3, 4};
    adjList[2] = {0, 4};
    adjList[3] = {1, 5};
    adjList[4] = {1, 2, 5};
    adjList[5] = {3, 4};

    int startNode = 0;
    bfs(startNode, adjList, V);

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- BFS mengunjungi setiap simpul dan semua tetangganya sekali saja
- Untuk graf dengan V simpul dan E sisi:
    - Kompleksitas waktu: O(V + E)
- Untuk grid N x N, waktu tergantung jumlah sel yang bisa dikunjungi:
    - O(N²) dalam kasus grid penuh (semua sel bisa dilalui)

**Kompleksitas Ruang**
- Struktur data yang digunakan:
    - Queue → bisa menyimpan hingga O(V) simpul
    - Visited array → O(V)
    - Jalur/parent tracking (opsional) → O(V)
- Untuk grid N x N, maka:
    - Kompleksitas ruang: O(N²)

## 🌟 Aplikasi Dunia Nyata
- BFS dalam labirin
- BFS di media sosial
- BFS dalam jaringan komputer
- BFS dalam transportasi dan navigasi (Google Maps, Grab, Gojek)

## 💪 Kekuatan dan Keterbatasan
**Kekuatan**
- Menjamin ditemukannya solusi yang paling baik (komplit dan optimal)

**Keterbatasan**
- Membutuhkan memori dan waktu yang cukup banyak

## 🏁 Kesimpulan
**Breadth-First Search (BFS)** adalah algoritma penelusuran graf atau pohon yang menjelajahi simpul secara berlapis, dimulai dari simpul awal. Menggunakan struktur data queue untuk memastikan semua simpul dengan jarak terdekat dijelajahi lebih dulu. BFS cocok untuk mencari jalur terpendek pada graf tak berbobot. Efektif digunakan dalam berbagai aplikasi seperti game, peta, dan pencarian data