---
title: "09 - Khan's Algorithm"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Khan's Algorithm]
tags: [Khan's Algorithm, C++]
---

# Khan's Algorithm

## 🗺️ Materi 9
**Khan's Algorithm**
**Kelompok 9**
- Nurul Marisa Clara Waldi
- Agung Allo Karaeng
- Ahmad Rizky Amis
- Mahesa Putri Lukman

## 📌 Deskripsi Singkat
**Kahn’s Algorithm** adalah algoritma penyusunan topologi (topological sorting) yang digunakan untuk menentukan urutan dalam sebuah Directed Acyclic Graph (DAG) berdasarkan ketergantungan antar simpul. Algoritma Kahn adalah pendekatan berbasis BFS untuk menemukan urutan node yang valid dalam Directed Acyclic Graph (DAG).  Urutan topologi DAG adalah urutan dimana untuk setiap tepi yang diarahkan u → v, u muncul sebelum v dalam urutan.

## 🧠 Konsep Utama
1. Gunakan struktur in-degree (jumlah sisi masuk) untuk tiap simpul
2. Tambahkan semua simpul dengan in-degree = 0 ke antrean (queue)
3. Proses antrean satu per satu
4. Jika semua simpul telah diproses, maka graf tidak memiliki siklus

## 🧑‍💻 Konsep Backtracking dalam Khan's Algorithm
Meskipun **Khan’s Algorithm** secara teknis tidak menggunakan backtracking seperti pada DFS atau algoritma eksploratif lain, ia tetap menyelesaikan masalah dependensi secara incremental dan selektif. Dalam konteks topological sorting, pemilihan simpul dengan in-degree 0 bisa dianggap sebagai “langkah maju”, dan jika tidak ada simpul dengan in-degree 0 tersisa namun graf belum selesai, maka kita bisa menganggap ini sebagai kondisi “buntu” — yang dalam konteks lain akan diikuti dengan backtrack. Namun, karena Khan’s Algorithm bersifat greedy dan berbasis queue (BFS-style), maka backtracking eksplisit tidak terjadi.

Namun, bila Khan's Algorithm gagal (misalnya karena siklus), maka pendeteksian kegagalan ini menyerupai backtrack, yaitu kondisi tidak ada solusi valid dan perlu “kembali” meninjau struktur graf untuk perbaikan.

## 📥 Input
- Sebuah graf berarah dalam bentuk:
    - Matriks adjacency (jika graf kecil)
    - Daftar adjacency (jika graf besar/sparse)
- Jumlah simpul (V) dan jumlah sisi (E)
- Setiap sisi merepresentasikan ketergantungan (misalnya: A → B artinya A harus dilakukan sebelum B)

## 📤 Output
- Urutan simpul secara topologis (urutan valid berdasarkan dependensi)
- Jika ada siklus, maka output menyatakan "Graf mengandung siklus, tidak bisa dilakukan topological sort."

## 🧮 Langkah Penyelesaian
1. Hitung in-degree untuk setiap simpul
2. Masukkan semua simpul dengan in-degree 0 ke dalam queue
3. Selama queue tidak kosong:
    - Ambil simpul dari depan queue dan tambahkan ke hasil urutan topologis
    - Kurangi in-degree dari semua tetangganya
    - Jika in-degree tetangga menjadi 0, tambahkan ke queue
4. Jika semua simpul telah diambil dari queue (jumlahnya = jumlah simpul di graf), maka topological sort berhasil
5. Jika tidak, berarti ada siklus dalam graf

## 🧩 Problem and Solution Example
Misalnya kita punya dependensi antar mata kuliah:

A -> C
B -> C
C -> D

- A dan B adalah prasyarat untuk C
- C adalah prasyarat untuk D

**Langkah Solusi**
1. In-degree awal:
    - A: 0, B: 0, C: 2, D: 1
2. Masukkan A dan B ke queue
3. Proses A → kurangi in-degree C (jadi 1)
4. Proses B → kurangi in-degree C (jadi 0) → masukkan C ke queue
5. Proses C → kurangi in-degree D (jadi 0) → masukkan D ke queue
6. Proses D → selesai

**Urutan topologis valid: A, B, C, D**
(Catatan: A dan B bisa ditukar karena keduanya tidak saling bergantung)

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <set>
#include <algorithm>
using namespace std;

unordered_map<char, vector<char>> graph;
unordered_map<char, int> in_degree;
set<char> vertices;

void allTopologicalSorts(vector<char> &res, vector<bool> &visited) {
    bool flag = false;
    for (char v : vertices) {
        if (!visited[v] && in_degree[v] == 0) {
            // Pilih simpul v
            visited[v] = true;
            res.push_back(v);

            // Kurangi in-degree tetangga
            for (char neighbor : graph[v]) {
                in_degree[neighbor]--;
            }

            // Rekursi
            allTopologicalSorts(res, visited);

            // Backtrack
            visited[v] = false;
            res.pop_back();
            for (char neighbor : graph[v]) {
                in_degree[neighbor]++;
            }

            flag = true;
        }
    }

    if (!flag) {
        for (char c : res) cout << c << " ";
        cout << endl;
    }
}

int main() {
    int E;
    cout << "Masukkan jumlah edge (sisi): ";
    cin >> E;

    cout << "Masukkan sisi dalam format 'A B' (tanpa panah):" << endl;
    for (int i = 0; i < E; ++i) {
        char u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        in_degree[v]++;
        vertices.insert(u);
        vertices.insert(v);
    }

    for (char v : vertices) {
        if (in_degree.find(v) == in_degree.end()) {
            in_degree[v] = 0;
        }
    }

    vector<char> res;
    unordered_map<char, bool> visited_map;
    for (char v : vertices) {
        visited_map[v] = false;
    }

    vector<bool> visited(256, false); // asumsi char
    allTopologicalSorts(res, visited);

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Hitung in-degree semua simpul: O(V + E)
- Proses semua simpul dan sisi satu kali: O(V + E)
- Total kompleksitas waktu: O(V + E)

**Kompleksitas Ruang**
- Menyimpan graf (adjacency list): O(V + E)
- Menyimpan in-degree array: O(V)
- Menyimpan queue dan hasil urutan: O(V)
- Total kompleksitas ruang: O(V + E)

## 🌟 Aplikasi Dunia Nyata
- Penjadwalan tugas
- Kompilasi program
- Prasyarat mata kuliah

## 💪 Kekuatan dan Keterbatasan
**Kekuatan**
- Kompleksitas waktu O(V + E)
- Deteksi siklus
- Masalah dependency

**Keterbatasan**
- Graf siklus
- Output lebih dari satu solusi
- Memerlukan struktur data tambahan

## 🏁 Kesimpulan
**Kahn’s Algorithm** merupakan metode yang efisien dan sistematis untuk melakukan topological sorting pada Directed Acyclic Graph (DAG). Dengan memanfaatkan struktur in-degree dan antrean, algoritma ini mampu menyusun simpul-simpul berdasarkan ketergantungan yang ada, serta mendeteksi keberadaan siklus dalam graf. Implementasinya dalam bahasa C++ memperlihatkan bagaimana struktur data seperti unordered_map, queue, dan vector dapat digunakan untuk memodelkan graf dan proses penyusunannya secara elegan. Dengan kompleksitas waktu O(V + E), algoritma ini sangat berguna dalam berbagai aplikasi nyata, seperti penjadwalan tugas, kompilasi program, dan pengelolaan prasyarat mata kuliah.