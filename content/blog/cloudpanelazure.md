---
title: "Langkah Mudah Memasang CloudPanel di Azure: Panduan Lengkap untuk Mahasiswa"
description: ""
dateString: Januari 2024 ·
draft: false
tags: ["cloud", "vps", "azure", "cloudpanel"]
showToc: false
weight: 207
cover:
  image: "/blog/cloudpanelazure/cover.png"
  caption: "Cover Made by Ataf with Figma (cloudpanel logo towards azure logo)"
---

# 📌 Prasyarat

1. Memiliki Paket Azure for Student (kalau banyak yang komentar nanti dibuatkan tutorialnya, minimal 5 komentar 😁)

# 📌 Apa itu CloudPanel dan Azure?

![question meme](/blog/cloudpanelazure/questionmeme.png)

Kehidupan selalu dipenuhi dengan tanda tanya, tapi seseorang pernah mengatakan

> "hidup adalah keberanian menghadapi tanya tanya".

Mungkin muncul pertanyaan di benak kamu apa itu cloudpanel dan azure, agar gundah gelisah dari pertanyaan itu terjawab ayo kita simak penjelasan berikut wkwk 😆

Oiya panduan ini akan cukup memakan waktu jadi sediakan camilan atau minuman biar kamu nggak bosan ya 😁 Let's go❗️

Langsung aja ya, CloudPanel itu adalah kontrol panel yang bisa dengan mudah kita gunakan untuk mengontrol berbagai layanan di cloud. Ngga cuma itu CloudPanel juga bisa memungkinkan kamu untuk mengatur dan memantau sumber daya cloud dan lagi CloudPanel ini gratis lhoo 🤩 Kalau mau mengunjungi website-nya langsung aja ke [sini ya!](https://www.cloudpanel.io/)

Lanjut ya, Lalu apa itu Azure? Azure itu adalah platform cloud computing milik Microsoft yang intinya Azure menyediakan berbagai layanan komputasi, penyimpanan, dan jaringan yang dapat digunakan untuk mengembangkan, menguji, dan memelihara aplikasi melalui infrastruktur awan Microsoft. Azure sendiri sebenarnya berbayar, namun, Azure menyediakan fasilitas berupa Azure for Student sehingga bisa gratis lho layanan Azure khusus untuk pelajar. Untuk mendapatkan Azure gratis tidak akan dibahas di panduan ini ya 😁

# 📌 Langkah-Langkah Memasang CloudPanel di Azure

## 📌📌 Get Started!

![Get started!](/blog/cloudpanelazure/getstarted.png)

Ketika kamu sampai di halaman utama [cloudpanel.io](https://www.cloudpanel.io/) klik tombol **Get Started**.

![Installation!](/blog/cloudpanelazure/installation.png)

Selanjutnya, pilih **Installation on Microsoft Azure**, terdapat berbagai pilihan platform cloud computing lainnya juga tapi di panduan ini akan menggunakan Microsoft Azure.

Di dalam halaman yang kamu buka juga terdapat panduan yang bisa kamu pilih, namun terdapat sedikit perbedaan pada tutorial ini khususnya di bagian penggunaan password daripada SSH public key.

> Ketika panduan ini dibuat (Januari 2024) mungkin konfigurasinya akan berbeda dengan konfigurasi terbaru pada halaman cloudpanel yang kamu buka. Ikuti saja panduan yang ada di cloudpanel karena itu yang terbaru.

## 📌📌 Pilih Azure Virtual Machine

![Choose Azure Virtual Machine!](/blog/cloudpanelazure/azurevm.png)
Buka Portal Azure, lalu pilih Azure Virtual Machine di layanan Virtual Machine ya.

## 📌📌 Konfigurasi Instance VM

![configure instance vm!](/blog/cloudpanelazure/konfigurasiinstance.png)

Untuk konfigurasi intance vm disamakan saja ya.

## 📌📌 Konfigurasi Administrasi

![configure password!](/blog/cloudpanelazure/konfigurasiadmin.png)

Untuk konfigurasi Administrasi terdapat dua pilihan yaitu SSH public key atau Password. Antara SSH public key dengan password lebih aman SSH public key seharusnya, namun karena panduan ini adalah panduan yang mudah sehingga kita akan menggunakan password saja.

## 📌📌 Konfigurasi Inbound Port Rules

![configure inbound!](/blog/cloudpanelazure/konfigurasiinbound.png)

Untuk port yang bisa diakses yaitu port 80 (http), 443 (https), dan 22(ssh).

## 📌📌 Atur Disk

![configure disk!](/blog/cloudpanelazure/aturdisk.png)

Untuk pengaturan disk-nya bisa disamakan saja ya. Lalu klik create + review.

![create!](/blog/cloudpanelazure/createvm.png)

Terakhir kita review bila sudah cocok maka langsung saja kita klik create.

## 📌📌 Masuk Network

![klik vm!](/blog/cloudpanelazure/klikvm.png)
Klik VM yang baru kamu buat tadi.
![network setting!](/blog/cloudpanelazure/networksetting.png)
Pergi di bagian setting network pada sidebar.

## 📌📌 Setting Inbound Port Rule

![create rule port !](/blog/cloudpanelazure/createruleport.png)
![konfigure inbound port!](/blog/cloudpanelazure/konfigurasiinboundport.png)
Untuk konfgurasi inbound port bisa disamakan saja ya.

## 📌📌 Login Terminal

![login instance di terminal!](/blog/cloudpanelazure/terminalinstance.png)
Selanjutnya kita akan masuk instance VM azure tadi lewat SSH, masukkan seperti di gambar atas ya, untuk kodenya

```bash
ssh azure@yourIpAddress
```

Untuk Mendapatkan Ip address bisa di dapatkan dari informasi VM masing-masing.
![ip publik!](/blog/cloudpanelazure/dapatkanippublik.png)

Apabila ada error ketika pertama kali melakukan koneksi dengan ssh maka coba ulangi lagi.

> Ketika Memasukkan password, maka tidak akan terlihat password terketik

![welcome!](/blog/cloudpanelazure/welcome.png)

Setelah masuk instance vm lewat SSH, masukkan kode ini untuk menjadi root:

```bash
sudo su root
```

Lalu lakukan update package di VM

```bash
apt update && apt -y upgrade && apt -y install curl wget sudo
```

Kemudian ketika tampil halaman seperti ini klik OK

> (gunakan tombol tab untuk menggeser)

![Konfigurasi package config](/blog/cloudpanelazure/packageconfig.png)

## 📌📌 Install CloudPanel dan MySQL di Instance VM

Masih di instance VM yang terhubung lewat SSH, masukkan kode ini untuk install cloud dengan MYSQL:

```bash
curl -sS https://installer.cloudpanel.io/ce/v2/install.sh -o install.sh; \
echo "85762db0edc00ce19a2cd5496d1627903e6198ad850bbbdefb2ceaa46bd20cbd install.sh" | \
sha256sum -c && sudo CLOUD=msa bash install.sh
```

## 📌📌 Cek Keaktifan CloudPanel

Terakhir mari kita cek keaktifan cloudpanel yang telah kita atur.
Pertama coba buka lewat browser, jangan lupa tambahkan https di depannya.
![Masuk Lewat Browser](/blog/cloudpanelazure/masukcloudpanel.png)
![Lewati keamanan browser 1](/blog/cloudpanelazure/koneksi1.png)
![Lewati keamanan browser 2](/blog/cloudpanelazure/koneksi2.png)

## 📌📌 Setting Admin

Ketika pertama kali login di cloudpanel maka kita harus mengatur admin user
![Konfigurasi Admin User](/blog/cloudpanelazure/isiadmin.png)

Isilah konfigurasi admin sesuai yang kamu mau, tapi jangan lupa dicatat.
Jika berhasil maka tampilan halaman awalnya seperti ini
![Konfigurasi Admin User](/blog/cloudpanelazure/berhasillogin.png)

Untuk login kembali ke halaman cloudpanel kamu bisa masuk ke alamat **ippublickamu:8443** lalu masukkan username dan password yang telah kamu atur di halaman setting admin tadi yaa.

# 📌 Akhirnya...

CloudPanel akhirnya telah terinstall di Azure kamu, sekarang kamu bisa menggunakan cloudpanel untuk memasang berbagai macam aplikasi seperti wordpress, nodejs, dan masih banyak lagi aplikasi yang bisa kamu coba. Kalau ada pertanyaan tentang panduan ini bisa langsung saja tanyakan kepadaku yaaa 😆 Selanjutnya Panduan apa lagi ya?
