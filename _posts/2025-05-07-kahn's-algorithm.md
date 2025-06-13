---
layout: post
title: "KAHN'S ALGORITHM"
date: 2025-05-27
categories: [DAA, Kahn's Algorithm]
tags: [daa, algorithm, backtracking, kahn]
---

Pernah nggak sih, mau ngerjain sesuatu tapi ternyata ada syaratnya? Kayak mau masak mie instan, ya harus rebus air dulu. Mau *install game*, ya harus *download* dulu. Nggak bisa dibalik, kan? Nah, di dunia *programming* dan manajemen proyek, "syarat-syarat" kayak gini tuh bejibun.

Hubungan "ini dulu, baru itu" secara teknis disebut **graf berarah tanpa siklus (DAG - Directed Acyclic Graph)**. Keren ya namanya? Padahal intinya cuma "graf anti-mundur" yang nggak punya alur kerja berputar-putar.

Nah, buat nyusun "daftar urutan kerja" yang benar biar nggak ada drama, kita butuh yang namanya **topological sorting**. Dan salah satu jagoan untuk melakukan ini adalah **Algoritma Kahn**. Anggap aja dia ini manajer proyek super pintar yang pakai strategi **BFS (Breadth-First Search)**. Dia bakal cari tahu, "Siapa nih yang bisa dikerjain duluan karena nggak punya prasyarat?" terus lanjut ke yang lain secara sistematis.

### Kapan Si Kahn Ini Dibutuhkan?

Algoritma ini bakal jadi pahlawan di berbagai skenario, misalnya:
* Pas mau bikin jadwal kerjaan yang saling ketergantungan (kayak *build system*).
* Buat ngecek, jangan-jangan ada "lingkaran setan" di kerjaan kita (misal: A butuh B, B butuh C, tapi C butuh A. Kapan kelarnya, coba?).
* Intinya, setiap kali **urutan** itu PENTING.

### Gimana Cara Kerjanya?

Proses kerja Algoritma Kahn itu simpel, kok. Bayangin aja lagi bagi-bagi tugas di kelompok:
1.  **Data Siapa Paling Mandiri**: Kita data dulu, tiap tugas itu "bergantung" ke berapa tugas lain. Istilah kerennya, kita hitung *in-degree* (jumlah panah masuk) untuk tiap simpul.
2.  **Panggil yang Mandiri Duluan**: Semua tugas yang nggak bergantung ke siapa-siapa (*in-degree* = 0) kita panggil duluan dan masukin ke sebuah antrean (*queue*).
3.  **Kerjakan dan Lepas Ketergantungan**: Satu per satu tugas di antrean kita "kerjakan". Kalau udah selesai, semua tugas yang tadinya bergantung sama dia, kita kurangi "beban ketergantungannya" satu per satu.
4.  **Cek Lagi, Siapa yang Jadi Mandiri?**: Kalau ada tugas yang beban ketergantungannya jadi 0 setelah ditinggal, langsung masukkan dia ke antrean! Ulangi terus langkah 3 & 4 sampai antrean kosong.
5.  **Audit Hasil**: Kalau semua tugas berhasil diproses, selamat! Urutannya valid. Tapi kalau ada yang nyangkut dan nggak masuk daftar, wah, fix itu ada siklus di dalamnya.

## ❓ Kasusnya Gimana Sih?

Bayangin kamu lagi ngerjain proyek *software* gede yang filenya banyak banget. Biar bisa jalan, ada aturannya: file `parser.cpp` harus kelar sebelum `compiler.cpp`, dan `compiler.cpp` harus beres sebelum `main.cpp`. Nah, tugasmu adalah nentuin urutan kompilasi yang paling benar biar nggak ada *error* "dependency not found" yang bikin panik.

### ➡️ Inputnya:
- Jumlah simpul (tugas atau modul).
- Daftar ketergantungan antar simpul dalam bentuk pasangan `(a, b)` yang artinya **`a` harus selesai sebelum `b`**.

### ⬅️ Outputnya:
- Daftar urutan kerja (topologis) yang benar, atau pesan kalau ada "lingkaran setan" yang bikin urutannya mustahil.

---

## ✅ Solusi (Implementasi C++)

### 💡 Langkah-langkah Versi Ngoding:

1. Hitung `in_degree` untuk setiap simpul.
2. Masukkan semua simpul dengan `in_degree` sama dengan 0 ke dalam *queue*.
3. Selama *queue* masih ada isinya:
   - "Kerjakan" simpul paling depan, lalu keluarkan dari *queue* dan catat di hasil urutan.
   - Untuk setiap tetangga dari simpul yang baru dikerjakan, kurangi `in_degree`-nya.
   - Kalau `in_degree` tetangga jadi 0, masukkan dia ke *queue*.
4. Setelah *loop* selesai, cek: jumlah simpul di hasil urutan sama nggak dengan jumlah simpul awal?
5. Kalau nggak sama, berarti ada siklus. Kalau sama, berarti urutannya valid!

---

### 💻 Saatnya Ngoding! (Implementasi C++)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void kahnAlgorithm(int V, vector<vector<int>>& adj) {
    vector<int> in_degree(V, 0);

    // Hitung in-degree untuk setiap simpul
    for (int i = 0; i < V; i++) {
        for (int neighbor : adj[i]) {
            in_degree[neighbor]++;
        }
    }

    queue<int> q;
    // Masukkan semua simpul yang mandiri (in-degree == 0) ke antrean
    for (int i = 0; i < V; i++) {
        if (in_degree[i] == 0)
            q.push(i);
    }

    vector<int> topo_order;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        topo_order.push_back(node);

        // Kurangi in-degree semua tetangganya
        for (int neighbor : adj[node]) {
            in_degree[neighbor]--;
            // Kalau ada tetangga yang jadi mandiri, masukkan ke antrean
            if (in_degree[neighbor] == 0)
                q.push(neighbor);
        }
    }

    // Cek apakah ada siklus atau tidak
    if (topo_order.size() != V) {
        cout << "Waduh, ada siklus! Nggak bisa diurutin, nih.\n";
    } else {
        cout << "Urutan Topological yang Aman: ";
        for (int node : topo_order) {
            cout << node << " ";
        }
        cout << endl;
    }
}

int main() {
    int V = 6;
    vector<vector<int>> adj(V);

    // Contoh kasus:
    // 5 butuh dikerjakan sebelum 0 dan 2
    // 4 butuh dikerjakan sebelum 0 dan 1
    // 2 butuh dikerjakan sebelum 3
    // 3 butuh dikerjakan sebelum 1
    adj[5].push_back(0);
    adj[5].push_back(2);
    adj[4].push_back(0);
    adj[4].push_back(1);
    adj[2].push_back(3);
    adj[3].push_back(1);

    kahnAlgorithm(V, adj);
    return 0;
}

Output:

Urutan Topological yang Aman: 4 5 2 3 1 0
```

### Kelebihan dan Kekurangan

---

**Kelebihan:**

* **Ngebut!**: Punya kompleksitas waktu $O(V+E)$ yang efisien banget, jadi tetap cepat meskipun grafnya besar.
* **Detektif Handal**: Jago banget nemuin "lingkaran setan" atau dependensi yang bikin macet.
* **Sesuai Kebutuhan**: Cocok banget buat masalah dunia nyata kayak nentuin urutan *install library*, prasyarat mata kuliah, sampai resep masakan.

**Kekurangan:**

* **Anti Lingkaran Setan**: Kalau grafnya ada siklus, algoritma ini bakal angkat tangan alias nggak bisa kerja.
* **Banyak Jalan Menuju Roma**: Kadang, bisa ngasih beberapa alternatif urutan yang sama-sama valid. Jadi, jangan kaget kalau hasilnya beda sama punya temanmu.
* **Perlu Bawaan Ekstra**: Butuh "kantong" tambahan (kayak `array` atau `map`) buat nyimpan data *in-degree* dan grafnya itu sendiri.

---

### Kesimpulan

Intinya, Algoritma Kahn ini adalah *tool* super keren dan efisien buat siapa aja yang berurusan sama tugas berantai. Daripada pusing tujuh keliling nentuin mana dulu yang harus dikerjain, serahin aja ke Kahn!

Dengan logikanya yang sistematis, dia nggak cuma ngasih urutan kerja yang pas, tapi juga bisa jadi satpam yang ngejagain proyek kita dari siklus tak berujung. Pokoknya, ini salah satu *skill* yang wajib ada di kantong para *programmer*!