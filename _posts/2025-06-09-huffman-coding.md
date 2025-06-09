---
title: "03 - Huffman Coding"
date: 2025-06-09 08:00:00 +0800
categories: [DAA, Huffman Coding]
tags: [Huffman Coding, C++]
---

# Huffman Coding

## 🧾 Materi 3 
**Huffman Coding**
**Kelompok 3**
- Dalvyn Suhada
- Sarham San
- Hilmy Affayyad Akbar
- Ivan Ramadhan
- Gyerend Nydle Linta Mangaluk

## 📌 Deskripsi Singkat
**Huffman Coding** adalah algoritma kompresi data lossless yang dikembangkan oleh David A. Huffman pada tahun 1952. Huffman Coding igunakan untuk mengurangi ukuran data dengan cara mengganti simbol sering muncul dengan kode bit lebih pendek. Umumnya digunakan dalam format kompresi seperti ZIP, JPEG, MP3.

## 🧠 Konsep Utama
- Berdasarkan frekuensi karakter dalam data.
- Karakter dengan frekuensi tinggi → kode pendek.
- Karakter dengan frekuensi rendah → kode panjang.
- Representasi data menggunakan pohon biner.

## 🧑‍💻 Proses Pembuatan Kode
1. Hitung frekuensi tiap karakter dalam data.
2. Buat simpul untuk setiap karakter.
3. Gabungkan dua simpul dengan frekuensi terkecil.
4. Ulangi hingga terbentuk satu pohon Huffman.
5. Tentukan kode biner untuk tiap karakter (0: kiri, 1: kanan).

## 📥 Input
Biasanya terdiri dari:
1. Daftar karakter beserta frekuensinya
    Karakter: [a, b, c, d, e, f]
    Frekuensi: [5, 9, 12, 13, 16, 45]
2. String (teks) mentah
    Input: "beep boop beer!"

## 📤 Output
1. Kode Huffman untuk setiap karakter
2. String terenkripsi (encoded message)
3. Pohon Huffman (opsional)

## 🧮 Langkah Penyelesaian
1. Buat node untuk setiap karakter dan tempatkan ke dalam min-heap berdasarkan frekuensinya
2. Bangun pohon Huffman
3. Buat kode Huffman
4. Enkode string (jika input berupa teks)
5. Deskripsi (jika dibutuhkan)

## 🧩 Problem and Solution Example
**Contoh: String = "ABBCCCDDDD"**

**Langkah Solusi**
- Frekuensi:
    - A:1
    - B:2
    - C:3
    - D:4
- Bangun pohon Huffman berdasarkan frekuensi
- Hasil kode yang memungkinkan
- D = 0, C = 10, B = 110, A = 111
- Output terkompresi: lebih pendek dari representasi asli

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
using namespace std;

struct Node {
    char ch;
    int freq;
    Node *left, *right;

    Node(char c, int f) {
        ch = c;
        freq = f;
        left = right = nullptr;
    }
};

struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq;
    }
};

void generateCodes(Node* root, string code, unordered_map<char, string>& huffmanCode) {
    if (!root) return;

    if (!root->left && !root->right) {
        huffmanCode[root->ch] = code;
    }

    generateCodes(root->left, code + "0", huffmanCode);
    generateCodes(root->right, code + "1", huffmanCode);
}

void huffmanCoding(const unordered_map<char, int>& freqMap) {
    priority_queue<Node*, vector<Node*>, Compare> minHeap;

    for (auto pair : freqMap) {
        minHeap.push(new Node(pair.first, pair.second));
    }

    while (minHeap.size() > 1) {
        Node* left = minHeap.top(); minHeap.pop();
        Node* right = minHeap.top(); minHeap.pop();

        Node* merged = new Node('\0', left->freq + right->freq);
        merged->left = left;
        merged->right = right;

        minHeap.push(merged);
    }

    Node* root = minHeap.top();

    unordered_map<char, string> huffmanCode;
    generateCodes(root, "", huffmanCode);

    cout << "Kode Huffman untuk tiap karakter:\n";
    for (auto pair : huffmanCode) {
        cout << pair.first << " : " << pair.second << endl;
    }
}

int main() {
    unordered_map<char, int> freqMap = {
        {'a', 5}, {'b', 9}, {'c', 12},
        {'d', 13}, {'e', 16}, {'f', 45}
    };

    huffmanCoding(freqMap);

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Total waktu untuk membangun pohon: O(n log n)
- Traversal seluruh pohon dengan n daun → O(n)
- Jika panjang teks = L, maka encoding-nya → O(L)
- Total time complexity -> O(n log n + L)

**Kompleksitas Ruang**
- Menyimpan n node awal + n - 1 node internal → total 2n - 1 node → O(n)
- Untuk setiap karakter unik, kita simpan kodenya → O(n)
- Setiap kode bisa panjangnya hingga O(n) dalam kasus ekstrem (tapi rata-rata lebih pendek)
- Panjangnya bisa mencapai maksimal L * log n (jika semua kode sepanjang log n bit)
- Total space complexity -> O(n + L)

## 🌟 Aplikasi Dunia Nyata
- Kompresi File (ZIP, GZIP, 7z)
- Format Gambar (JPEG, PNG)
- Format Video dan Audio (MP3, MPEG, H.264)
- Transmisi Data (Telekomunikasi & Satelit)
- Sistem Informasi & Penyimpanan Dokumen

## 💪 Kelebihan dan Kekurangan
**Kelebihan**
- Lossless: tidak ada data yang hilang.
- Efisien untuk data dengan distribusi karakter tidak merata.
- Digunakan luas di sistem kompresi file.

**Kekurangan**
- Kurang optimal jika frekuensi karakter merata.
- Memerlukan tabel kode untuk dekompresi

## 🏁 Kesimpulan
**Huffman Coding** merupakan salah satu algoritma **kompresi data lossless** paling efisien dan banyak digunakan dalam berbagai aplikasi dunia nyata, seperti kompresi file (ZIP, GZIP), format multimedia (JPEG, MP3), serta transmisi data digital. Dengan pendekatan berbasis frekuensi karakter, algoritma ini menghasilkan representasi biner yang lebih pendek untuk simbol yang sering muncul, sehingga mampu mengurangi ukuran data secara signifikan tanpa kehilangan informasi. 