# Tugas 2 PKSJ | Uji Ketahanan WordPress terhadap SQL Injection

## 1. Pendahuluan

## 2. Dasar Teori

### 2.1. Ubuntu Server Xenial 16.04

Ubuntu adalah sistem operasi berbasis Linux Debian. Ubuntu adalah distribusi yang paling populer karena dukungan serta komunitasnya. Sistem operasi ini tidak berbayar alias gratis. Ubuntu Xenial adalah salah satu versi Ubuntu yang rilis pada April 2016.

### 2.2. WordPress

Wordpress adalah _content management system_ berbasis PHP dan MySQL. Wordpress adalah CMS yang paling populer dan memiliki lebih dari 45000 plugin. Wordpress digunakan untuk berbagai kebutuhan, seperti website profil instansi, jual beli, galeri, dan forum.

### 2.3. SQL Injection

SQL Injection adalah teknik serangan dengan cara memasukkan query SQL dari _client_. Jika berhasil, serangan ini dapat membaca, memperbaharui, dan menghapus database yang terdapat pada server. 

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

Version : 1.5.20

Author  : [WebDorado](https://web-dorado.com/)

#### 2.4.3. User Access Manager

##### Deskripsi

The “User Access Manager”-plugin for WordPress allows you to manage the access of your content. This is useful if you need a member area, a private section at your blog or you want that other people can write at your blog but not everywhere. Including all post type (post, pages etc.), taxonomies (categories etc.) and files by creating user groups. Just assign the content you want to restrict und and your registered users which should have a access to a group. From now on the content is only accessible and writable for the specified group.

Version : 2.1.10

Author  : Alexander Schneider

#### 2.4.4. Advanced Access Manager

##### Features

- [free] Backend Lockdown. Restrict access to your website backend side for any user or role.
- [free] Secure Login Widget & Shortcode. Drop AJAX login widget or shortcode anywhere on your website.
- [limited] Content Access. Very granular access to unlimited number of post, page or custom post type (19 different options). With premium Plus Package extension also manage access to hierarchical taxonomies or setup the default access to all post types and taxonomies.
- [free] Content Filter. Filter or replace parts of your content with AAM shortcodes.
- [free] Login/Logout Redirects. Define custom login and logout redirect for any user or role.
- [free] 404 Redirect. Redefine where user should be redirected when page does not exist.
- [free] Access Denied Redirect. Define custom redirect for any role, user or visitors when access is denied for restricted area on your website.
- [free] Manage Roles & Capabilities. Manage all your WordPress role and capabilities.
- [free] Manage Backend Menu. Manage access to the backend menu for any user or role.
- [free] Manage Metaboxes and Widgets. Filter out restricted or unnecessary metaboxes and widgets on both frontend and backend for any user, role or visitors.
- [paid] Manage Access Based On Geo Location or IP. Manage access to your website for all visitors based on referred host, IP address or geographical location.
- [paid] Monetize Access To You Content. Start selling access to your website content with premium E-Commerce extension.
- [free] Multisite Support. Sync access settings across your network or even restrict none-members from accessing one of your sites.
- etc.

Version: 5.0.2

Author: Vasyl Martyniuk

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

Pertama unduh file WordPress ke dalam direktori yang *writable*, misalkan `/tmp`:
```
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
```
Ekstrak isinya:
```
tar xzvf latest.tar.gz
```
Kemudian salin file contoh konfigurasi agar menjadi file konfigurasi yang nanti akan benar-benar digunakan:
```
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
```
Sekalian buat folder `update` agar ketika WordPress akan melakukan pembaruan tidak terkendala masalah *permission*:
```
mkdir /tmp/wordpress/wp-content/upgrade
```
Lalu salin keseluruhan direktori worpress ke dalam `/var/www/html`:
```
sudo cp -a /tmp/wordpress/. /var/www/html
```

###### 4. Konfigurasi direktori WordPress

Berikan *ownership* kepada user sudo dalam grup `www-data` yang dimiliki terhadap keseluruhan isi direktori WordPress:
```
sudo chown -R [usersudo]:www-data /var/www/html
```
Atur supaya semua file baru tetap mengikuti *ownership* direktori induknya:
```
sudo find /var/www/html -type d -exec chmod g+s {} \;
```
Berikan grup `www-data` akses *write* ke direktori `wp-content` agar bisa mengubah tema dan plugin WordPress:
```
sudo chmod g+w /var/www/html/wp-content
sudo chmod -R g+w /var/www/html/wp-content/themes
sudo chmod -R g+w /var/www/html/wp-content/plugins
```

###### 5. Setel file konfigurasi WordPress

Untuk mengamankan proses instalasi WordPress nanti, jalankan:
```
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```
Kemudian hasil yang keluar disalin ke dalam file `/var/www/html/wp-config.php` pada bagian:
```
. . .

define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

. . .
```
Di file yang sama, ubah beberapa detil lagi:
```
. . .

define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'username');

/** MySQL database password */
define('DB_PASSWORD', 'password');

. . .

/** Tambahkan baris ini */
define('FS_METHOD', 'direct');
```

###### 6. Install WordPress lewat web-interface

Lewat browser, akses `http://[ip_server_wordpress]]`, kemudian ikuti proses sampai selesai. Pastikan user dan password user WordPress diingat betul. Halaman beranda setelah berhasil melakukan instalasi adalah semacam ini:

![alt text](./wp_home.JPG)

## 3.2. Plugin WordPress

Setelah login sebagai user WordPress lewat browser, pada bagian sidebar akan ada menu `Plugins`. Pilih menu tersebut dan sub-menu `Add new`, lalu cari plugin yang ingin diinstall. Setelah ketemu, cukup klik `Install Now` dan `Activate` setelah terinstall.

![alt text](./wp_plugininstall.JPG)

## 3.3. *SQL Injection Testing Tools*

## 3.3.1. WPScan

Sebelum menggunakan WPScan, kita pastikan terlebih dahulu sistem *host attacker* sudah terinstall dependensi-dependensi yang dibutuhkan WPScan. Untuk itu lakukan:
```
sudo apt-get update
sudo apt-get install curl git libcurl4-openssl-dev make zlib1g-dev gawk g++ gcc libreadline6-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgdbm-dev libncurses5-dev automake libtool bison pkg-config ruby ruby-bundler ruby-dev
```
Setelah itu kita akan meng-*clone* aplikasi WPScan. Misalkan kita letakkan aplikasi ini di direktori home user:
```
cd ~
git clone https://github.com/wpscanteam/wpscan.git
```
Masuk ke folder aplikasi WPScan dan install dependensi ruby:
```
cd wpscan
bundle install --without test
```
Disarankan memperbarui WP Scan Vulnerability Database dengan perintah:
```
ruby wpscan.rb --update
```
Kemudian untuk menjalankan pemindaian website WordPress bisa dilakukan dengan:
```
ruby wpscan.rb --url https://[nama_domain_web_atau_alamat_ip_web]
```

## 3.3.2. sqlmap

Untuk menggunakan sqlmap, cukup dipastikan bahwa *host attacker* sudah terinstall git, lalu jalankan:
```
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git
```

## 4. Uji Penetrasi

### 4.1. WPScan

Pertama yang harus dilakukan adalah melakukan instalasi plugin League Manager dan Spider Video Player. Kami juga melakukan instalasi beberapa plugin lain yang menurut sumber yang kami rujuk merupakan plugin yang rentan: User Access Manager dan Advanced Access Manager. Kemudian dari terminal VM attacker kami menjalankan perintah:
```
$ ruby wpscan.rb --url [alamat_target]
```

Dalam kasus ini `[alamat_target]` adalah alamat IP VM target atau `http://10.151.36.199`. Hasil yang muncul ternyata terdapat vulnerability pada Wordpress itu sendiri dan pada plugin yang telah diinstall. Untuk celah pada Wordpress:

* Beberapa direktori rentan karena directory-listing masih aktif.
* Rentan header injection pada saat melakukan reset password.
* Rentan unggahan file javascript, namun dari hasil wpscan sudah teratasi jika versi Wordpress diperbarui.

![alt text](./wpscan1.JPG)

* RSS and Atom Feed Escaping (namun dari hasil wpscan sudah teratasi jika versi Wordpress diperbarui).
* HTML Language Attribute Escaping (namun dari hasil wpscan sudah teratasi jika versi Wordpress diperbarui).
* 'newbloguser' Key Weak Hashing (namun dari hasil wpscan sudah teratasi jika versi Wordpress diperbarui).

![alt text](./wpscan2.JPG)

Sedangkan celah pada plugin yang ditemukan oleh wpscan adalah:

* League Manager: directory-listing masih aktif
* League Manager: Multiple Parameter XSS
* League Manager: SQL Injection
* League Manager: Unauthenticated SQL Injection

![alt text](./wpscan3.JPG)

* Spider Video Player: directory-listing masih aktif
* Spider Video Player: SQL Injection
* Spider Video Player: Reflected Cross-Site Scripting

![alt text](./wpscan4.JPG)

* User Access Manager: directory-listing masih aktif

![alt text](./wpscan5.JPG)

* Advanced Access Manager: directory-listing masih aktif

![alt text](./wpscan6.JPG)

### 4.2. sqlmap

## 5. Kesimpulan dan Saran

Jika ingin menjadikan Wordpress sebagai platform untuk situs web yang digunakan, sebaiknya dilakukan berbagai pengaturan tambahan untuk menutup celah-celah yang terlalu mudah terekspos seperti directory-listing dan SQL Injection. Terlebih lagi dengan berbagai macam plugin yang akan diinstall yang tentunya menambah jumlah lubang yang harus ditutup. Begitu juga pihak yang mengurus web harus rajin melakukan pembaruan versi Wordpress dan plugin-pluginnya.

## 6. Sumber

http://sqlmap.org/

https://guides.wp-bullet.com/install-wpscan-ubuntu-16-04-wordpress-vulnerability-scanning/

https://firstsiteguide.com/tools/hacked-dangerous-vulnerable-wordpress-plugins/

https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04

https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lamp-on-ubuntu-16-04

https://wordpress.org/

https://distrowatch.com/

https://www.owasp.org/index.php/SQL_Injection
