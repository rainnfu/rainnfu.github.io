---
layout: post
title: "Pemecahan Masalah Activity Selection"
date: 2025-06-11
categories: algoritma optimasi greedy
---

## Dipublikasikan oleh Rainnfu Dewa Basket  
**Diterbitkan:** 6 Mei 2025  
**Diperbarui:** 9 Juni 2025  
**Waktu baca:** 6 menit

---

Halo! Kali ini kita akan membahas salah satu konsep fundamental dalam algoritma dan teknik optimasi, yaitu *Activity Selection Problem*.

---

### Gambaran Umum

Dalam berbagai konteks kehidupan nyata maupun sistem komputasi, sering kali kita harus menentukan aktivitas mana yang bisa dijalankan dari sekumpulan aktivitas dengan batasan tertentu. Setiap aktivitas punya waktu mulai dan waktu selesai. Tantangannya muncul ketika kita hanya memiliki satu sumber daya — seperti satu ruangan, satu CPU, atau satu orang — yang hanya bisa menangani satu aktivitas dalam satu waktu.

Pertanyaannya: **bagaimana kita dapat memilih kombinasi aktivitas terbanyak tanpa saling bertabrakan secara waktu?**

Inilah inti dari masalah *Activity Selection*. Masalah ini sangat cocok untuk diselesaikan dengan pendekatan *greedy algorithm*, yaitu teknik yang memilih solusi terbaik pada setiap langkah kecil dengan harapan hasil akhirnya juga optimal.

---

### Definisi Masalah

*Activity Selection Problem* berfokus pada memilih sebanyak mungkin aktivitas dari kumpulan aktivitas yang tidak saling tumpang tindih secara waktu. Setiap aktivitas memiliki waktu mulai (`start`) dan selesai (`finish`). Dua aktivitas dianggap kompatibel jika yang satu selesai sebelum yang lain dimulai.

Tujuan akhirnya adalah menemukan subset terbesar dari aktivitas yang semuanya saling kompatibel, dijalankan oleh satu sumber daya.

---

### Prinsip Dasar Algoritma Greedy

*Greedy algorithm* adalah metode yang secara iteratif memilih opsi yang paling menguntungkan pada saat itu juga, tanpa memperhitungkan hasil ke depan.

Dalam konteks *Activity Selection*, strategi ini dilakukan dengan cara selalu memilih aktivitas yang memiliki waktu selesai paling awal. Dengan menyelesaikan aktivitas secepat mungkin, kita menyisakan waktu lebih banyak untuk aktivitas selanjutnya.

---

### Langkah-Langkah Algoritma Activity Selection

1. Urutkan semua aktivitas berdasarkan waktu selesai (ascending).
2. Pilih aktivitas pertama dalam urutan (waktu selesai paling awal).
3. Untuk setiap aktivitas berikutnya, pilih jika waktu mulai ≥ waktu selesai aktivitas terakhir yang terpilih.

#### Pseudocode

```text
ACTIVITY-SELECTOR(s, f, n)
    A = {a1}         // Pilih aktivitas pertama
    j = 1
    for i = 2 to n
        if s[i] >= f[j]
            A = A ∪ {ai}
            j = i
    return A
```

Keterangan:
- `s` = array waktu mulai aktivitas.
- `f` = array waktu selesai (sudah diurutkan).
- `n` = jumlah total aktivitas.
- `A` = kumpulan aktivitas yang dipilih.
- `j` = indeks aktivitas terakhir yang dipilih.

---

### Contoh Soal

Misalkan ada enam aktivitas:

| Aktivitas | Mulai | Selesai |
|-----------|-------|---------|
| A1        | 1     | 4       |
| A2        | 3     | 5       |
| A3        | 0     | 6       |
| A4        | 5     | 7       |
| A5        | 8     | 9       |
| A6        | 5     | 9       |

**Langkah Penyelesaian:**

1. Urutkan aktivitas berdasarkan waktu selesai → `[A1, A2, A3, A4, A5, A6]`.
2. Pilih A1.
3. A2 dan A3 tumpang tindih ❌.
4. A4 cocok ✅ → {A1, A4}
5. A5 cocok ✅ → {A1, A4, A5}
6. A6 tumpang tindih ❌.

**Solusi akhir:** `{A1, A4, A5}`

---

### Implementasi C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Activity {
    int start, finish, index;
};

bool activityCompare(Activity s1, Activity s2) {
    return (s1.finish < s2.finish);
}

void printMaxActivities(vector<Activity>& arr, int n) {
    sort(arr.begin(), arr.end(), activityCompare);
    cout << "Aktivitas yang terpilih: ";
    int i = 0;
    cout << "A" << arr[i].index << " ";
    for (int j = 1; j < n; j++) {
        if (arr[j].start >= arr[i].finish) {
            cout << "A" << arr[j].index << " ";
            i = j;
        }
    }
    cout << endl;
}

int main() {
    vector<Activity> arr = {
        {1, 4, 1}, {3, 5, 2}, {0, 6, 3},
        {5, 7, 4}, {8, 9, 5}, {5, 9, 6}
    };
    int n = arr.size();
    printMaxActivities(arr, n);
    return 0;
}
```

**Keluaran:**
```
Aktivitas yang terpilih: A1 A4 A5
```

---

### Analisis Kompleksitas

- **Waktu:**
  - Pengurutan aktivitas: `O(n log n)`
  - Seleksi aktivitas: `O(n)`
  - Total: `O(n log n)`

- **Ruang:**
  - Menyimpan data aktivitas: `O(n)`
  - Penyimpanan hasil terpilih: `O(n)` (dalam kasus ekstrem)

---

### Aplikasi Nyata & Evaluasi

**Aplikasi Dunia Nyata:**
- Penjadwalan ruang kelas, lab, atau ruang rapat.
- Pengalokasian waktu CPU dalam OS.
- Penjadwalan kendaraan dan pengiriman logistik.
- Manajemen alokasi bandwidth di jaringan.

**Kelebihan Algoritma Greedy:**
- Implementasi sederhana.
- Sangat efisien untuk data dalam jumlah besar.
- Solusi optimal untuk kasus tanpa batasan kompleks.

**Kekurangan:**
- Harus melakukan pengurutan awal.
- Kurang cocok jika ada batasan tambahan seperti prioritas, jarak, atau biaya.

---

### Kesimpulan

*Activity Selection Problem* adalah persoalan klasik dalam optimasi penjadwalan yang dapat diselesaikan secara efisien dengan pendekatan greedy. Dengan memilih aktivitas berdasarkan waktu selesai paling awal, kita bisa memperoleh hasil optimal dengan kompleksitas `O(n log n)`. Meskipun sangat cocok untuk kasus penjadwalan dasar, jika problem memiliki banyak batasan tambahan, kita perlu pertimbangkan pendekatan lain seperti *dynamic programming* atau *metaheuristic algorithms*.
