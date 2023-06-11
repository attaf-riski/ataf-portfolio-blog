---
title: "Knapsack Problem With Dynamic Programming And Exhaustive Search"
description: ""
dateString: Juni 2023 ·
draft: false
tags: ["dynamic programming", "exhaustive search", "java"]
showToc: false
weight: 209
cover:
  image: "/blog/knapsack_with_DP_ES/cover.png"
  caption: "Made by Ataf with Krita"
---

# Pendahuluan

Di dunia ini permasalahan punya lebih dari satu solusi, malahan kadang masalah tidak punya solusi. Solusi yang didapatkan ada yang lebih baik dari solusi lain. Tidak jauh berbeda dengan permasalahan di dunia pemrograman, terdapat banyak solusi yang bisa digunakan untuk menyelesaikan masalah, salah satu masalah yang punya banyak solusi adalah permasalahan _Knapsack_ 🎒.

# Permasalahan

![](/blog/knapsack_with_DP_ES/akira.jpg)

Hidup selalu diwarnai dengan permasalahan. Begitu juga dengan knapsack problem yang memiliki banyak masalah turunan yang mirip dengan knapsack. Misalnya saja kasus ini:

**Media Advertising Corp (MAC) adalah perusahaan yang bergerak di bidang periklanan, khususnya di area Kota Semarang. Untuk meningkatkan pengalaman pengguna di divisi billboard, perusahaan MAC ingin menambahkan fitur agar pengguna bisa memilih billboard dengan anggaran yang pengguna miliki dan mendapatkan pemirsa perhari tertinggi. Bantulah MAC untuk mewujudkan fitur ini.**

Permasalahan di atas adalah masalah knapsack, terlihat dari adanya target untuk mendapatkan pemirsa tertinggi dari satu atau lebih billboard sesuai dengan anggaran iklan pengguna. Misalkan saja tiap billboard memiliki jumlah pemirsa dan harga maka lengkap sudah knapsack komponennya 🎊.

Selanjutnya terjemahkan permasalahan tersebut ke dalam dunia pemrograman, misalkan saja ditentukan inputan, constraint, dan output agar mirip dengan tantangan di HackerRank.

**Input**
N = Anggaran Untuk Iklan

Array Pasangan = Harga Billboard(W) dan Jumlah Pemirsa Perhari(P)

- 5000000 <= N <= 100000000
- 100000 <= elemen W <= 3000000
- 5000000 <= elemen P <= 30000000

**Constraint**

- 1000 ms

**Output**

- jumlah akumulasi pemirsa
- index billboard terpilih (index mulai dari 1 & urutan dari yang paling besar)

**Contoh TestCase**
![](/blog/knapsack_with_DP_ES/testcase.png)

Mau coba langsung 🤨 sebelum lihat solusi dibawah? Bisa kunjungi [kontes HackerRank](https://www.hackerrank.com/tugas-asa-week-n-1)

# Solusi

![](/blog/knapsack_with_DP_ES/eren.png)

Karena sudah terindikasi bahwa permasalahan ini adalah tipe knapsack problem maka ada beberapa algoritma yang bisa dipakai untuk menyelesaikannnya seperti Dynamic Programming, Greedy, Exhaustive Search, dan algoritma-algoritma di luar sana yang belum kesebut. Oleh karena itu di sini aku akan memakai Dynamic Programming dan Exhaustive Search.

Mapping permasalahan knapsack:

- Anggaran Iklan = Kapasitas
- Harga Papan Iklan/billboard = bobot
- Jumlah Pemirsa = profit

# Knapsack dengan Dynamic Programming

```java
// Nama file: DP.java
// Deskripsi: Dynamic Programming Algorithm for Knapsack
// Pembuat: Attaf Riski Putra Ramadhan
// Last Updated: 11 Juni 2023

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Scanner;

public class DP {
    static ArrayList<Long> jumlahPemirsa = new ArrayList<>();
    static ArrayList<Long> hargaBillboard = new ArrayList<>();
    static long pembagi = 1000000; // pembagi nilai besar agar lebih kecil dalam pembuatan tabel dynamic

    public static void main(String[] args) throws Exception {
        // KAMUS
        Long anggaranBiaya;

        // ALGORITMA
        try (Scanner input = new Scanner(System.in)) {
            anggaranBiaya = input.nextLong();
            while (input.hasNextBigInteger()) {
                hargaBillboard.add(input.nextLong());
                jumlahPemirsa.add(input.nextLong());

            }
        }
        DPKnapsack(anggaranBiaya);

    }

    static void showTable(int i, int j, long tabel[][]) {
        for (int k = 0; k < i; k++) { // sampe ke i karena
            for (int l = 0; l <= j; l++) { // sampe ke = j karena index ditambah satu untuk kolom ke 0
                System.out.print(tabel[k][l] + " ");
            }
            System.out.println("===");
        }
    }

    static long max(long a, long b) {
        return (a > b) ? a : b;
    }

    static void DPKnapsack(Long anggaranBiaya) {

        long i = jumlahPemirsa.size();
        long j = anggaranBiaya / pembagi;
        long[][] tabel = new long[(int) i][(int) j + 1];
        for (long[] row : tabel) {
            Arrays.fill(row, 0);
        }

        for (int k = 0; k < i; k++) { // sampe ke i karena baris
            for (int l = 0; l <= j; l++) { // sampe ke = j karena index ditambah satu untuk kolom ke 0, kolom
                // jika biaya billboard > biaya per kolom
                if (hargaBillboard.get(k) / pembagi > l) {
                    // berikan nilai sebelumnya
                    if (k == 0) {
                        // karena paling atas
                        tabel[k][l] = 0;
                    } else {
                        tabel[k][l] = tabel[k - 1][l];
                    }
                } else if (hargaBillboard.get(k) / pembagi <= l) {
                    // harga kurang dari atau sama dengan
                    if (k == 0) {
                        // karena paling atas
                        // lakukan komparasi
                        tabel[k][l] = jumlahPemirsa.get(k);
                    } else {
                        // lakukan komparasi
                        tabel[k][l] = max(
                                jumlahPemirsa.get(k) + tabel[k - 1][l - (int) (hargaBillboard.get(k) / pembagi)],
                                tabel[k - 1][l]); // ditambah sisa weight dari proses sebelumnya
                    }
                }
            }
        }

        // untuk menampilkan tabel DP
        // showTable((int) i, (int) j, tabel);

        // show product picked
        long result = tabel[(int) i - 1][(int) j];
        System.out.println(result);

        // // tidak dapat billboard karena uang kurang
        if (result == 0) {
            System.out.print(0);
            return;
        }

        // tampilkan billboard mana yang diambil
        int w = (int) j;
        for (int k = (int) i; k >= 0; k--) {
            if (k == 0) {
                // paling atas sendiri
                System.out.print(k + 1);
            } else {
                if (result == tabel[k - 1][w]) {
                    // tidak termasuk
                    continue;
                } else {
                    // karena termasuk
                    System.out.printf((k + 1) + " ");
                    result -= jumlahPemirsa.get(k);
                    w -= hargaBillboard.get(k) / pembagi;
                }
                // sudah berhenti
                if (result == 0) {
                    break;
                }
            }

        }

    }
}
```

# Knapsack dengan Exhaustive Search

```java
// Nama file: BF.java
// Deskripsi: Brute Force Algorithm for Knapsack
// Pembuat: Attaf Riski Putra Ramadhan
// Last Updated: 11 Juni 2023

import java.util.ArrayList;
import java.util.Scanner;

public class BF {
    static ArrayList<Long> jumlahPemirsa = new ArrayList<>();
    static ArrayList<Long> hargaBillboard = new ArrayList<>();
    // untuk BFKnapsack
    static long maxJumlahPemirsa = 0;
    static long maxBiaya = 0;
    static int[] bestSelection;


    public static void main(String[] args) throws Exception {
        // KAMUS
        Long anggaranBiaya;

        // ALGORITMA
         try (Scanner input = new Scanner(System.in)) {
            anggaranBiaya = input.nextLong();
            while(input.hasNextBigInteger()){
                hargaBillboard.add(input.nextLong());
                jumlahPemirsa.add(input.nextLong());

            }
        }
        // DPKnapsack(anggaranBiaya);
        int n = hargaBillboard.size();
        int[] selection = new int[n];
        bestSelection = new int[n];

        exhaustiveSearchKnapsack(anggaranBiaya, selection, 0);
        System.out.println(maxJumlahPemirsa);
        // item
        if(maxJumlahPemirsa==0){
            System.out.println(0);
        }else{
            for (int i = n-1; i >= 0; i--) {
                if(bestSelection[i]==1 && i==0){
                    System.out.print(i+1);
                }else if(bestSelection[i]==1){
                    System.out.print(i+1+" ");
                }
            }
        }

    }

    // knapsack brute force
    private static void exhaustiveSearchKnapsack(long capacity, int[] selection, int currentItem) {
        int n = hargaBillboard.size(); // sudah mencapai kondisi kombinasi yang tepat

        if (currentItem == n) {
            int totalWeight = 0;
            int totalValue = 0;

            for (int i = 0; i < n; i++) {
                totalWeight += hargaBillboard.get(i) * selection[i];
                totalValue += jumlahPemirsa.get(i) * selection[i];
            }

            if (totalWeight <= capacity && totalValue > maxJumlahPemirsa) {
                maxBiaya = totalWeight;
                maxJumlahPemirsa = totalValue;
                System.arraycopy(selection, 0, bestSelection, 0, n);
            }
            return;
        }

        // Setiap item memiliki dua kemungkinan: dipilih (1) atau tidak dipilih (0)
        // menyebabkan 2^n
        selection[currentItem] = 1;
        exhaustiveSearchKnapsack(capacity, selection, currentItem + 1);

        selection[currentItem] = 0;
        exhaustiveSearchKnapsack(capacity, selection, currentItem + 1);
    }
}

```

# Perbandingan DP VS ES

Sebelum lanjut, coba kita lihat tabel dari Software University Foundation
![](/blog/knapsack_with_DP_ES/tabelkompleksitas.jpg)

Solusi DP di atas memiliki kompleksitas waktu **O(m.n)** dimana m adalah jumlah kapasitas dan n adalah jumlah papan iklan yang termasuk kedalam kompleksitas waktu **O(nˆ2)**. Kemudian untuk solusi Exhaustive Search memiliki kompleksitas waktu **O(2ˆn)** dimana n adalah banyak papan iklan.

Jadi, apabila ada 50 papan iklan maka waktu yang dipakai oleh Exhaustive Search untuk menghasilkan solusi akan lebih dari 1000ms (1s) dan Dynamic Programming akan masih aman.

# Itu untuk sekarang!

Pepatah mengatakan **"Ada banyak solusi untuk suatu permasalah, tapi suatu solusi juga dapat mengakibatkan banyak masalah"** hhh mungkin kurang nyambung pepatahnya, tapi pepatah itu terdengar keren 😂 Seperti biasa jika kamu membaca sampai titik ini aku ucapkan terimakasih untuk waktu yang sudah kamu luangkan. Maaf kalau ada tutur kata aku yang kurang berkenan 😌

# Special Tributes

Makasih untuk [gambarnya](https://lovepik.com/images/png-yellow-knapsack.html")
