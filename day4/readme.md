# Day 4

# Definisi Git

Menurut saya, git merupakan aplikasi yang digunakan untuk version
control, project/code colaboration, dan management.

# Contoh Perintah Git

## git clone

`Git clone` digunakan untuk mengcopy keseluruhan reposity ke repository
local baru yang akan dibuat

![](./images/media/image1.png) 

## git log

menampilkan log/history commit dari repo

![](./images/media/image2.png) 

## Git rm

`Git rm` digunakan untuk mengapus file dari repository lokal

![](./images/media/image3.png) 

## Git restore

`Git restore` digunakan untuk mengembalikan file yang sudah dihapus dengan
command `git rm`

![](./images/media/image4.png) 

# Menyiapkan environtment Git di Ubuntu

## Konfigurasi user, email, dan SSH Key

### Step 1

Pastikan git sudah terinstall dengan command:

`Git --version`

![](./images/media/image5.png)

### Step 2

Isikan username dan email github dengan command berikut

![](./images/media/image6.png)

### Step 3

Buat private key dan public key dengan command berikut:

`Ssh-keygen`

![](./images/media/image7.png) 

### Step 4

Buka file `~/.ssh/id_rsa.pub` yang digenerate dari command `ssh-keygen`
sebelumnya. Copy isinya lalu simpan untuk sementara

![](./images/media/image8.png) 

### Step 5

Buka `github.com`, klik profil pada bagian kanan atas, lalu masuk ke menu
settings

![](./images/media/image9.png)

### Step 6

Pilih bagian SSH and GPG keys, lalu klik New SSH Key

![](./images/media/image10.png) 

### Step 7

Masukkan public key yang dicopy sebelumya ke kotak Key. Jika sudah klik
Add SSH Key

![](./images/media/image11.png) 

### Step 8

Cek koneksi ke Github dengan command berikut

`ssh -T git@github.com`

![](./images/media/image12.png) 

## Membuat dan Mengisi Repository+Branch dengan Aplikasi

### Step 1

Masuk ke folder yang akan di upload ke github

![](./images/media/image13.png) 

### Step 2

Ketikkan command berikut. Command ini akan membuat folder `.git` di folder
aplikasi

`git init`

![](./images/media/image14.png)

### Step 3

Sebelum upload, kita harus memilih file yang ingin kita upload. Kita
akan membuat file `.gitignore`. File ini berisi file atau direktori yang
tidak akan di upload. Contoh, kita tidak akan mengupload direktori
`node_modules`

`Touch .gitignore`

`Echo "node_modules" > .gitignore`

![](./images/media/image15.png) 

### Step 4

Setelah itu, kita akan memilih file yang akan kita upload. Ini akan
membuat file memasuki fase staged dimana file siap untuk di
commit(ditulis ke repository)

`git add <file>`

![](./images/media/image16.png) 

Jalankan git status untuk mengecek apakah file sudah ditambahkan
(ditandai dengan warna hijau)

### Step 5 

Buka github, lalu buat repository baru dengan memilih New repository
(Jika sudah membuat repo, skip ke step 7)

![](./images/media/image17.png) 

### Step 6

Isikan nama repo lalu klik create repository dibawah

![](./images/media/image18.png) 

### Step 7

Pilih SSH, lalu copy baris berikut

![](./images/media/image19.png)

Jika sudah membuat repository, klik code lalu pilih berikut

![](./images/media/image19-2.png)

### Step 8

Buka terminal kembali, lalu isikan kode berikut

`git remote add <remote-name> <url>`

![](./images/media/image20.png) 

Cek dengan command `git remote -v` untuk melihat daftar remote yang sudah
terdaftar didalam repository lokal

### Step 9

Lakukan commit untuk menulis file kedalam repository lokal dengan
command berikut

`git commit -m "my first commit"`

![](./images/media/image21.png) 

### Step 10

Lakukan push untuk mengirim file yang berada di repository lokal ke
repository github.

`git push <remote-name> <branch-name>`

![](./images/media/image22.png)

Isikan `<remote-name> ` dengan nama remote yang dibuat sebelumnya.
`<branch-name> ` secara default jika tidak disebutkan saat `git init`, akan
menggunakan `master`

### Step 11

Jika kita cek di github, maka file yang kita buat sudah muncul

![](./images/media/image23.png)

### Step 12

Buat branch baru dengan command berikut

`Git branch <branch-name>`

![](./images/media/image24.png) 

Lihat daftar branch yang sudah ada dengan command `git branch -a`

### Step 13

Saat ini kita masih di branch `master`. Untuk berpindah ke branch lain,
gunakan command berikut

`git checkout <branch-name>`

![](./images/media/image25.png) 

### Step 14

Lalu kita akan push branch tersebut ke github. Gunakan command berikut

`Git push <remote-name> <branch-name>`

![](./images/media/image26.png) 

### Step 15

Kita cek di github. Akan muncul branch baru beserta isinya

![](./images/media/image27.png) 

![](./images/media/image28.png) 

