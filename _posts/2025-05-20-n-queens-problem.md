---
title: N-QUEENS PROBLEM
date: 2025-05-20
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking]     # TAG names should always be lowercase
---

![Desktop View](https://media.geeksforgeeks.org/wp-content/uploads/20230814111624/N-Queen-Problem.png){: width="500"}

---

Pernah dengar teka-teki catur paling "drama" sedunia? Kenalin, namanya **N-Queens Problem** 👑. Misinya kelihatannya simpel tapi bisa bikin pusing: menempatkan `N` ratu di papan catur `N x N` **tanpa ada satu pun yang bisa saling serang**.

Dalam catur, ratu itu *overpowered*. Dia bisa bergerak lurus (horizontal & vertikal) dan miring (diagonal) sesuka hatinya. Nah, tantangannya adalah gimana caranya menempatkan semua ratu di papan agar tidak ada yang berada di **baris, kolom, atau diagonal yang sama**.

Ini bukan sekadar iseng-iseng biar jago catur, lho. Prinsip di balik N-Queens ini kepake banget buat:
- Mengatur jadwal biar nggak ada yang bentrok.
- Melatih logika AI untuk mencari solusi.
- Menguji seberapa jago sebuah algoritma *backtracking*.

---

## Jadi, Apa Sih Inti Masalahnya?

N-Queens adalah *puzzle* klasik di dunia IT yang intinya adalah mencari solusi di tengah banyak aturan. Aturannya ketat: **satu baris, satu kolom, dan satu diagonal hanya boleh diisi oleh satu ratu**.

Tujuan utamanya bukan cuma nemuin satu solusi, tapi sering kali untuk:
1.  Menemukan **semua kemungkinan** posisi yang aman.
2.  Menjadi "arena tanding" untuk menguji algoritma pencarian seperti *backtracking*, DFS, sampai algoritma genetika.
3.  Melatih otak kita untuk berpikir logis dalam menyelesaikan *Constraint Satisfaction Problem* (CSP) atau masalah yang penuh batasan.

### Kenapa Masalah Ini Penting Banget?

1.  **Menu Wajib Belajar Ngoding Lanjutan:** N-Queens adalah contoh paling pas buat belajar teknik *backtracking*. Hampir semua mata kuliah Desain dan Analisis Algoritma (DAA) atau Kecerdasan Buatan (AI) pasti membahas masalah ini.
2.  **Contoh Sempurna Masalah Penuh Aturan (CSP):** Kamu harus menempatkan ratu (nilai) di papan (variabel) tanpa melanggar aturan (batasan). Konsep ini ada di mana-mana.
3.  **Bukan Cuma Catur, tapi Juga Soal Dunia Nyata:** Prinsipnya bisa diterapkan untuk masalah penjadwalan, penempatan *chip* di sirkuit elektronik, sampai alokasi sumber daya.
4.  **Ujian Kekuatan untuk Algoritma:** Jumlah kemungkinan solusi meledak seiring bertambahnya `N`. Ini cara yang bagus buat ngeliat seberapa efisien sebuah algoritma.

---

## Kenapa *Backtracking* Jadi Jagoannya?

Bayangin kamu lagi nyoba-nyoba mecahin Sudoku. Kamu isi satu angka, lalu lanjut ke kotak lain. Kalau ternyata angka yang kamu isi salah dan bikin buntu, apa yang kamu lakukan? Pasti kamu hapus angka itu dan coba angka lain, kan? Nah, itulah *backtracking*!

N-Queens sangat cocok diselesaikan dengan *backtracking* karena:
1.  **Pengerjaan Coba-Coba yang Terstruktur:** Kita menempatkan ratu satu per satu, baris demi baris. Di setiap baris, kita coba semua kolom.
2.  **Kalau Aman, Lanjut!:** Kalau posisi aman, kita maju ke baris berikutnya (rekursi).
3.  **Kalau Buntu, Mundur!:** Kalau di satu baris nggak ada kolom yang aman, kita **mundur (*backtrack*)** ke baris sebelumnya, lalu geser ratu di sana ke posisi lain. Proses "maju-mundur cantik" inilah inti dari *backtracking*.
4.  **Pangkas Jalan Buntu:** *Backtracking* secara cerdas membuang semua kemungkinan yang sudah pasti salah, jadi kita tidak perlu mengecek semua kombinasi yang ada di alam semesta.

---

## ❓ Tantangannya

Diberikan papan catur `N x N`, tempatkan `N` buah ratu sedemikian rupa sehingga **tidak ada dua ratu yang saling menyerang**.

### ➡️ Input:
- Sebuah bilangan bulat `N`, yang merupakan ukuran papan sekaligus jumlah ratu.

### ⬅️ Output:
- Semua konfigurasi papan di mana `N` ratu bisa ditempatkan dengan aman.

---

## ✅ Solusi (Implementasi C++)

Solusi paling populernya? Tentu saja pakai **backtracking**. Kita akan coba menempatkan ratu di setiap baris, dan jika ada konflik, kita akan mundur dan mencoba posisi lain.

---

### 💻 Kode C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Fungsi untuk mengecek apakah posisi (row, col) aman dari serangan
bool isSafe(int row, int col, vector<string>& board, int n) {
    // Cek ke atas di kolom yang sama
    for (int i = 0; i < row; i++)
        if (board[i][col] == 'Q') return false;

    // Cek diagonal kiri atas
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
        if (board[i][j] == 'Q') return false;

    // Cek diagonal kanan atas
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++)
        if (board[i][j] == 'Q') return false;

    return true; // Posisi aman!
}

// Fungsi rekursif utama dengan backtracking
void solve(int row, vector<string>& board, vector<vector<string>>& solutions, int n) {
    // Base case: jika semua ratu sudah ditempatkan
    if (row == n) {
        solutions.push_back(board);
        return;
    }

    // Coba tempatkan ratu di setiap kolom pada baris saat ini
    for (int col = 0; col < n; col++) {
        if (isSafe(row, col, board, n)) {
            board[row][col] = 'Q';    // Tempatkan ratu
            solve(row + 1, board, solutions, n); // Lanjut ke baris berikutnya
            board[row][col] = '.';    // Backtrack: hapus ratu, coba posisi lain
        }
    }
}

void nQueens(int n) {
    vector<vector<string>> solutions;
    vector<string> board(n, string(n, '.'));

    solve(0, board, solutions, n);

    cout << "Jumlah solusi untuk N = " << n << " adalah: " << solutions.size() << endl << endl;
    for (const auto& sol : solutions) {
        for (const string& row : sol)
            cout << row << endl;
        cout << "----------" << endl;
    }
}

int main() {
    int N = 4;
    nQueens(N);
    return 0;
}
```

---
## Kesimpulan

Jadi, N-Queens bukan sekadar teka-teki iseng, tapi ini adalah "gym" untuk otak *programmer*. Masalah ini memaksa kita untuk berpikir secara terstruktur dan memecahkan masalah yang kelihatannya mustahil dengan cara yang sistematis.

Dengan menguasai backtracking melalui N-Queens, kita belajar cara memangkas "jalan buntu" dan fokus pada solusi yang paling mungkin. Kalau sudah bisa menaklukkan para "ratu drama" ini, kamu siap hadapi tantangan coding yang lebih gede lagi! 💪