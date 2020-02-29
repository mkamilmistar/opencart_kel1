# About OpenCart
**OpenCart** merupakan aplikasi webstore (toko online) yang berbasis PHP dan MySQL yang dapat dikelola dengan sistem CMS *(Content Management System)*, dimana untuk penggunaannya bersifat terbuka *(OpenSource)* dan gratis untuk siapa saja. Aplikasi webstore ini digunakan dengan lisensi GNU *(General Public License).*

# Instalasi
### Kebutuhan Sistem
* Web Server (Apache)
* PHP 5.4+
* Database (MariaDB)

### PHP Libraries Yang Dibutuhkan
* Curl
* ZIP
* Zlib
* GD Library
* Mcrypt
* Mbstrings
* Xml

### Proses Instalasi
1. Nyalakan server, lalu login kedalam server dengan menggunakan SSH <br/>
  ```
  $ ssh student@localhost -p 2200
  ```

2. Lalu set repository server <br/>
  ```
  $ sudo tee /etc/apt/sources.list << ! <br/>
  deb http://repo.x.ac.id/ubuntu bionic main restricted universe multiverse
  deb http://repo.x.ac.id/ubuntu bionic-updates main restricted universe multiverse
  deb http://repo.x.ac.id/ubuntu bionic-security main restricted universe multiverse <br/>
  !   
  ```

3. Lakukan pengecekan update terhadap sistem, lalu lakukan penginstalan *requirement* untuk **OpenCart** seperti **PHP, Apache, MariaDB serta PHP modul yang dibutuhkan.** <br/>
  ```
  $ sudo apt-get update -y
  $ sudo apt-get install apache2
  $ sudo apt-get install php
  $ sudo apt-get install libapache2-mod-php
  $ sudo apt-get install php-cli php-common php-mbstring php-gd php-intl php-xml php-mysql php-zip php-curl php-xmlrpc
  $ sudo apt-get install unzip
  $ sudo apt install mariadb-server mariadb-client

  ```
4. Konfigurasi MariaDB
  ```
  $ mysql_secure_installation
  
  ```
  * **Enter Password** (enter saja)
  * **Set root password? [Y/n]:** Ketika Y dan tekan enter
  * **New password:** Masukkan password baru
  * **Re-enter new password:** Ketik ulang password baru
  * **Remove anonymous users? [Y/n]:** ketik Y, lalu tekan enter
  * **Disallow root login remotely? [Y/n]:** Ketik Y lalu tekan enter
  * **Remove test database and access to it? [Y/n]:** ketik Y, lalu tekan enter
  * **Reload privilege table now? [Y/n]:** Ketik Y dan tekan enter.
  * **Restart MariaDB:** ```$ sudo systemctl restart mariadb.service```

5. Unduh **OpenCart**
  ```
  
  ```
