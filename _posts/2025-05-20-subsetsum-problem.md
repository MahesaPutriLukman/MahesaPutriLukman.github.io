---
title: "05 - Subset Sum Problem"
date: 2025-05-20 08:00:00 +0800
categories: [DAA, Subset Sum Problem]
tags: [Subset Sum Problem, Backtracking, C++]
---

# Subset Sum Problem

## 🧨 Materi 5 
**Subset Sum Problem**
**Kelompok 5**
- Ishmah Nurwasilah
- Trivania Buli Karoma
- Diani Annisah
- Diesty Mendila Tappo
- Muhammad Fatir Syabhan

## 📌 Deskripsi Singkat
**Subset Sum Problem** adalah masalah klasik dalam ilmu komputer yang termasuk dalam kategori NP-Complete. Diberikan sebuah himpunan bilangan bulat dan sebuah nilai target, tugasnya adalah menentukan apakah ada subset dari himpunan tersebut yang jumlah elemennya sama dengan nilai target. Subset Sum Problem (SSP) termasuk dalam decision problem, yaitu jenis masalah yang hanya membutuhkan jawaban “ya” atau “tidak”. Inti dari masalah ini adalah mencari apakah mungkin memilih sejumlah angka dari sekumpulan bilangan bulat sehingga totalnya pas sama dengan angka target. 

## 🧠 Variasi Masalah Subset Sum Problem
- Unbounded Subset Sum Problem
- Multi-target Subset Sum Problem
- Approximate Subset Sum

## 🧑‍💻 Pendekatan Subset Sum Problem

**Pendekatan Rekursif (Rercursive Approach)**
Cek semua subset secara eksplisit.
- Untuk setiap elemen:
✅ Sertakan ATAU ❌ Abaikan.
- Basis kasus:
    - Jika sum == 0 → ✅ True
    - Jika n == 0 && sum != 0 → ❌ False
Kompleksitas: O(2ⁿ)
Tidak efisien untuk array besar

**Dynamic Programming Approach**
1. **Memoization (Top-Down DP)** 
Kombinasi rekursi + penyimpanan hasil submasalah.
- 📦 Gunakan struktur dp[n][sum] → cache hasil.
- 📉 Kurangi pemanggilan ulang fungsi yang sama.
- 🔁 Efisien untuk input sedang, tetap butuh stack rekursi.
- ⏱ Kompleksitas Waktu: O(n × sum)
- 💾 Kompleksitas Memori: O(n × sum)
2. **Tabulation (Bottom-Up DP)**
Iteratif → mulai dari kasus paling kecil.
- 📋 Buat tabel dp[n+1][sum+1]– dp[i][j] = True jika subset arr[0..i-1] bisa menghasilkan j.
- Inisialisasi:- dp[i][0] = True (karena subset kosong = 0)- dp[0][j] = False (tidak bisa dapat sum = 0 tanpa elemen)
- 📈 Iterasi solusi dari bawah ke atas.

## 📥 Input
Biasanya berupa:
- Sebuah array A berisi angka-angka bulat positif
- Sebuah nilai target S, yang merupakan jumlah yang ingin dicapai dari kombinasi elemen-elemen dalam array

## 📤 Output
Output bisa bermacam-macam bentuk tergantung implementasi:
- ✅ Apakah ada subset yang jumlahnya S? (Boolean: true/false)
- 🧩 Apa saja subset-subset yang jumlahnya S? (Daftar subset)
- 🔢 Jumlah subset yang valid

## 🧮 Langkah Penyelesaian
1. Pilih atau lewati elemen
2. Batasan (constraint check)
3. Rekursi (recursive exploration)
4. Backtrack
5. Basis kasus (base case)

## 🧩 Problem and Solution Example
**CARI SUBSET DARI [3, 4, 5] YANG JUMLAHNYA 9**

**Langkah Solusi**
1. Pilih 3
    - total = 3
    - lanjutkan ke [4, 5]
2. Pilih 4
    - total = 3 + 4 = 7
    - lanjut ke 5
3. Pilih 5
    - total = 3 + 4 + 5 = 12 ❌ (lebih dari 9)
    - Backtrack
4. Lewati 5
    - total = 7 ✅❌ (belum 9, tapi sudah habis elemen)
    - Backtrack
5. Kembali ke 3 → Pilih 5 langsung
    - total = 3 + 5 = 8 ❌
    - habis elemen → Backtrack
6. Lewati 3, pilih 4 dan 5
    - total = 9 ✅
    - Simpan subset: [4, 5]
7. Lewati 3 dan 4, pilih 5 saja
    - total = 5 ❌
**Solusi akhir:** [4, 5]

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool isSubsetSum(const vector<int>& nums, int target) {
    int n = nums.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));

    // Subset dengan sum 0 selalu bisa dibuat (dengan subset kosong)
    for (int i = 0; i <= n; i++)
        dp[i][0] = true;

    // Isi tabel DP
    for (int i = 1; i <= n; i++) {
        for (int s = 1; s <= target; s++) {
            if (s < nums[i - 1]) {
                dp[i][s] = dp[i - 1][s];  // tidak bisa ambil elemen ke-i
            } else {
                dp[i][s] = dp[i - 1][s] || dp[i - 1][s - nums[i - 1]];
            }
        }
    }

    return dp[n][target];
}

int main() {
    int n, target;
    cout << "Masukkan jumlah elemen: ";
    cin >> n;

    vector<int> nums(n);
    cout << "Masukkan elemen array:\n";
    for (int i = 0; i < n; i++) {
        cin >> nums[i];
    }

    cout << "Masukkan nilai target sum: ";
    cin >> target;

    if (isSubsetSum(nums, target)) {
        cout << "Terdapat subset dengan jumlah " << target << "." << endl;
    } else {
        cout << "Tidak ada subset yang memiliki jumlah " << target << "." << endl;
    }

    return 0;
}
```

## 📚 Analisis Kompleksitas
**Kompleksitas Waktu**
- Brute Force / Rekursif -> O(2^n)
- Dynamic Programming (DP) -> O(n x sum)

**Kompleksitas Ruang**
- Brute Force -> O(n)
- DP -> O(n x sum)
- DP dioptimalkan -> O(sum)

## 🌟 Aplikasi Dunia Nyata
1. Kriptografi: Digunakan dalam sistem kriptografi berbasis ransel, di mana kesulitan menyelesaikan Permasalahan Subset Sum menjadi dasar keamanan kunci publik. 
2. Alokasi Sumber Daya: Membantu memilih subset proyek atau item agar sesuai dengan anggaran atau target tertentu.
3. Genetika dan Bioinformatika: Digunakan untuk menganalisis kombinasi gen atau fragmen DNA dengan total ekspresi atau panjang tertentu.
4. Keuangan dan Investasi: Membantu menentukan apakah target investasi dapat dicapai dengan memilih kombinasi aset yang tepat.
5. Perancangan Permainan dan Teka-Teki: Muncul dalam permainan atau teka-teki yang melibatkan pencarian kombinasi angka sesuai skor target

## 💪 Kelebihan dan Kekurangan
**Kelebihan**
- Mengajarkan teknik algoritma penting
- Contoh masalah NP-Complete
- Dasar untuk banyak aplikasi praktis
- Digunakan untuk benchmarking algoritma optimasi

**Kekurangan**
- Kompleksitas tinggi untuk input besar
- Tidak selalu scalable
- Bisa jadi terlalu abstrak
- Solusi banyak, tidak unik

## 🏁 Kesimpulan
**Subset Sum Problem (SSP)** adalah masalah dalam ilmu komputer yang bertujuan untuk menentukan apakah terdapat subset dari sekumpulan bilangan bulat yang jumlahnya sama dengan nilai target tertentu. Masalah ini tergolong dalam kategori NP-Complete dan memiliki berbagai variasi, seperti bounded, unbounded, exact k elements, partition, multi-target, hingga approximate subset sum, yang masing-masing memiliki aplikasi dan batasan berbeda.Untuk menyelesaikan SSP, digunakan pendekatan rekursif yang sederhana namun kurang efisien, serta dynamic programming yang lebih optimal baik dari segi waktu maupun ruang. Subset Sum memiliki aplikasi nyata dalam bidang kriptografi, keuangan, pengelolaan sumber daya, hingga kecerdasan buatan, menjadikannya masalah penting yang tidak hanya teoritis, tetapi juga relevan dalam dunia praktis