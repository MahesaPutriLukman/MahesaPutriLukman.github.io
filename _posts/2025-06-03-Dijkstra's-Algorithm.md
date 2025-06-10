---
title: "10 - Dijkstra's Algorithm"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Dijkstra's Algorithm]
tags: [Dijkstra's Algorithm, C++]
---

# Dijkstra's Algorithm

## 🗺️ Materi 10
**Dijkstra's Algorithm**
**Kelompok 10**
- Akmal
- Muhammad Arlis
- Muhammad Hairi
- Zahra Aulia Putri

## 📌 Deskripsi Singkat
**Dijkstra's Algorithm** adalah sebuah metode yang digunakan untuk mencari jalur terpendek antara dua titik dalam sebuah graf. Graf ini bisa berupa jaringan yang terdiri dari simpul (titik) dan sisi (hubungan antar titik) dengan bobot tertentu pada sisi-sisinya. Algoritma ini membantu kita menentukan jalur terpendek dari titik awal ke titik tujuan.

## 🧠 Konsep Utama
1.  Buat tabel dan kolom sebagai tempat nilai atau jarak dari simpul, sedangkan baris sebagai posisi simpul
2.  Pilih simpul awal dan simpul tujuan, dengan syarat simpul tersebut harus terhubung secara langsung. 
3. Hitung jarak beberapa simpul tujuan yang telah dipilih.
4. Setelah semua simpul diuji, lalu pilih jarak  yang terpendek.
5.  Simpul yang dipilih akan menjadi acuan untuk tahap selanjutnya, ulangi seperti sebelumnya sampai semua simpul telah diuji

## 🧑‍💻 Konsep Backtracking dalam Khan's Algorithm
**Dijkstra’s Algorithm** adalah algoritma greedy untuk mencari jarak terpendek dari satu simpul sumber ke semua simpul lain dalam graf berbobot dan tak bertanda negatif. Meskipun tidak menggunakan backtracking eksplisit seperti DFS, konsep pelacakan jalur terpendek bisa melibatkan backtracking secara implisit. Setelah menghitung jarak minimum ke simpul tujuan, kita sering melakukan backtrack dari tujuan ke asal menggunakan informasi parent atau predecessor node. Inilah bentuk backtracking dalam Dijkstra: untuk merekonstruksi jalur terpendek dari hasil perhitungan jarak minimum.

## 📥 Input
- Graf berbobot positif:
    - Representasi: daftar adjacency dengan pasangan (node, bobot)
- Jumlah simpul (V) dan sisi (E)
- Simpul awal (source node)

## 📤 Output
- Jarak minimum dari simpul awal ke semua simpul lainnya
- Jalur terpendek dari simpul awal ke simpul tertentu (jika diminta)

## 🧮 Langkah Penyelesaian
1. Inisialisasi jarak semua simpul ke ∞ (tak hingga), kecuali sumber yang di-set ke 0
2. Gunakan priority queue (min-heap) untuk memilih simpul dengan jarak terpendek saat ini
3. Selama queue tidak kosong:
    - Ambil simpul u dengan jarak minimum
    - Untuk setiap tetangga v dari u, jika jarak[u] + bobot(u,v) < jarak[v], lakukan:
        - Perbarui jarak[v]
        - Simpan u sebagai parent[v] untuk keperluan backtracking jalur
        - Masukkan/ubah v ke dalam queue
4. (Opsional) Setelah selesai, gunakan array parent[] untuk backtrack jalur dari target ke source

## 🧩 Problem and Solution Example
Graf berbobot:

A --1--> B
A --4--> C
B --2--> C
B --6--> D
C --3--> D

Source: A

**Langkah Solusi**
1. Inisialisasi
    - dist[A]=0, dist[B]=∞, dist[C]=∞, dist[D]=∞
    - parent[A]=-1, parent[B]=-1, parent-[C]=-1, parent[D]=-1
    - Queue: [(0, A)]
2. Proses A
    - A → B: 0 + 1 < ∞ → update dist[B] = 1, parent[B] = A
    - A → C: 0 + 4 < ∞ → update dist[C] = 4, parent[C] = A
    - Queue: [(1, B), (4, C)]
3. Proses B
    - B → C: 1 + 2 = 3 < 4 → update dist[C] = 3, parent[C] = B
    - B → D: 1 + 6 = 7 → update dist[D] = 7, parent[D] = B
    - Queue: [(3, C), (4, C lama), (7, D)]
4. Proses C
    - C → D: 3 + 3 = 6 < 7 → update dist[D] = 6, parent[D] = C
    - Queue: [(4, C), (7, D lama), (6, D baru)]
5. Proses simpul lain sudah tidak mengubah apa-apa

Jarak terpendek:
A: 0
B: 1
C: 3
D: 6

Jalur A → D: A → B → C → D

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

typedef pair<int, int> pii; // (jarak, node)

void dijkstra(int V, vector<vector<pii>>& adj, int source) {
    vector<int> dist(V, INT_MAX);
    vector<int> parent(V, -1);
    priority_queue<pii, vector<pii>, greater<pii>> pq;

    dist[source] = 0;
    pq.push({0, source});

    while (!pq.empty()) {
        int d = pq.top().first;
        int u = pq.top().second;
        pq.pop();

        if (d > dist[u]) continue;

        for (auto edge : adj[u]) {
            int v = edge.first;
            int w = edge.second;

            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                parent[v] = u;
                pq.push({dist[v], v});
            }
        }
    }

    // Output jarak
    cout << "Jarak minimum dari simpul " << char('A' + source) << ":\n";
    for (int i = 0; i < V; ++i) {
        cout << char('A' + i) << ": " << dist[i] << endl;
    }

    // Backtrack: jalur dari source ke D (misalnya simpul 3)
    int target = 3; // contoh: D
    cout << "Jalur dari A ke D: ";
    vector<int> path;
    for (int at = target; at != -1; at = parent[at])
        path.push_back(at);
    reverse(path.begin(), path.end());
    for (int node : path)
        cout << char('A' + node) << " ";
    cout << endl;
}

int main() {
    int V = 4;
    vector<vector<pii>> adj(V);

    // A=0, B=1, C=2, D=3
    adj[0].push_back({1, 1}); // A->B (1)
    adj[0].push_back({2, 4}); // A->C (4)
    adj[1].push_back({2, 2}); // B->C (2)
    adj[1].push_back({3, 6}); // B->D (6)
    adj[2].push_back({3, 3}); // C->D (3)

    dijkstra(V, adj, 0); // source = A (0)
    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Dengan priority queue (min-heap):
    - O((V + E) * log V)
    - Setiap simpul diproses satu kali, dan setiap tetangga dicek dengan heap

**Kompleksitas Ruang**
- Adjacency list: O(V + E)
- Array dist[], parent[]: O(V)
- Priority queue: O(V)

## 🌟 Aplikasi Dunia Nyata
- Navigasi GPS (Google Maps, Waze)
- Rute terpendek dalam game
- Jaringan komunikasi dan routing
- Perencanaan biaya minimum (transportasi, logistik)
- Analisis jarak sosial atau jaringan relasi

## 💪 Kekuatan dan Keterbatasan
**Kekuatan**
- Menjamin jalur terpendek
- Efisien untuk banyak aplikasi
- Dapat menyelesaikan banyak tujuan sekaligus

**Keterbatasan**
- Tidak bekerja dengan bobot negatif
- Kurang efisien untuk graf besar yang jarang terhubung
- Perhitungan bisa terlalu luas

## 🏁 Kesimpulan
**Algoritma Dijkstra** merupakan salah satu algoritma pencarian jalur terpendek yang paling efisien dan banyak digunakan dalam teori graf. Algoritma ini bekerja dengan cara mencari jalur terpendek dari satu simpul sumber ke semua simpul lainnya dalam graf berbobot non-negatif. Melalui pendekatan greedy, Dijkstra memastikan bahwa setiap langkahnya mendekatkan solusi menuju hasil optimal. Keunggulan utama algoritma ini terletak pada keakuratannya dan efisiensinya, terutama jika dikombinasikan dengan struktur data seperti priority queue atau min-heap. Dalam implementasi praktis, algoritma Dijkstra sangat berguna dalam berbagai bidang seperti jaringan komputer, sistem navigasi, dan pemodelan transportasi