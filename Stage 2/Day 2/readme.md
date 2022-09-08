## Intalasi Database MySQL

Dalam instalasi Database, sudah dicreate instance server tersendiri
untuk database.

![](./images/media/image1.png) 

![](./images/media/image2.png) 

### Step 1

Lakukan instalasi mysql dengan command berikut
```
sudo apt-get update && sudo apt-get install -y mysql-server
```
### Step 2

Kita akan melakukan setup mysql dulu supaya database bisa diakses dengan
user root
```
sudo mysql
```
![](./images/media/image3.png) 

### Step 3

Jalankan urutan command berikut, sesuaikan untuk password yang ingin
digunakan untuk user root database
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by '<password>';
exit
```
![](./images/media/image4.png) 

### Step 4

Jalankan command berikut
```
mysql_secure_installation
```
![](./images/media/image5.png) 

### Step 5

Selanjutnya, kita akan membuat user khusus untuk database dumbflix.
Jalankan urutan command berikut
```
mysql -u root -p
CREATE USER '<username>'@'%' IDENTIFIED BY '<password>';
CREATE DATABASE <database-name>;
GRANT ALL ON <database-name>.* TO '<username>'@'%';
exit
```
![](./images/media/image6.png) 
![](./images/media/image7.png) 

### Step 6

Selanjut kita tes dulu untuk user dan database bisa diakses
```
mysql -u dumbflix -p
use dumbflix
show tables;
```
![](./images/media/image8.png) 

### Step 7

Buka file `/etc/mysql/mysql.conf.d/mysqld.cnf`
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
lalu rubah line `bind-address` dan `mysqlx-bind-address` menjadi seperti berikut
```
bind-address = 0.0.0.0
mysqlx-bind-address = 0.0.0.0
```
![](./images/media/image9.png){width="5.740384951881015in"
height="1.0939031058617672in"}

### Step 8

Lakukan restart service `mysql`
```
sudo systemctl restart mysql.service
```
![](./images/media/image10.png) 

## Setup aplikasi backend dan integrasi dengan database

### Step 1

Pertama kita akan clone dulu aplikasi backend dumbflix
```
git clone https://github.com/dumbwaysdev/dumbflix-backend.git
```
![](./images/media/image11.png) 

### Step 2

Kita akan mengikuti environment yang diminta dari file `readme.md`

![](./images/media/image12.png) 

Menggunakan nodejs 10

![](./images/media/image13.png)

Isi file `config.json` disesuaikan

![](./images/media/image14.png) 

Untuk import database, gunakan command berikut
```
mysql -h 10.71.15.250 -u dumbflix -p -D dumbflix < dumbflix.sql
```
![](./images/media/image15.png) 

![](./images/media/image16.png) 

### Step 3

Sesuaikan konfigurasi di Frontend agar bisa terkoneksi ke Backend

![](./images/media/image17.png) 

![](./images/media/image18.png) 

### Step 4

Jalankan aplikasi dengan `PM2`

![](./images/media/image19.png) 

### Step 5

Tes fitur Register dan Login website

![](./images/media/image20.png) 

![](./images/media/image21.png) 

![](./images/media/image22.png) 

## Setup SSL dengan CertBot

### Step 1

Lakukan instalasi berikut
```
sudo apt-get install snapd
sudo snap install core; sudo snap refresh core
sudo apt-get install certbot python3-certbot-nginx
```
### Step 2

Jalankan command ini untuk instalasi certificate secara otomatis.
```
sudo certbot --nginx
```
Akan muncul prompt setelah command dijalankan. Isikan sesuai kebutuhan

![](./images/media/image23.png) 

### Step 3

Setelah proses selesai, tes dengan membuka browser

![](./images/media/image24.png) 
