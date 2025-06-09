---
title: "02 - Factional Knapsack"
date: 2025-05-06 08:00:00 +0800
categories: [DAA, Fractional Knapsack]
tags: [Fractional Knapsack, algoritma greedy, C++]
---

# Fractional Knapsack

## 🎒 Materi 2 
**Fractional Knapsack**
**Kelompok 2**
- Zalfa Syauqiyah Hamka
- Davidzen
- Maisyah Mahdiyyah
- Muhammad Fadel Aryasatya Makkulau
- Hezekiah Reynard Tikupadang

## 📌 Deskripsi Singkat
**Knapsack Problem** adalah sebuah masalah optimasi di mana kita harus memilih barang-barang dari sekumpulan item agar nilai totalnya maksimal tanpa melebihi kapasitas berat ransel yang tersedia. Ada dua variasi utama dari masalah ini, yaitu **0/1 Knapsack** dan **Fractional Knapsack**. Pada 0/1 Knapsack, kita hanya bisa memilih **seluruh barang atau tidak sama sekali**, sehingga barang tidak boleh dipotong-potong. Sedangkan pada Fractional Knapsack, kita diperbolehkan mengambil **sebagian dari barang tersebut**, sehingga barang bisa **dipotong sesuai kebutuhan** agar mendapatkan **nilai maksimal**. Perbedaan ini membuat 0/1 Knapsack menjadi lebih kompleks dan biasanya diselesaikan menggunakan metode pemrograman dinamis, sementara Fractional Knapsack dapat diselesaikan dengan algoritma greedy yang lebih sederhana dan efisien.

## 🧠 Konsep Utama
- Setiap barang memiliki berat dan nilai.
- Barang dapat diambil sebagian sesuai kebutuhan.
- Solusi dapat diselesaikan dengan algoritma greedy.

## 🧑‍💻 Konsep Algoritma Greedy
**Algoritma Greedy** adalah strategi pemecahan masalah optimasi yang bekerja dengan membuat pilihan yang tampak paling baik pada setiap langkah, dengan harapan rangkaian pilihan ini akan mengarahkan pada solusi yang optimal secara keseluruhan. Algoritma ini bersifat **serakah** karena langsung mengambil pilihan terbaik saat itu tanpa mempertimbangkan konsekuensi di masa depan.

Dalam konteks **Activity Selection Problem**, algoritma greedy merupakan pendekatan untuk memilih opsi terbaik saat ini demi mencapai solusi optimal. Karakteristik utama: sederhana, efisien, tidak perlu mempertimbangkan semua kemungkinan. Tapi algoritma greedy tidak selalu menghasilkan solusi optimal global untuk semua kasus. Kadang pilihan terbaik lokal menghalangi solusi optimal secara keseluruhan.

## 📥 Input
Biasanya terdiri dari:
- n: jumlah item
- W: kapasitas maksimum tas (dalam satuan berat)
- Daftar item yang berisi:
    - value[i]: nilai item ke-i
    - weight[i]: berat item ke-i

## 📤 Output
- Total nilai maksimum yang bisa dibawa (max_value)
- Bisa juga menampilkan persentase atau bagian dari item yang diambil.

## 🧮 Langkah Penyelesaian
1. Hitung rasio nilai per berat (value/weight) untuk setiap item.
2. Urutkan barang berdasarkan rasio tertinggi.
3. Ambil sebanyak mungkin dari item dengan rasio tertinggi hingga tas penuh.
4. Jika tidak bisa mengambil seluruh item, ambil sebagian.

## 🧩 Problem and Solution Example
**Diketahui ada 4 item dengan data berikut:**
**Item, Nilai (Value), Berat (Weight)**
- A, 50, 10
- B, 60, 20
- C, 140, 40
- D, 60, 30

Kapasitas tas (W) = 50

Pertanyaan: Berapa **nilai maksimum** yang bisa dibawa dalam tas?

**Langkah Solusi**
1. Hitung rasio value/weight:
    - A:5.0
    - B:3.0
    - C:3.5
    - D:2.0
2. Urutkan: A(5.0), C(3.5), B(3.0), D(2.0)
3. Isi tas berdasarkan kapasitas:
    - Ambil A (10kg, nilai 50) -> sisa 40
    - Ambil C (40kg, nilai 140) -> sisa 0
    - B dan D tidak diambil
**Total nilai maksimum = 50 + 140 = 190**

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Item {
    string name;
    double weight;
    double value;

    double ratio() const {
        return value / weight;
    }
};

bool compare(Item a, Item b) {
    return a.ratio() > b.ratio();
}

int main() {
    double capacity = 30.0; 
    vector<Item> items = {
        {"Laptop", 10, 300},
        {"Buku Paket", 20, 200},
        {"Baju", 30, 180}
    };

    sort(items.begin(), items.end(), compare);

    double totalValue = 0.0;
    double totalWeight = 0.0;

    cout << "Barang yang dipilih:\n";
    for (const auto& item : items) {
        if (capacity == 0) break;

        if (item.weight <= capacity) {
            totalValue += item.value;
            capacity -= item.weight;
            cout << "- " << item.name << " (berat: " << item.weight << " kg, nilai: " << item.value << ")\n";
        } else {
            double fraction = capacity / item.weight;
            totalValue += item.value * fraction;
            cout << "- " << item.name << " (berat: " << capacity << " kg dari " << item.weight << " kg, nilai: " << item.value * fraction << ")\n";
            capacity = 0;
        }
    }

    cout << "\nTotal nilai yang dibawa: " << totalValue << endl;

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Menghitung rasio value/weight untuk setiap item → O(n)
- Mengurutkan item berdasarkan rasio tersebut → O(n log n)
- Mengisi knapsack dengan iterasi atas item (ambil penuh atau sebagian) → O(n)
- Total time complexity -> O(n log n)

**Kompleksitas Ruang**
- Array/list untuk menyimpan n item beserta value, weight, dan ratio: O(n)
- Tidak ada penggunaan struktur tambahan yang kompleks atau rekursi.
- Total space complexity -> O(n)

## 🌟 Aplikasi Dunia Nyata
- Pengangkutan barang di logistik
- Pengisian tangki minyak atau bahan bakar
- Investasi portofolio
- Penggunaan bandwidth atau CPU dalam Cloud Computing

## 💪 Kelebihan dan Kekurangan
**Kelebihan**
- Solusi optimal dijamin
- Cepat dan efisien
- Fleksibel
- Mudah diimplementasikan

**Kekurangan**
- Tidak cocok untuk masalah 0/1
- Tidak menangani batasan lain
- Hanya maksimalkan nilai, bukan prioritas lain

## 🏁 Kesimpulan
**Fractional Knapsack** adalah salah satu variasi dari **Knapsack Problem** yang dapat diselesaikan secara efisien menggunakan **algoritma greedy**. Dalam versi ini, barang boleh **diambil sebagian (fraksional)**, berbeda dengan 0/1 Knapsack yang hanya membolehkan **pengambilan penuh atau tidak sama sekali**. Dengan menghitung rasio nilai terhadap berat **(value/weight)** untuk setiap barang, lalu mengambil barang dengan rasio tertinggi hingga kapasitas tas penuh, kita bisa mendapatkan solusi optimal secara cepat dan sederhana. Pendekatan greedy ini bekerja efektif karena pilihan optimal lokal (berdasarkan rasio) juga mengarah pada **solusi optimal** global dalam kasus Fractional Knapsack.