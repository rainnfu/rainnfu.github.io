---
layout: post
title: FRACTIONAL KNAPSACK
date: 2025-05-06
categories: [DAA, Greedy Algorithm]
tags: [daa, algorithm, greedy]
---

![Desktop View](https://res.cloudinary.com/codecrucks/images/f_webp,q_auto/v1634623105/fractional-knapsack/fractional-knapsack.jpg?_i=AA){: width="500"}
_---_

Pernah nggak sih, kamu harus milih barang bawaan buat naik gunung tapi tas kamu nggak cukup besar buat bawa semuanya? Nah, di situlah kamu butuh strategi cerdas: bawa barang paling berfaedah tanpa bikin punggung patah. 😅

Masalah ini mirip banget sama **Fractional Knapsack Problem**.

Beda dengan **0/1 Knapsack** yang galak banget—barang cuma bisa diambil penuh atau enggak sama sekali—si **Fractional Knapsack** ini lebih fleksibel. Kamu bisa ambil *sebagian* barang! Cocok banget buat kamu yang suka mikir praktis. 😎

### Apa Itu Fractional Knapsack?

**Fractional Knapsack** adalah versi santai dari masalah Knapsack, di mana kamu boleh ambil sebagian barang. Jadi kalau barangnya kegedean tapi nilainya tinggi, kamu bisa ambil sepotong aja buat dimasukin ke tas. Cuan maksimal, capek minimal!

---

### Ciri-Ciri Utama

* **Boleh ambil sebagian barang** – Bebas potong! Asal totalnya nggak melebihi kapasitas tas.
* **Tiap barang punya berat dan nilai** – Ibaratnya kayak mie instan: ada yang berat tapi nggak terlalu enak, ada yang ringan tapi gurih.
* **Kapasitas tas terbatas** – Nggak bisa bawa lemari, guys.
* **Tujuannya?** – Maksimalin total nilai barang yang dibawa.

---

### Strategi Solusi

Karena kita boleh ambil sebagian barang, kita bisa pakai pendekatan **greedy** alias rakus tapi pintar. Caranya? Ambil barang yang paling “bernilai” per kilogramnya duluan.

---

### Waktu Eksekusi (Biar Nggak Timeout)

Secara umum, waktu yang dibutuhin buat nyelesaiin masalah ini didominasi sama proses nyortir barang berdasarkan nilai per berat.

* Urutkan barang berdasarkan nilai/berat: **O(n log n)**
* Masukin barang satu-satu: **O(n)**

Totalnya? Ya, kira-kira **O(n log n)**. Nggak buruk lah.

---

### Contoh Kasus Nyata

Bayangin kamu jadi seorang penjelajah (kayak di game RPG). Kamu punya ransel 50 kg, dan ada beberapa barang di depanmu. Masing-masing punya berat dan nilai. Tapi kamu boleh ambil sebagian barang juga. Enak kan?

| Barang | Berat (kg) | Nilai (unit) |
|--------|------------|--------------|
| 1      | 10         | 60           |
| 2      | 20         | 100          |
| 3      | 30         | 120          |

Pertanyaannya: **Gimana caranya kamu bisa ngisi ransel kamu biar total nilainya paling gede?**

---

## Solusi Gaya Greedy

Kita bakal pakai algoritma greedy—alias pilih yang paling untung duluan. Langkahnya:

1. **Hitung rasio nilai/berat**  
   Cek seberapa “berharga” tiap kilogram dari setiap barang. Barang yang kasih nilai gede per kilo pasti lebih menarik.

2. **Urutkan dari yang paling cuan**  
   Barang dengan rasio tertinggi harus ada di urutan pertama. Kita prioritasin yang paling worth it.

3. **Masukin satu-satu sampai tasnya full**  
   - Kalau barangnya muat, masukin semua.
   - Kalau nggak muat, ambil secukupnya yang bisa masuk. Sisanya tinggalin (atau mungkin titip ke teman? 😂)

4. **Kalau kapasitas habis, ya udah—selesai.**

### Simulasi Singkat

Katakan:

- Kapasitas tas: 50 kg  
- Barang:
  - A: 10 kg, 60 nilai → rasio = 6
  - B: 20 kg, 100 nilai → rasio = 5
  - C: 30 kg, 120 nilai → rasio = 4

Langkah:

1. Urutkan: A (6), B (5), C (4)  
2. Masukin A → 10 kg, sisa 40 kg  
3. Masukin B → 20 kg, sisa 20 kg  
4. Ambil sebagian C → 20/30 dari berat → 2/3 dari nilai → 80

**Total nilai: 60 (A) + 100 (B) + 80 (C) = 240**

Mantap! 🎉

---

### Kesimpulan

**Fractional Knapsack** ini kayak pacaran tanpa komitmen penuh—boleh ambil sebagian asal manfaat maksimal. 😆

Dengan pendekatan **greedy**, kita cukup fokus pada barang-barang dengan nilai per berat tertinggi. Karena bisa ambil sebagian, kita nggak perlu mikir ribet kayak di 0/1 Knapsack. Cukup urutkan → ambil yang paling untung → selesai.

Dan dengan kompleksitas waktu yang efisien (*O(n log n)*), algoritma ini cocok banget buat banyak kasus nyata seperti manajemen sumber daya, investasi, sampai packing koper liburan biar semua skincare bisa masuk.

![Desktop View](https://i.pinimg.com/564x/3f/55/43/3f554324815a6183498fb891d37b1a97.jpg){: width="200"}
_---_
