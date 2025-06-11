---
layout: post
title: "Activity Selection Problem"
date: 2025-05-06
categories: [DAA, Greedy Alghoritm]
tags: [daa, algorithm, greedy]
---

Halo! Kali ini kita bakal bahas salah satu topik keren di dunia algoritma dan optimasi: **Activity Selection Problem**. Yap, ini tentang milih aktivitas sebanyak mungkin tanpa bikin mereka tabrakan kaya jadwal kuliah yang numpuk semua di hari Senin.

---

### 😎 Apa sih permasalahan ini?

Bayangin kamu punya daftar kegiatan—misalnya rapat, kelas, main game (yang ini opsional sih 😅) dan kamu cuma punya satu ruang atau waktu untuk ngerjain semuanya. Masalahnya: kamu cuma bisa ngelakuin satu hal dalam satu waktu. Jadi, gimana caranya milih kegiatan sebanyak mungkin *tanpa ada yang saling numpuk*?

Jawabannya? Gunain **algoritma greedy**. Alias, kita ambil yang paling cepat selesai dulu, biar waktu selanjutnya bisa dipakai buat yang lain. Ibaratnya: "Sikat yang bisa disikat dulu, nanti pikirin sisanya."

---

### 📚 Inti Permasalahannya

Setiap aktivitas punya dua hal: waktu mulai dan waktu selesai. Tujuannya? Cari kombinasi sebanyak mungkin dari aktivitas-aktivitas itu yang **nggak saling bentrok**.

Kegiatan dianggap aman dan kompatibel kalau salah satu udah selesai sebelum yang lain mulai. Gampangnya, jangan sampe A sama B jalan barengan, kecuali kamu punya dua badan (yang sayangnya belum bisa 🤖).

---

### ⚡ Greedy Strategy, Tapi Bukan Serakah

*Greedy algorithm* itu pendekatan yang milih solusi yang keliatan paling bagus **saat itu juga**. Kita nggak mikirin masa depan terlalu jauh, tapi herannya, buat masalah ini malah berhasil dapet solusi paling optimal!

Di sini, caranya dengan milih aktivitas yang selesai paling cepat. Semakin cepet kita kelarin satu, semakin banyak yang bisa kita ambil setelahnya.

---

### 🔁 Gimana Caranya?

Langkah-langkahnya simpel banget:

1. Urutkan semua aktivitas berdasarkan waktu selesai (dari yang paling cepet).
2. Ambil aktivitas pertama—karena dia selesai duluan.
3. Lanjut pilih aktivitas berikutnya **kalau waktu mulainya ≥ waktu selesai** aktivitas sebelumnya.

#### 💡 Pseudocode-nya gini:

```text
ACTIVITY-SELECTOR(s, f, n)
    A = {a1}         // Ambil aktivitas pertama
    j = 1
    for i = 2 to n
        if s[i] >= f[j]
            A = A ∪ {ai}
            j = i
    return A
```

---

### 🧪 Contoh Soal (Nggak Seserem UTS, Tenang)

| Aktivitas | Mulai | Selesai |
|-----------|-------|---------|
| A1        | 1     | 4       |
| A2        | 3     | 5       |
| A3        | 0     | 6       |
| A4        | 5     | 7       |
| A5        | 8     | 9       |
| A6        | 5     | 9       |

Langkah penyelesaiannya:

1. Urutin berdasarkan waktu selesai → `[A1, A2, A3, A4, A5, A6]`.
2. Ambil A1.
3. A2 & A3? Bentrok. ❌
4. A4? Aman ✅ → {A1, A4}
5. A5? Aman juga ✅ → {A1, A4, A5}
6. A6? Maaf, bentrok lagi. ❌

**Jadi hasil akhirnya: `{A1, A4, A5}`. Gasken!**

---

### 🧑‍💻 Kodingan Versi C++

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

### 📊 Analisis Kompleksitas

- **Waktu:**
  - Urutin aktivitas: `O(n log n)`
  - Seleksi: `O(n)`
  - Total: `O(n log n)` — secepat kamu buka chat pas dosen bilang “open note”.

- **Ruang:**
  - Nyimpen daftar aktivitas: `O(n)`
  - Output juga `O(n)` kalau semuanya bisa kepilih.

---

### 🌍 Aplikasi di Dunia Nyata

**Dipakai buat hal-hal kayak:**
- Penjadwalan ruang kelas, lab, atau tempat futsal 😆
- Alokasi CPU di sistem operasi.
- Logistik: ngatur rute dan waktu pengiriman.
- Telekomunikasi: jadwal transmisi data atau alokasi bandwidth.

---

### ✅ Plus Minusnya

**Keunggulan:**
- Mudah diimplementasikan (nggak bikin sakit kepala).
- Ngebut buat data gede.
- Solusinya optimal kalau masalahnya "bersih", alias nggak ada aturan aneh-aneh.

**Kelemahan:**
- Harus urut dulu.
- Kurang cocok buat masalah yang ribet kayak ada prioritas, biaya, atau jarak.

---

### 🎯 Penutup

*Activity Selection Problem* adalah contoh klasik masalah optimasi yang ternyata bisa disikat pake strategi greedy. Dengan pilih aktivitas yang cepat selesai duluan, kita bisa dapet solusi yang optimal tanpa perlu nguras otak berlebihan.

Tapi inget, ini cuma cocok buat kasus yang "lurus-lurus aja". Kalau kamu ketemu masalah yang penuh drama dan kompleksitas, mungkin harus naik level ke dynamic programming atau bahkan algoritma AI. Tapi untuk sekarang, greedy udah cukup keren kok 🚀

---

Kalau kamu pengen belajar lebih banyak soal algoritma lain, stay tuned di blog ini ya. Jangan sampe ketinggalan 😎
