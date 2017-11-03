# Tugas 2 PKSJ | Uji Ketahanan WordPress terhadap SQL Injection

## 1. Pendahuluan

## 2. Dasar Teori

### 2.1. Ubuntu Server Xenial 16.04

### 2.2. WordPress

### 2.3. SQL Injection

### 2.4. Plugin WordPress

#### 2.4.1. League Manager

##### Deskripsi

This Plugin is designed to manage sports leagues and display them on your blog.

Version : 4.2-RC1.3

Author  : Kolja Schleich, LaMonte Forthun

##### Features

- easy adding of teams and matches
- fancy slideshows of previous or next matches in combination with the Fancy Slideshow Plugin)
- numerous point-rules implemented to also support special rules (e.g. Hockey, Pool, Baseball, Cornhole)
- weekly-based ordering of matches with bulk editing mechanism
- automatic or manual saving of standings table
- automatic or drag & drop ranking of teams
- link posts with specific match for match reports
- unlimited number of widgets
- modular setup for easy implementation of sport types
- separate capability to control access and compatibility with Role Manager
- dynamic match statistics
- Championship mode with preliminary and k/o rounds

#### 2.4.2. Spider Video Player

##### Deskripsi

Spider Video Player is highly customizable. You can choose to customize the library, videos and frames background color adjustment features for the video player, the height and width of the video player, transparency level for the video player buttons and more.

Spider video player has wonderful flash effects. You can add several video players in one page with different parameters and playlists. With Spider video player you can have unlimited playlists with unlimited number of videos.

Spider Video Player supports types of videos, such as Http, YouTube, Vimeo and rtmp

The WordPress plugin comes with the image watermark support for the video player (in Flash mode). You can choose to modify the parameters of the watermark image, including the size, position and border spacing.

Version: 1.5.20

Author: [WebDorado](https://web-dorado.com/)

### 2.5. SQL Injection Testing Tools for WordPress

#### 2.5.1. WPScan

WPScan is a WordPress vulnerability scanner written in Ruby. It is sponsored by Sucuri and hosted on github. Using its security vulnerability database for WordPress core, plugins and themes it will provide a report on your site’s known security problems which can be exploited by hackers.

#### 2.5.2. sqlmap

sqlmap is an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database servers. It comes with a powerful detection engine, many niche features for the ultimate penetration tester and a broad range of switches lasting from database fingerprinting, over data fetching from the database, to accessing the underlying file system and executing commands on the operating system via out-of-band connections.

## 3. Instalasi

## 3.1. WordPress

###### 1. Install dan konfigurasi DBMS

Sistem manajemen database yang digunakan adalah MySQL Server. Instalasi dilakukan dengan perintah:
```
sudo apt-get install mysql-server
```
Selama instalasi, sistem akan meminta password untuk user root pada mysql.

Setelah MySQL server terinstal, selanjutnya masuk ke console mysql dengan perintah:
```
mysql -uroot -p
```
Buat database untuk wordpress sekaligus username khusus yang nanti akan digunakan oleh WordPress:
```
CREATE DATABASE wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO 'username'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

###### 2. Install dan konfigurasi web server beserta PHP dan ekstensi yang dibutuhkan

Web server yang digunakan di sini adalah Apache2. Untuk melakukan instalasi gunakan perintah:
```
sudo apt-get update
sudo apt-get install apache2 php php-mcrypt php-mysql php-curl php-gd php-mbstring php-mcrypt php-xml php-xmlrpc libapache2-mod-php
```
Setelah dilakukan instalasi, buka file `/etc/apache2/mods-enabled/dir.conf`. Temukan baris berikut:
```
<IfModule mod_dir.c>
  DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```
Lalu ubah baris tersebut menjadi seperti ini agar web service membaca `index.php` lebih dulu daripada `index.html`:
```
<IfModule mod_dir.c>
  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

Terdapat opsi tambahan jika ingin file `.htaccess` yang ada di direktori aplikasi WordPress diberlakukan. Yaitu dengan menambahkan konfigurasi pada file `/etc/apache2/apache2.conf`:
```
. . .

<Directory /var/www/html/>
  AllowOverride All
</Directory>

. . .
```
Kemudian simpan perubahan. Agar fungsi permalink WordPress dapat bekerja, aktifkan module rewrite apache:
```
sudo a2enmod rewrite
```
Jika semua konfigurasi yang dibutuhkan telah dilakukan, langkah terakhir adalah melakukan restart service apache:
```
sudo service apache2 restart
```

###### 3. Download WordPress

Setelah server terkonfigurasi, langkah selanjutnya adalah *set up* WordPress.

Pertama

## 3.2. Plugin WordPress

## 3.3. *SQL Injection Testing Tools*

## 3.3.1. WPScan

## 3.3.2. sqlmap

## 4. Uji Penetrasi

## 5. Kesimpulan dan Saran

## 6. Sumber

http://sqlmap.org/

https://guides.wp-bullet.com/install-wpscan-ubuntu-16-04-wordpress-vulnerability-scanning/

https://firstsiteguide.com/tools/hacked-dangerous-vulnerable-wordpress-plugins/

https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04

https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lamp-on-ubuntu-16-04
