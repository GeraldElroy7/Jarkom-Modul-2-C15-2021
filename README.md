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

<br>

# Nomor 14
Luffy meminta agar ```www.general.mecha.franky.c15.com``` agar hanya dapat diakses oleh port ```15000``` dan ```15500```

Langkah pengerjaan:
- Pada node Skypie
  - pilih direktori ```/etc/apache2/sites-available```
  - copy file ```000-default.conf``` dan ubah nama file menjadi ```general.mecha.franky.c15.com.conf```
  - ubah port menjadi 15000 dan 15500
  - ![image](https://user-images.githubusercontent.com/90212308/139532434-23e92f5d-d287-45cd-8f30-6ee9f168f6a5.png)

  - kemudian pada direktori ```/etc/apache/``` dan ubah pada file ```ports.conf```
  - ![image](https://user-images.githubusercontent.com/90212308/139532566-c69dfacf-de8b-4cf7-a372-24cac803898b.png)
  - ubah port yang di listen seperti gambar diatas
  - kemudian aktifkan konfigurasi dengan ```a2ensite general.mecha.franky.c15.com.conf```
  - setelah konfigurasi diaktifkan restart apache ```service apache2 restart```

  - pindah direktori ke ```/var/www```
  - download file yang telah disediakan dengan command ```wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/general.mecha.franky.zip```
  - unzip file ```unzip general.mecha.franky.c15.com```
  - kemudian hasil dari file zip yang telah di unzip tersebut di rename menjadi ```general.mecha.franky.c15.com``` menggunakan command ```mv general.mecha.franky general.mecha.franky.c15.com```

 ### Hasil
 - Testing pada Node Loguetown
  - ```lynx general.mecha.c15.com:15000```
  - ![image](https://user-images.githubusercontent.com/90212308/139533130-c6f38aba-24b5-44bf-b771-63c73e5a37aa.png)

<br>

# Nomor 15
memberi autentikasi pada file ```/var/www/general.mecha.franky.c15.com```

Langkah pengerjaan:
- Node Skypie
  - edit file pada ```/etc/apache2/sites-available/general.mecha.franky.c15.com``` seperti gambar diabawah ini
  - ![image](https://user-images.githubusercontent.com/90212308/139533384-34ca6f33-bdf9-4d3e-a793-facfccfa9404.png)

  - Kemudian jalankan command ```htpasswd -c /etc/apache2/.htpasswd luffy``` dan isikan password ```onepiece```
  - restart apache ```service apache2 restart```

### Hasil 
- Testing pada Node Loguetown
- jalankan command ```lynx www.general.mecha.franky.c15.com:15000```
- ![image](https://user-images.githubusercontent.com/90212308/139533542-77aea596-d04b-432e-b444-9db479939677.png)
- muncul interface sebagai berikut dan isikan username dengan ```luffy```
- ![image](https://user-images.githubusercontent.com/90212308/139533651-09aaf168-08f1-4155-8cd1-5812f6572899.png)
- dan isikan password tersebut dengan ```onepiece```
- ![image](https://user-images.githubusercontent.com/90212308/139533686-c2d265d6-9d8c-43cb-978f-1474ed52b081.png)

Kendala yang dialami:
- Penerapan directory listing
- pada ```htpasswd```

<br>

# Nomer 16
Ketika mengakses IP Skypie maka akan dialihkan ke www.franky.c15.com

Langkah pengerjaan :
- pada node Skypie
  - Edit file ``` nano /etc/apache2/sites-available/000-default.conf``` menjadi seperti berikut
  - ![image](https://user-images.githubusercontent.com/90212308/139533891-7255b1ee-37b8-4f23-9411-63767d1636ab.png)
  - service apache2 restart

### Hasil 
- Testing pada node Loguetown:
    - akses IP Skypie dengan command ```192.191.2.4```
    - maka akan dialihkan ke ```www.franky.c15.com```
    - ![image](https://user-images.githubusercontent.com/90212308/139533979-b4f05efc-a6dc-4c83-9efc-bba83abb800c.png)
Kendala yang dialami:
 -






