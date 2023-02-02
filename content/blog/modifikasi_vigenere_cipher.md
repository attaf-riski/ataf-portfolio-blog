---
title: "Modifikasi Enkripsi Vigenere Cipher"
description: "Meningkatkan keamanan enkripsi vigenere cipher"
dateString: Februari 2022 ·
draft: false
tags: ["encryption", "python"]
showToc: false
weight: 209
cover:
  image: "/blog/modifikasi_vigenere_cipher/cover.png"
  caption: "Made by Ataf with Krita"
---

# Pendahuluan

Pernah terbayang bagaimana _password_ kamu diamankan di dalam server? Atau pernah kepikiran bagaimana pesan whatsapp kamu dikirim secara aman? Enkripsi jawabannya! Enkripsi yang menjadi standar dunia digital saat ini merupakan jenis yang cukup aman dan terbaru. Nah, pada kesempatan ini aku mau berbagi tentang modifikasi enkripsi klasik yang tentunya lebih mudah daripada enkripsi modern yaa, namanya adalah enkripsi vigenere cipher 🔑.

Vigenere Cipher adalah salah satu teknik enkripsi klasik yang diciptakan pada tahun 1553 oleh Blaise de Vigenere, seorang matematikawan Prancis. Selama berabad-abad, teknik ini telah digunakan untuk menyandikan pesan rahasia dan banyak dianggap sebagai salah satu cipher yang paling tidak dapat ditembus. _Keren ngga tuh_!

# Cara Kerja Enkripsi Vigenere Cipher

Cara kerjanya adalah tiap huruf pada kalimat yang akan dienkripsi memiliki nilai _ascii_, nilai tersebut akan ditambah dengan nilai _ascii_ dari kunci lalu di modulus 256. Tunggu kenapa di modulus? Hal tersebut karena kali ini aku akan memakai kode _ascii_ yang jumlah hurufnya 256 kalau lebih dari itu ngga di hitung dulu hurufnya.

## Enkripsi

![](/blog/modifikasi_vigenere_cipher/enkripsi.png)

## Dekripsi

![](/blog/modifikasi_vigenere_cipher/dekripsi.png)

Untuk mendekripsi atau mengubah dari pesan enkripsi ke pesan asli caranya dengan mengurangi nilai _ascii_ tiap huruf enkripsi dengan nilai kunci lalu di modulus 256.

Kali ini aku pakai bahasa pemrograman _python_ untuk mengimplementasikan algoritma dari enkripsi vigenere cipher. Biasanya kalau lihat kodenya atau lihat praktiknya bakalan lebih mudah ya,

## Enkripsi

```python
# Fungsi Enkripsi
def cipherText(message, key_new):
# Kamus lokal
    cipher_text = ''
    i = 0
# Algoritma
    for letter in message:
        x = ord(letter)+(ord(key_new[i])) % 256
        cipher_text += chr(x)
        i += 1
    return cipher_text
```

## Dekripsi

```python
# Fungsi Dekripsi
def originalText(cipher_text, key_new):
# Kamus lokal
    or_txt = ''
    i = 0
# Algoritma
    for letter in cipher_text:
        x = ord(letter)-(ord(key_new[i])+256) % 256
        or_txt += chr(x)
        i += 1
    return or_txt
```

## Menghasilkan Kunci

Kunci dalam vigenere cipher adalah kunci, _lohe kok aneh_. hhh. Tapi memang benar kunci dalam algoritma enkripsi ini sangat penting. Lebih baik terlihat pesannya daripada terlihat kuncinya.
![](/blog/modifikasi_vigenere_cipher/kunci.jpg)

Kunci dimasukan oleh pemrograman yang kemudian akan disesuaikan panjangnya dengan pesan asli agar semua huruf tercakup.

```python
# Membuat kunci baru
def generate_key(message, key):
# Kamus Lokal
    i = 0
# Algoritma
    while True:
        if len(key) == len(message):
            break
        else:
            key += (key[i% len(key)])
            i += 1
    print("key: ",key)
    return key
```

## Kenapa Perlu Memofifikasi Vigenere Cipher?

Pertanyaan bagus, masalah yang akan muncul ketika menggunakan enkripsi vigenere cipher adalah pesan cipher dapat memiliki pola kalau pesannya memiliki huruf yang sama dan itu adalah hal yang tidak bagus.
![](/blog/modifikasi_vigenere_cipher/kurang.png)

# Modifikasi Enkripsi Vigenere Cipher

Berangkat dari kekurangan vigenere cipher aku bakal modif ini algoritma, konsepnya nanti membagi nilai suatu huruf dipesan menjadi dua bagian yang apabila dijumlahkan akan menghasilkan nilai asli itu sendiri. Lalu nilainya dapat dari mana? Dengan cara menghasilkannya secara random antara 0 - nilai asli. Lalu nilai asli dikurangi nilai random tersebut sehingga dapat menghasilkan pesan yang lebih panjang dua kali.

## Enkripsi

```python
# buat original text 2X lipat lebih panjang
    for letter in message:
      z = ord(letter)
      nil_random = random.randint(1, z)
      modif_ori += chr(nil_random)
      modif_ori += chr(z-nil_random)
```

Baru setelah itu dienkripsi seperti biasa seperti vigenere cipher, namun tentunya dengan modifikasi sedikit agar satu kunci bisa untuk dua huruf

## Dekripsi

Untuk dekripsinya, prosesnya dibalik dari proses enkripsi. Memakai satu kunci untuk dua huruf cipher text baru setelah itu digabung.

```python
# Fungsi Dekripsi
def originalText(cipher_text, key_new):
# Kamus lokal
    or_txt = ''
    i = 0
    modif_ori = '';
# Algoritma
    # deksripsi seperti biasa
    for j in range(1,len(cipher_text),2):
            #pakai satu kunci untuk 2 original text
            for k in range(2):
              x = (ord(cipher_text[j+k-1])-(ord(key_new[i])+256)) % 256
              or_txt += chr(x)

            i += 1
    # jadikan or_txt 2 kali lebih kecil
    for r in range(0,len(or_txt),2):
          modif_ori += chr(ord(or_txt[r])+ord(or_txt[r+1]))

    return modif_ori
```

> Untuk proses menghasilkan kunci sama saja

## Kelebihan

Dengan menggunakan algoritma ini, pesan cipher tidak akan memiliki pola dan akan random setiap dihasilkan!

![contoh 1:](/blog/modifikasi_vigenere_cipher/test1.png)

![contoh 2:](/blog/modifikasi_vigenere_cipher/test2.png)

![contoh 3:](/blog/modifikasi_vigenere_cipher/test3.png)

## Vigenere Cipher Asli Python

```python
# Nama file: vigenere-cipher.ipynb
# Deskripsi: modul vigenere-cipher tanpa autokey
# Pembuat: Attaf Riski
# Tanggal: 9 Des 2022

import random

# Membuat kunci baru
def generate_key(message, key):
# Kamus Lokal
    i = 0
# Algoritma
    while True:
        if len(key) == len(message):
            break
        else:
            key += (key[i% len(key)])
            i += 1
    print("key: ",key)
    return key


# Fungsi Enkripsi
def cipherText(message, key_new):
# Kamus lokal
    cipher_text = ''
    i = 0
# Algoritma
    for letter in message:
        x = ord(letter)+(ord(key_new[i])) % 256
        cipher_text += chr(x)
        i += 1
    return cipher_text


# Fungsi Dekripsi
def originalText(cipher_text, key_new):
# Kamus lokal
    or_txt = ''
    i = 0
# Algoritma
    for letter in cipher_text:
        x = ord(letter)-(ord(key_new[i])+256) % 256
        or_txt += chr(x)
        i += 1
    return or_txt



def main():
    message = 'aaaaaaaaaaaaaaaaa'
    key = 'februari'
    key_new = ''
    if(len(key)>len(message)):
      key_new = key[:len(message)]
    elif(len(key)<len(message)):
      key_new = generate_key(message, key)
    else:
      key_new = key

    cipher_text = cipherText(message, key_new)
    original_text = originalText(cipher_text, key_new)
    print("Teks Cipher =", cipher_text)
    print("Teks Asli =", original_text)


# Executes the main function
if __name__ == '__main__':
    main()
```

## Vigenere Cipher Modifikasi

```python
# Nama file: modifikasi-vigenere-cipher.ipynb
# Deskripsi: tugas akhir KJI > modul modifikasi vigenere cipher
# Pembuat: Attaf Riski
# Tanggal: 8 Des 2022

import random

# Membuat kunci baru
def generate_key(message, key):
# Kamus Lokal
    i = 0
# Algoritma
    while True:
        if len(key) == len(message):
            break
        else:
            key += key[i % len(key)]
            i += 1
    print("key: ",key)
    return key


# Fungsi Enkripsi
def cipherText(message, key_new):
# Kamus lokal
    cipher_text = ''
    i = 0
    nil_random = 0;
    modif_ori = ''
# Algoritma
    # buat original text 2X lipat lebih panjang
    for letter in message:
      z = ord(letter)
      nil_random = random.randint(1, z)
      modif_ori += chr(nil_random)
      modif_ori += chr(z-nil_random)
    # enkripsi seperti biasa
    for j in range(1,len(modif_ori),2):
            #pakai satu kunci untuk 2 original text
            for k in range(2):
              x = (ord(modif_ori[j+k-1])+(ord(key_new[i]))) % 256
              cipher_text += chr(x)
            i += 1
    return cipher_text


# Fungsi Dekripsi
def originalText(cipher_text, key_new):
# Kamus lokal
    or_txt = ''
    i = 0
    modif_ori = '';
# Algoritma
    # deksripsi seperti biasa
    for j in range(1,len(cipher_text),2):
            #pakai satu kunci untuk 2 original text
            for k in range(2):
              x = (ord(cipher_text[j+k-1])-(ord(key_new[i])+256)) % 256
              or_txt += chr(x)

            i += 1
    # jadikan or_txt 2 kali lebih kecil
    for r in range(0,len(or_txt),2):
          modif_ori += chr(ord(or_txt[r])+ord(or_txt[r+1]))

    return modif_ori



def main():
    message = 'aaaaaaaaaaaaaaaaa'
    key = 'februari'
    key_new = ''
    if(len(key)>len(message)):
      key_new = key[:len(message)]
    elif(len(key)<len(message)):
      key_new = generate_key(message, key)
    else:
      key_new = key

    cipher_text = cipherText(message, key_new)
    original_text = originalText(cipher_text, key_new)
    print("Teks Cipher =", cipher_text)
    print("Teks Asli =", original_text)


# Executes the main function
if __name__ == '__main__':
    main()
```

# Itu Sudah Semua!

Kalau mau lihat [colab](https://colab.research.google.com/drive/1XIt_zEsT1KNpC8OODfUVSUBqwIbfFc6j?usp=sharing).
Jika kamu membaca sampai titik ini aku ucapkan terimakasih, dengan ini juga kamu telah mempelajari salah satu jenis enkripsi klasik, kalau kamu tertarik coba deh cari _keyword_ ini di google: autokey dan enkripsi modern. Maaf kalau ada tutur kata yang kurang berkenan 😌. Thanks Chat GPT for the layout ❤️.
