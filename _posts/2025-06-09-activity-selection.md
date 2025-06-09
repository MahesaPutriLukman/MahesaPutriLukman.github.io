---
title: "01 - Activity Selection Problem"
date: 2025-06-09 08:00:00 +0800
categories: [DAA, Greedy Algorithm]
tags: [Activity Selection, Greedy, C++]
---

# Activity Selection Problem

## 📌 Deskripsi Singkat
Activity Selection Problem adalah masalah optimasi yang bertujuan untuk memilih sebanyak mungkin aktivitas yang **tidak saling tumpang tindih**, berdasarkan waktu mulai dan selesai.

## 🧠 Konsep Utama
- Termasuk dalam kategori **Greedy Algorithm**
- Strategi: selalu pilih aktivitas yang selesai paling awal
- Cocok untuk masalah penjadwalan yang efisien

## 📥 Input
- Dua array: waktu mulai dan waktu selesai dari setiap aktivitas

## 📤 Output
- Indeks aktivitas yang dapat dipilih tanpa tumpang tindih

## 🧮 Langkah Penyelesaian
1. Urutkan aktivitas berdasarkan waktu selesai
2. Pilih aktivitas pertama (paling awal selesai)
3. Pilih aktivitas berikutnya hanya jika waktu mulai ≥ waktu selesai aktivitas terakhir yang dipilih

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