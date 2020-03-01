<p align="center"><img src="https://user-images.githubusercontent.com/39584996/75613465-9a2f4a80-5b60-11ea-994c-28e81d587fef.jpg" width="800"></p>

# About OpenCart
**OpenCart** merupakan aplikasi webstore (toko online) yang berbasis PHP dan MySQL yang dapat dikelola dengan sistem CMS *(Content Management System)*, dimana untuk penggunaannya bersifat terbuka *(OpenSource)* dan gratis untuk siapa saja. Aplikasi webstore ini digunakan dengan lisensi GNU *(General Public License).*

[`^ Kembali ke atas ^`](#)
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
[`^ Kembali ke atas ^`](#)
1. Nyalakan server, lalu login kedalam server dengan menggunakan SSH
    ```
    $ ssh student@localhost -p 2200
    ```

2. Lalu set repository server
    ```
    $ sudo tee /etc/apt/sources.list << !
    deb http://repo.x.ac.id/ubuntu bionic main restricted universe multiverse
    deb http://repo.x.ac.id/ubuntu bionic-updates main restricted universe multiverse
    deb http://repo.x.ac.id/ubuntu bionic-security main restricted universe multiverse
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
    * Setujui persyaratan yang berlaku
    ![persyaratan](https://user-images.githubusercontent.com/39584996/75612040-94cb0380-5b52-11ea-985e-836705a16830.png)
    * Pre-Instalation untuk mengecek requirement penginstalan, kalau sudah pilih continue
    ![pre-install](https://user-images.githubusercontent.com/39584996/75612067-d78cdb80-5b52-11ea-9468-bdf75f1bb13a.png)
    * Konfigurasi Database
    ![database](https://user-images.githubusercontent.com/39584996/75612153-8c26fd00-5b53-11ea-9e72-f5a12b43eba2.png)
    * Instalasi telah dilakukan
    ![Done](https://user-images.githubusercontent.com/39584996/75612210-1ec79c00-5b54-11ea-80bd-44322510a9ab.png)
15. Setelah melakukan instalasi, untuk alasan keamanan, hapus direktori install.
    ```
    $ sudo rm -rf /var/www/html/opencart/upload/install
    ```

# Konfigurasi
[`^ Kembali ke atas ^`](#)
### Menambah Plugin
1. Sebelum melakukan instalasi plugin, terlebih dahulu kita harus membuat akun opencart di [Register OpenCart](https://www.opencart.com/index.php?route=account/register), untuk mendapatkan `API Marketplace`
2. Isi data dengan sesuai dan pilih register
   ![0](https://user-images.githubusercontent.com/39584996/75620757-5586cc00-5bbf-11ea-8a46-6b38f8daf5c2.png)
3. Dimenu akun, masuk ke **Purchase** dan pilih **Your Store**
   ![0 1](https://user-images.githubusercontent.com/39584996/75620731-017be780-5bbf-11ea-9e3c-6a2deb731389.png)
4. Pilih **Add store**
   ![0 2](https://user-images.githubusercontent.com/39584996/75620732-02ad1480-5bbf-11ea-9dca-158d89659d59.png)
5. Masukkan domain website kita
   ![0 3](https://user-images.githubusercontent.com/39584996/75620733-03de4180-5bbf-11ea-84e9-a845d4b3c471.png)
6. Pilih menu lihat, disitu akan ditampilkan API Marketplace dan kemudian disalin
   ![0 4](https://user-images.githubusercontent.com/39584996/75620734-050f6e80-5bbf-11ea-815b-ad98c0b5430f.png)
7. Lalu kembali ke admin website kita, dibagian Sidebar, masuk ke menu **Extentions** dan pilih **Marketplace**
8. Tinggal pilih plugin atau extention yang ingin dipasang di website kita, misalnya disini kita ingin menambahkan fitur `Customer Service`, kita bisa melakukan filtering, memasukkan kata kunci untuk mencari plugin yang kita butuhkan.
   ![2](https://user-images.githubusercontent.com/39584996/75620784-c0d09e00-5bbf-11ea-8bd6-cae1965d0dea.png)
9. Lalu pilih Marketplace API yang berada dipojok kanan atas, logo gear, masukkan secret API dan username
   ![3](https://user-images.githubusercontent.com/39584996/75620786-c332f800-5bbf-11ea-9d42-9024d0ff4c59.png)
10. Masuk ke menu download dan pilih install
   ![4](https://user-images.githubusercontent.com/39584996/75620903-6a179400-5bc0-11ea-9208-92f744bf92c6.png)
11. Tunggu hingga proses selesai

### Ubah Profil
1. Melakukan pengubahan nama user tampilan, dengan menekan ikon user dipojok kanan atas
   ![0](https://user-images.githubusercontent.com/39584996/75612666-55071a80-5b58-11ea-8148-9e6a8a2b8d38.png)
2. Mengganti Gambar dan Nama
   ![0 1](https://user-images.githubusercontent.com/39584996/75612665-53d5ed80-5b58-11ea-8d2b-0aa3961be93c.png)

# Maintenance
Maintenance yang bisa dilakukan oleh **OpenCart** adalah melakukan backup/restore data dan setting dari website tersebut, hal ini merupakan kegiatan yang sangat penting apabila terjadi sesuati pada website kita. Cara yang dilakukan untuk melakukan backup adalah :
1. Dibagian sidebar navigation, pilih menu **System**, lalu **Maintenance** dan pilih **Backup / Restore**, untuk backup, kita bisa memilih ingin membackup data apa saja
   ![1](https://user-images.githubusercontent.com/39584996/75620040-4e0ef500-5bb6-11ea-940a-f8d34fe69f62.png)
2. Lalu tekan `Export`
3. Dan backup data tersebut akan terdownload dalam format `.sql`
   ![2](https://user-images.githubusercontent.com/39584996/75620056-7c8cd000-5bb6-11ea-880e-0122b3f7b7b1.png)
4. Untuk melakukan restore, tinggal masuk ke menu **Restore** dan tinggal import backup yang telah kita lakukan sebelumnya
   ![3](https://user-images.githubusercontent.com/39584996/75620083-b6f66d00-5bb6-11ea-8f7f-6b8e0a3a3e1d.png)
5. Tunggu hingga proses import selesai

# Otomatisasi
[`^ Kembali ke atas ^`](#)
Terdapat beberapa cara dalam melakukan otomatisasi pemasangan **OpenCart** atau beberapa CMS lain, yaitu dengan penginstalan dengan menjalankan `script shell` yang biasanya berekstensi `.sh`, akan tetapi sedikit rumit untuk dilakukan karena harus memahami cara membuat dari script tersebut. 
Lalu alternatif lain yang lebih mudah adalah menggunakan bantuan dari fitur yang biasanya terdapat pada `panel hosting` dari `web-hosting provider` itu sendiri, mereka akan menginstal script secara otomatis pada website tujuan, yang biasa dipakai oleh banyak kalangan adalah `Installatron`, karena mudah tanpa perlu melakukan login, untuk melakukan instalasinya, yang perlu dilakukan adalah :
1. Kunjungi link ini [Installatron](https://installatron.com/opencart), lalu pilih menu `Install this application` yang berada dipojok kanan atas
    ![1](https://user-images.githubusercontent.com/39584996/75619924-e0ae9480-5bb4-11ea-91eb-bce3c5124aa5.png)
2. Lakukan pengisian data dengan sesuai, lalu tinggal lakukan penginstalan
    ![2](https://user-images.githubusercontent.com/39584996/75619927-e310ee80-5bb4-11ea-8e5a-05902eb6f566.png)
    ![3](https://user-images.githubusercontent.com/39584996/75619928-e3a98500-5bb4-11ea-9ffc-ac3966ef8dce.png)
    ![4](https://user-images.githubusercontent.com/39584996/75619929-e4421b80-5bb4-11ea-832f-9885feb11ea5.png)
3. Tunggu hingga proses instalasi selesai dilakukan

# Cara Penggunaan
[`^ Kembali ke atas ^`](#)
Cara penggunaan CMS dari **OpenCart** ini sangatlah mudah dengan menyediakan interface yang *User Friendly*. Berikut penggunaan lebih jelasnya :
1. Lakukan login ke administrator <br/>
   ![login](https://user-images.githubusercontent.com/39584996/75612373-52ef8c80-5b55-11ea-8d00-1a6f36e824fb.png)
2. Memindahkan storage directory ke luar directory web
   ![move_dir](https://user-images.githubusercontent.com/39584996/75612410-a06bf980-5b55-11ea-8e95-0388a0f51b9c.png)
3. Setelah melakukan login dan pemindahan directory storage, kita akan dihadapkan dengan tampilan dashboard website yang berisi statistik dari website tersebut, seperti hasil penjualan, jumlah customer dan lain-lain
   ![dashboard](https://user-images.githubusercontent.com/39584996/75612686-87b11300-5b58-11ea-8c57-30da0000f18e.png)
4. Di *Navigasi* kita akan menemukan banyak sekali pilihan menu yang bisa digunakan, terdapat menu **Catalog** yang berisi seperti Categories, yang berguna untuk sorting produk berdasarkan jenis barang
   ![Categories](https://user-images.githubusercontent.com/39584996/75612743-0443f180-5b59-11ea-8a38-5657a16ed8d5.png)
   ![8 1](https://user-images.githubusercontent.com/39584996/75612753-132aa400-5b59-11ea-96c8-a12b46ae394e.png)
5. Lalu ada Poducts yang merupakan halaman yang berisi produk yang akan kita jual, disini juga merupakan tempat untuk memanajemen produk yang dijual, seperti harga, diskon dan lain-lain
   ![Products](https://user-images.githubusercontent.com/39584996/75612776-538a2200-5b59-11ea-976a-c25d7fa89f42.png)
   ![9](https://user-images.githubusercontent.com/39584996/75612778-54bb4f00-5b59-11ea-9c2c-55fa92b50c12.png)
6. Recurring profile atau profil pembayaran rutin pada opencart merupakan fitur yang memungkinkan bagi kamu sebagai pengelola toko online untuk melakukan pengaturan psistem pembayaran rutin bagi pelanggan
   ![recurring](https://user-images.githubusercontent.com/39584996/75612790-73b9e100-5b59-11ea-954b-a36d2c7d9113.png)
7. Filter Group, untuk melakukan multi filtering
   ![filter](https://user-images.githubusercontent.com/39584996/75613153-ea0c1280-5b5c-11ea-97c4-bcb586421588.png)
8. Attribute
   ![12](https://user-images.githubusercontent.com/39584996/75612798-98ae5400-5b59-11ea-8b27-9b911e4cc09c.png)
9. Option, mengatur elemen kecil web
   ![13](https://user-images.githubusercontent.com/39584996/75612871-3d309600-5b5a-11ea-9ca0-cad0d679ba5d.png)
10. Manufactures, manajemen manufaktur barang
   ![14](https://user-images.githubusercontent.com/39584996/75612872-3e61c300-5b5a-11ea-827b-00955f62c54f.png)
11. Downloads, Melihat user yang melakukan download
   ![15](https://user-images.githubusercontent.com/39584996/75612873-3efa5980-5b5a-11ea-974a-cb428997df30.png)
12. Reviews, Melihat Review produk yang ada
   ![16](https://user-images.githubusercontent.com/39584996/75612874-3f92f000-5b5a-11ea-9e69-3577e9683d38.png)
13. Informations, page informasi yang ditampilkan ke user
   ![17](https://user-images.githubusercontent.com/39584996/75612875-402b8680-5b5a-11ea-808b-55e59987bdc9.png)
14. Dan masih banyak menu-menu lain yang bermanfaat banyak untuk pengelolaan web ini, seperti **Extentions** yang berguna untuk menambah add-ons untuk menunjang kinerja web tersebut,
   ![18](https://user-images.githubusercontent.com/39584996/75613009-7584a400-5b5b-11ea-867c-0aada8d24c3e.png)
   ![20](https://user-images.githubusercontent.com/39584996/75613026-a5cc4280-5b5b-11ea-8ce1-de0457c512de.png)
15. Lalu ada **Design** untuk mengatur tampilan dari web tersebut, baik menggunakan thema ataupun layout editor
   ![20](https://user-images.githubusercontent.com/39584996/75613026-a5cc4280-5b5b-11ea-8ce1-de0457c512de.png)
16. Lalu ada **Sales** yang mengatur manajemen orderan barang, seperti pengembalian, order yang terjadi, dan lain-lain
   ![22](https://user-images.githubusercontent.com/39584996/75613060-0b203380-5b5c-11ea-9cad-be39bfa99d8e.png)
17. Lalu ada **Customers** yang berisi manajemen user yang terdaftar
   ![23](https://user-images.githubusercontent.com/39584996/75613078-360a8780-5b5c-11ea-9bdc-e3e20423d08a.png)
18. **Marketing**, mengatur marketing yang ada diweb
   ![25](https://user-images.githubusercontent.com/39584996/75613087-5a666400-5b5c-11ea-9bc0-f4d2f7360f03.png)
19. **Reports**, berisikan laporan mengenai website
   ![26](https://user-images.githubusercontent.com/39584996/75613105-6fdb8e00-5b5c-11ea-9a88-b9c39ae59353.png)
20. **System**, menu untuk mempermudah konfigurasi website, baik itu konfigurasi umum maupun konfigurasi lanjutan
   ![27](https://user-images.githubusercontent.com/39584996/75613122-939ed400-5b5c-11ea-8914-7d1365769bee.png)

# Pembahasan
[`^ Kembali ke atas ^`](#)
**OpenCart** memiliki fitur yang sangat lengkap, yang suport dengan berbagai macam jenis database, seperti `MySQL` atau `MariaDB`, sehingga memiliki banyak **kelebihan** seperti diantaranya:
* *Simple* dalam setup cms tersebut, seperti konfigurasi awal dan pengaturan tampilan
* *Performa dan Usability* yang baik, openchart sendiri menggunakan `AJAX technology` yang membuat load time lebih sedikit
* *Multi-Store Functionality* yang membuat openchart bisa mengelola multi store
* *Harga* yang open-source
* Banyak *Features and Extensions* gratis

Akan tetapi dibalik kelebihan tersebut, terdapat kelemahan dari **OpenCart** seperti :
* Bagi pengguna awam sering mengalami kesulitan saat instalasi ketika harus rename config-dist menjadi config dan harus delete folder install, tidak secara otomatis terhapus.
* Halaman login admin masih harus mengetikan di address bar yaitu localhost/opencart/upload/admin, belum ada link khusus di website.
* Pilihan theme dan modul yang sederhana, kecuali untuk yang berbayar
* Fungsi yang masih sulit untuk difahami

Akan tetapi, dibandingkan dengan e-commerce lain seperti **Magento**, CMS ini memiliki kelebihan dan kekurangan, seperti :
* Magento lebih `populer dan support` lebih dibandingkan opencart
* Magento memiliki `extention` yang lebih banyak dibandingkan opencart
* OpenCart lebih `mudah` digunakan dibandingkan Magento
* Sama-sama open-source
* Magento memiliki tingkat secure yang lebih baik
* Sama-sama bisa multi-store
* Sama-sama mudah dikustomisasi

# Referensi
[`^ Kembali ke atas ^`](#)
1. [About OpenCart](https://www.opencart.com/index.php?route=cms/company)
2. [How to Install OpenCart On Ubuntu 18.04 Server](https://cloudcone.com/docs/article/install-opencart-on-ubuntu-18-04-server/)
3. [5 Benefits That Make OpenCart Rock](https://www.shopping-cart-migration.com/carts-reviews/opencart/12797-5-benefits-that-make-opencart-rock)
4. [Kelebihan & Kekurangan OpenCart](http://cyber-blog95.blogspot.com/2016/04/kelebihan-kekurangan-opencart.html)
5. [Magento vs OpenCart](https://magenticians.com/magento-vs-opencart-comparison/)
6. [OpenCart with Installatron](https://installatron.com/opencart)
