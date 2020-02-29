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
  $ mysql_secure_installatio
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

5. Buat Database Untuk **OpenCart**
  ```
  $ sudo mysql -u root -p
      CREATE DATABASE opencartdb;
      GRANT ALL ON opencartdb.* TO 'student'@'localhost' IDENTIFIED BY 'student';
      FLUSH PRIVILEGES;
      EXIT;
  ```
6. Unduh **OpenCart**
  ```
  $ wget https://github.com/opencart/opencart/releases/download/3.0.2.0/3.0.2.0-OpenCart.zip
  ```
7. Ekstrak file download tadi ke folder /var/www/html/opencart
  ```
  $ sudo unzip 3.0.2.0-OpenCart.zip -d /var/www/html/opencart
  
  ```
8. Ubah nama config-dist.php ke config.php dan admin/config-dist.php to admin/config.php
  ```
  $ cd /var/www/html/opencart/upload 
  $ mv config-dist.php config.php
  $ cd /var/www/html/opencart/upload/admin
  $ mv config-dist.php config.php
  ```
  
9. Ubah otorisasi kepemilikan ke user www-data (webserver)
  ```
  $ sudo chown -R www-data:www-data /var/www/html/opencart
  $ sudo chmod -R 755 /var/www/html/opencart/
  ```
10. Konfigurasi Apache2 untuk membuat file virtual host untuk **OpenCart**
  ```
  vim /etc/apache2/sites-available/opencart.conf
  
  <VirtualHost *:80>
  ServerAdmin admin@example.com
  ServerName example.com
  DocumentRoot /var/www/html/opencart/upload/

  <Directory /var/www/html/opencart/upload/>
  AllowOverride All
  allow from all
  </Directory>
  ErrorLog /var/log/apache2/your-domain.com-error_log
  CustomLog /var/log/apache2/your-domain.com-access_log common
  </VirtualHost>
  ```
  Save dan keluar dengan ```:wq```

11. Edit file php.ini di etc/php/7.2/apache2/php.ini, sesuaikan dengan dibawah ini:
  ```
  $ cd etc/php/7.2/apache2/
  $ nano php.ini

  memory_limit = 128M
  upload_max_filesize = 16M
  max_execution_time = 60
  file_uploads = On
  allow_url_fopen = On
  magic_quotes_gpc = Off
  register_globals = Off
  ```
12. Mengaktifkan virtual host
  ```
  $ sudo systemctl reload apache2
  $ a2ensite opencart.conf
  $ a2dissite 000-default.conf
  $ sudo service apache2 start
  ```
13. Mengaktifkan *re-write* modul
  ```
  $ a2enmod rewrite
  $ sudo systemctl reload apache2
  ```
14. Kunjungi alamat IP web server untuk melakukan instalasi **OpenCart**
  * 

## Referensi
1. [About OpenCart](https://www.opencart.com/index.php?route=cms/company)
2. [How to Install OpenCart On Ubuntu 18.04 Server](https://cloudcone.com/docs/article/install-opencart-on-ubuntu-18-04-server/)
