# Jarkom-Modul-2-C15-2021

## Nomor 8

Konfigurasi server yang telah dibuat pada **nomor 1-7** akan dilanjutkan dengan konfigurasi Webserver. Pertama dengan webserver `www.franky.yyy.com`. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada `/var/www/franky.yyy.com`.

### Jawaban

#### Skypie

Lakukan instalasi Apache, PHP, serta libapache2-mod-php7.0.

```
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y

```

Lakukan start apache pada command :

`service apache2 start`

Pindah directory ke `/etc/apache2/sites-available` dengan command :

`cd /etc/apache2/sites-available`

Kemudian, buat config website franky dengan menyalin dari template yang sudah ada :


`cp 000-default.conf franky.c15.com.conf`

Buka `conf`-nya dengan `nano franky.c15.com.conf` dan tambahkan sebagai berikut :

![no-8-conf](https://user-images.githubusercontent.com/64303057/139532657-b8932df6-9c50-4a90-9042-aecc32f0478d.jpeg)

Aktifkan konfigurasi franky.c15.com dengan `a2ensite franky.c15.com` dan restart apache `service apache2 restart`.

Kita juga perlu download file `franky.zip` yang ada pada Github dengan `wget` sebagai berikut :

`wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip`

Lakukan unzip dengan `unzip franky.zip` dan copy file yang ada di dalam folder `franky` setelah di unzip ke directory `/var/www/franky.c15.com`.

Periksa kembali (untuk mengecek apakah ada `home.html` & `index.php`) dengan :
`ls /var/www/franky.c15.com`

#### Loguetown

Pertama, install lynx terlebih dulu dengan `apt-get install lynx -y`.

Jika sudah, lakukan command `lynx.www.franky.c15.com` dan akan mengeluarkan output :

![lynx-franky-8](https://user-images.githubusercontent.com/64303057/139533857-0c7e59c3-685e-4838-863f-9475dad60503.jpeg)


## Nomor 9

Setelah itu, Luffy juga membutuhkan agar url `www.franky.yyy.com/index.php/home` dapat menjadi menjadi `www.franky.yyy.com/home`. 

### Jawaban
#### Skypie

Jalankan perintah `a2enmod rewrite` untuk mengaktifkan module rewrite dan restart kembali Apache dengan `service apache2 restart`.


Terkadang, ada sebuah kasus bahwa hak akses root untuk mengedit file konfigurasi yang berada di folder `/etc/apache2/sites-available` tidak dimiliki, atau kita tidak ingin user lain untuk mengedit file konfigurasi yang berada di directory `/etc/apache2/sites-available`.


Untuk mengatasinya, kita perlu buat file `.htaccess` pada folder `/var/www/franky.c15.com` dan tambahkan seperti ini :

![rewrite](https://user-images.githubusercontent.com/64303057/139533529-90e6ef27-6fd2-4012-92c7-92af573e7387.jpeg)

Kembali ke directory `/etc/apache2/sites-available` dan edit file `franky.c15.com.conf` (agar file `.htaccess` dapat berjalan) sebagai berikut :

![conf-htaccess](https://user-images.githubusercontent.com/64303057/139533681-249f54a3-ecd7-40bc-adaa-e8a36e08ee9b.jpeg)

Kembali lakukan restart apache `service apache2 restart`

#### Loguetown

Cek apakah config untuk rewrite berhasil dengan command :


`lynx www.franky.c15.com/home`

Hasilnya adalah sebagai berikut :

![lynx-9](https://user-images.githubusercontent.com/64303057/139533826-0dce8c9c-c522-4887-aeb5-ff51952983f7.jpeg)

## Soal 10

Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache

### Jawaban

#### Skypie

Pengerjaan nomor 10 tidak beda jauh dengan nomor 8, hanya domain (subdomain) -nya saja yang diubah menjadi `www.super.franky.yyy.com`.

Pertama, pindah ke directory /etc/apache2/sites-available dan copy file template `000-default.conf` menjadi file baru dengan command :

`cp 000-default.conf super.franky.E08.com.conf`

Tambahkan juga pada `super.franky.c15.com.conf` sebagai berikut :

![1-nomor-10-conf](https://user-images.githubusercontent.com/64303057/139534575-65a57515-0c80-4d52-8029-ed3873a06a5b.jpeg)

Aktifkan konfigurasi dengan `a2ensite super.franky.c15.com` dan restart apache `service apache2 restart`.

Pindah dengan `cd /var/www` dan download file zip dengan command :

`wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip`

Unzip dengan `unzip super.franky.zip`. Rename juga foldernya menjadi `super.franky.c15.com` yang berisi file `error` & `public`.

#### Loguetown

Lakukan `lynx www.super.franky.c15.com`, hasil output adalah :

![10-lynx](https://user-images.githubusercontent.com/64303057/139535601-a6ca161b-06f6-45fd-9020-0f808905ec09.jpeg)

## Soal 11

Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

### Jawaban
#### Skypie

Kembali ke directory letak config dengan `cd /etc/apache2/sites-available`

Tambahkan pada file `super.franky.c15.com.conf` sebagai berikut :

![11-superfranky-conf1](https://user-images.githubusercontent.com/64303057/139536144-aa5faebf-6ec4-402f-ac3e-9cbfd5825636.jpeg)

Pada `.../public/*`, diberikan `Options -Indexes` agar mengeluarkan error apabila nanti melakukan lynx ke `.../public/...`.

Lakukan restart apache `service apache2 restart`.

#### Loguetown

Lakukan `lynx www.super.franky.c15.com/public`, hasil output adalah :

 ![hasil-public](https://user-images.githubusercontent.com/64303057/139536469-25de794a-91ac-48b6-a68b-805563ff9def.jpeg)
 
 Kemudian, di bawah ini adalah output jika mengakses `.../public/css`, `.../public/images`, dan `.../public/js` menggunakan lynx.
 
![gagal-unduh](https://user-images.githubusercontent.com/64303057/139536522-d0ae0cb6-82d5-4c66-aa70-b6cafb0e8928.jpeg)

## Soal 12
Tidak hanya itu, Luffy juga menyiapkan error file `404.html` pada folder `/error` untuk mengganti error kode pada apache.

### Jawaban
#### Skypie

Kembali ke directory letak config dengan `cd /etc/apache2/sites-available`.

Tambahkan `Error Document` pada file `super.franky.c15.com.conf` sebagai berikut :

![11-superfranky-conf2-error](https://user-images.githubusercontent.com/64303057/139536666-5ffa8e71-dae4-4f20-aba2-eb935cf32897.jpeg)

Lakukan restart apache `service apache2 restart`.

#### Loguetown

Lakukan `lynx www.super.franky.c15.com/publiccc` (disengaja adanya typo), hasil output adalah :

![messageImage_1635595132257](https://user-images.githubusercontent.com/64303057/139536875-4818850a-9a3c-4820-9606-657d4267f4b3.jpeg)

## Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset `www.super.franky.yyy.com/public/js` menjadi `www.super.franky.yyy.com/js`. 

### Jawaban
#### Skypie

Kembali ke directory letak config dengan `cd /etc/apache2/sites-available`.

Tambahkan `Alias untuk js` pada file `super.franky.c15.com.conf` sebagai berikut :

![11-superfranky-conf](https://user-images.githubusercontent.com/64303057/139537070-27f83462-8da1-453d-bcd7-1d08242af3d0.jpeg)

Lakukan restart apache `service apache2 restart`.

#### Loguetown

Lakukan `lynx www.super.franky.c15.com/js`, hasil output adalah :

![messageImage_1635595274159](https://user-images.githubusercontent.com/64303057/139537159-d2560e74-5965-4f64-8372-464c591ba1bd.jpeg)






