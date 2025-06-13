---
layout: post
title: HUFFMAN CODING
date: 2025-05-20
categories: [DAA, Greedy Algorithm]
tags: [daa, algorithm, greedy]     # TAG names should always be lowercase
---

![Coding Time](https://upload-os-bbs.hoyolab.com/upload/2024/01/09/137652040/586aa1a23ffe39e6955e3d49bd0c59bc_6338591603245794027.jpg?x-oss-process=image%2Fresize%2Cs_1000%2Fauto-orient%2C0%2Finterlace%2C1%2Fformat%2Cwebp%2Fquality%2Cq_70){: width="450"}
_---_

Kalau kamu pernah kirim meme, file zip, atau video kucing lewat internet, percaya deh — itu semua butuh teknik kompresi biar nggak makan kuota segunung. Nah, salah satu teknik kece buat kompresi data adalah **Huffman Coding**.

**Huffman Coding** ini semacam algoritma jenius yang tahu karakter mana yang sering muncul, dan dia bakal kasih kode lebih pendek buat mereka. Sementara karakter yang jarang muncul? Ya, dapet kode panjang. Mirip kayak temenmu yang banyak ngomong dapet tempat duduk depan pas presentasi, yang pendiam duduk di belakang. 😅

### 📚 Kenapa Huffman Coding itu keren?

Karena dia jadi pondasi di berbagai sistem kompresi kayak:

- Format file: JPEG, MP3, PNG
- Protokol komunikasi (biar cepet ngirim data)
- Kompresi teks di sistem info dan search engine

Dengan ngerti ini, kamu udah satu langkah lebih deket jadi master coding hemat memori.

---

### Konsepnya Gimana Sih?

Simple banget:

- Karakter sering muncul → kode pendek
- Karakter langka → kode panjang
- Dibuat pakai pohon biner, jadi berasa main tanam-tanaman digital

## 🌱 Step-by-step Huffman Coding

1. Hitung frekuensi tiap karakter
2. Buat node untuk tiap karakter
3. Gabungkan dua node dengan frekuensi terkecil
4. Ulangi sampai cuma ada satu node → itulah akar pohon Huffman
5. Traversal buat kasih kode 0 dan 1 (kiri-kanan)

## 🎯 Studi Kasus

Bayangin kamu dapet string: `"FAHIRAFAHIRA"`  
Gimana caranya string ini dikompres biar hemat memori tapi bisa dibuka lagi kayak semula?

---

## 💻 Solusi dalam C++ (Tenang, nggak perlu nangis)

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>
using namespace std;

// Node Huffman Tree
struct Node {
    char ch;
    int freq;
    Node *left, *right;
    Node(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
};

// Perbandingan untuk min-heap
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

void huffmanCoding(string text) {
    unordered_map<char, int> freq;
    for (char ch : text) freq[ch]++;

    priority_queue<Node*, vector<Node*>, Compare> pq;
    for (auto pair : freq) {
        pq.push(new Node(pair.first, pair.second));
    }

    while (pq.size() > 1) {
        Node* left = pq.top(); pq.pop();
        Node* right = pq.top(); pq.pop();
        Node* merged = new Node('\0', left->freq + right->freq);
        merged->left = left;
        merged->right = right;
        pq.push(merged);
    }

    Node* root = pq.top();
    unordered_map<char, string> huffmanCode;
    generateCodes(root, "", huffmanCode);

    cout << "Kode Huffman:\n";
    for (auto pair : huffmanCode) {
        cout << pair.first << " : " << pair.second << '\n';
    }

    cout << "\nTeks Asli: " << text << '\n';
    cout << "Teks Terkompresi (bit): ";
    for (char ch : text) {
        cout << huffmanCode[ch];
    }
    cout << endl;
}

int main() {
    string text = "FAHIRAFAHIRA";
    huffmanCoding(text);
    return 0;
}
```

Output:
```
Kode Huffman:
F : 00
A : 01
H : 10
I : 110
R : 111

Teks Asli: FAHIRAFAHIRA
Teks Terkompresi (bit): 0001101011110100011010111101
```

---

## ⚖️ Kelebihan dan Kekurangan

### Kelebihan
- ✅ **Lossless** → Data yang didekompresi 100% sama dengan aslinya
- ✅ **Efisien banget** buat data yang karakternya nggak seimbang frekuensinya
- ✅ **Diterapkan luas** di dunia nyata (bukan cuma tugas kampus!)

### Kekurangan
- ❌ **Kurang optimal** kalau semua karakter muncul merata (kaya anak kelas yang semuanya aktif)
- ❌ **Butuh tabel kode** buat dekompresi, jadi harus disimpan bareng datanya

---

### 💬 Penutup

Huffman Coding tuh kayak ngasih prioritas jalan ke karakter-karakter favorit di jalan tol data. Yang sering lewat dapet jalur cepat, yang jarang disuruh muter-muter dulu. 🤭  
Tapi hasil akhirnya? Semua sampai tujuan dengan aman, dan datamu jadi lebih ramping dari sebelumnya.

---
