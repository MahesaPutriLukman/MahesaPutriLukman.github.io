---
title: "01 - Activity Selection Problem"
date: 2025-06-09 08:00:00 +0800
categories: [DAA, Greedy Algorithm]
tags: [Activity Selection, Greedy, C++]
---

# Activity Selection Problem

## 🧠 Materi 1
**Activity Selection Problem**
**Kelompok 1**
- Ahmad Hidayat
- Julio Rema Palotongan
- Suci Sri Aulia
- Naailah Mazaya
- Yud Bryawan

## 📌 Deskripsi Singkat
Activity Selection Problem adalah masalah optimasi yang bertujuan untuk memilih sebanyak mungkin aktivitas yang **tidak saling tumpang tindih**, berdasarkan waktu mulai dan selesai. Diberikan sejumlah aktivitas yang masing-masing didefinisikan dengan **waktu mulai (s)** dan **waktu selesai (f)**. Dua aktivitas dikatakan kompatibel jika mereka **tidak tumpang tindih secara waktu**, artinya salah satu aktivitas selesai sebelum aktivitas lainnya dimulai. 

## 🧠 Konsep Utama
- Termasuk dalam kategori **Greedy Algorithm**
- Strategi: selalu pilih aktivitas yang selesai paling awal
- Cocok untuk masalah penjadwalan yang efisien

## 💻 Konsep Algoritma Greedy
**Algoritma Greedy** adalah strategi pemecahan masalah optimasi yang bekerja dengan membuat pilihan yang tampak paling baik pada setiap langkah, dengan harapan rangkaian pilihan ini akan mengarahkan pada solusi yang optimal secara keseluruhan. Algoritma ini bersifat **serakah** karena langsung mengambil pilihan terbaik saat itu tanpa mempertimbangkan konsekuensi di masa depan.

Dalam konteks **Activity Selection Problem**, algoritma greedy secara efektif diterapkan dengan memilih aktivitas yang memiliki waktu selesai paling awal di setiap langkah.

## 📥 Input
- Dua array: waktu mulai dan waktu selesai dari setiap aktivitas

## 📤 Output
- Indeks aktivitas yang dapat dipilih tanpa tumpang tindih

## 🧮 Langkah Penyelesaian
1. Urutkan aktivitas berdasarkan waktu selesai (finish time)
2. Pilih aktivitas pertama (paling awal selesai)
3. Pilih aktivitas berikutnya hanya jika waktu mulai ≥ waktu selesai aktivitas terakhir yang dipilih

## 🧮 Problem and Solution Example
**Aktivitas, Mulai (s), Selesai (f)**
- A1, 1, 4
- A2, 3, 5
- A3, 0, 6
- A4, 5, 7
- A5, 8, 9
- A6, 5, 9

**Langkah Solusi**
1. **Urutkan** berdasarkan waktu selesai: [A1, A2, A3, A4, A5, A6]
2. **Pilih A1** (selesai paling awal)
3. **Iterasi:**
    - A2, A3: Tumpang tindih
    - A4: Kompatibel -> {A1, A4}
    - A5: Kompatibel -> {A1, A4, A5}
    - A6: Tumpang tindih
**Solusi Optimal:** {A1, A4, A5} (3 aktivitas)

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Activity {
    int start, finish, index;
};

bool compare(Activity a, Activity b) {
    return a.finish < b.finish;
}

void activitySelection(vector<Activity>& activities) {
    sort(activities.begin(), activities.end(), compare);

    cout << "Aktivitas terpilih (index): ";
    int lastFinish = -1;

    for (auto act : activities) {
        if (act.start >= lastFinish) {
            cout << act.index << " ";
            lastFinish = act.finish;
        }
    }
    cout << endl;
}

int main() {
    vector<Activity> activities = {
        {1, 2, 0}, {3, 4, 1}, {0, 6, 2},
        {5, 7, 3}, {8, 9, 4}, {5, 9, 5}
    };

    activitySelection(activities);
    return 0;
}
```

## 🧠 Analisis Kompleksitas
**Kompleksitas Waktu**
- Pengurutan aktivitas berdasarkan waktu selesai: O(n log n)
- Pemilihan aktivitas: O(n)
- Total kompleksitas waktu: O(n log n)

**Kompleksitas Ruang**
- Menyimpan aktivitas: O(n)
- Tidak memerlukan ruang tambahan yang signifikan untuk menyimpan input dan output

## 📌 Aplikasi Dunia Nyata
- Penjadwalan & Fasilitas (Jadwal ruang kelas, meeting, lab, olahraga)
- Sistem Operasi (Jadwal proses CPU, alokasi memori)
- Logistik (Jadwal pengiriman, rute kendaraan)
- Telekomunikasi (Alokasi bandwidth, jadwal transmisi data)

## Kekuatan dan Keterbatasan
**Kekuatan**
- Sederhana dan mudah diimplementasikan
- Efisien untuk dataset besar
- Memberikan solusi optimal dalam kasus tanpa batasan kompleks

**Keterbatasan**
- Membutuhkan proses pengurutan awal
- Tidak cocok untuk masalah dengan banyak tambahan (seperti prioritas, jarak, atau biaya)

## Kesimpulan
**Activity Selection Problem** adalah masalah optimasi untuk memilih aktivitas sebanyak mungkin tanpa tumpang tindih. Diselesaikan dengan **Algoritma Greedy** yang mengurutkan aktivitas berdasarkan waktu selesai, menghasilkan solusi optimal dengan efisiensi O(n log n). Cocok untuk aplikasi penjadwalan, namun untuk kasus kompleks dibutuhkan metode lain seperti pemrograman dinamis atau metaheuristik.