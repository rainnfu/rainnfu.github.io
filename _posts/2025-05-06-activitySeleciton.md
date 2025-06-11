---
layout: post
title: Activity Selection Problem 
date: 2025-05-06 12:39:22 +0800
categories: [DAA, Greedy Algorithm]
tags: [daa, alghoritm, greedy]     # TAG names should always be lowercase
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
