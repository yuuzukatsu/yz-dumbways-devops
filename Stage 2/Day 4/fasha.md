# revproxy untuk jenkins
1. buat folder baru dan konfigurasi baru untuk jenkins, jalankan dengan port 8080 port default jenkins

![jenkinsrevprox1](https://user-images.githubusercontent.com/111863692/190257763-7c639046-73a8-4659-9f1b-e7903eecbebd.png)

2.lalu pastikan sudah terkonfigurasi pada nginxnya di nginx.conf, include sesuai letak file jenkins reverse proxynya

![jenkinsrevprox](https://user-images.githubusercontent.com/111863692/190259165-040aa5d5-f047-4631-ba61-2b09e08e8ed9.png)

3.jika sudah pastikan syntax tidak error jika tidak error lakuka restart nginx

![jenkinsrevprox](https://user-images.githubusercontent.com/111863692/190259378-389f65cc-e033-4707-b21d-6f22f211925c.png)

4.buat domain pada cloudflare sesuai dengan domain yang sudah di konfigurasi pada nginx tadi, dan sesuaikan ipnya dengan nginx

![jenkinsrevprox0](https://user-images.githubusercontent.com/111863692/190259662-0b3ec5fb-8dc2-4af3-94b5-8365be748383.png)

5.set certbot untuk domain jenkins,pastikan kalian sudah install snapd, `sudo ln -s /snap/bin/certbot /usr/bin/certbot` untuk memastikan perintah certbot jalan

![revproxjenkinscerbto](https://user-images.githubusercontent.com/111863692/190260348-b534da6a-b796-40fc-a05e-bb534c2811ab.png)

6.jalankan `certbot --nginx`

![jenkincerbot2](https://user-images.githubusercontent.com/111863692/190260722-5cb998e7-831c-4423-bf56-f80bac9ae799.png)

pilih domain yang ingin dicerbot

7.jika sudah cek konfigurasi revprox kalian untuk memastikan perubahan

![jenkinsrevprox6](https://user-images.githubusercontent.com/111863692/190260843-5c9df15c-cc11-4a14-83da-ce27772700d4.png)

# install jenkins
1.update server terlebih dahulu

![installjenkins1](https://user-images.githubusercontent.com/111863692/190261230-a7932023-c696-4728-9ebb-5f6dbfb312db.png)

agar memudahkan install docker, buat file bash dengan berisi perintah install docker

#!/bin/bash

sudo apt-get update

sudo apt-get install \ca-certificates\curl\gnupg\lsb-release\apt-transport-https\software-properties-common

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

docker -v

jalankan file bashnya `sh nama_file.sh`

![bash docker](https://user-images.githubusercontent.com/111863692/190395938-517394b9-c55a-418c-acc7-b4b478e643a8.png)

2.pastikan diserver kamu sudah terinstall docker lalu jalankan `docker run --name jenkins1 -p 8080:8080 -p 50000:50000 -v jenkins_home:var/jenkins_home jenkins/jenkins:lts`

![installjenkins3](https://user-images.githubusercontent.com/111863692/190261685-1db40de8-5253-41f9-98e0-d78dad068e1b.png)

![installjenkins4](https://user-images.githubusercontent.com/111863692/190261953-efb581fe-536e-471b-98c0-12807dd1e4bb.png)

3.lihat logs pada container jenkins untuk mengambil pass dibutuhkan untuk instalasi jenkins `docker logs jenkins1`

![installjenkins6](https://user-images.githubusercontent.com/111863692/190262585-c936ab92-67fe-4260-b120-594cd684890b.png)

4.jika container jenkins sudah berjalan, lalu jalankan domain di web browser yang sudah dikonfigurasi tadi untuk jenkins
masukkan password yang sudah kalian copy tadi dari logs jenkins1, lalu continue

![installjenkins7](https://user-images.githubusercontent.com/111863692/190262793-79e45b15-9e3e-4f70-a4ac-0d81356a5051.png)

5.disini pilh install suggested plugins saja agar plugins yang diinstal yang memang seperlunya saja

![installjenkins8](https://user-images.githubusercontent.com/111863692/190262922-862013af-6aab-45db-96b2-c1076c73b1a6.png)

6.tunggu penginstallan plugins selesai 

![jenkinsinstall7](https://user-images.githubusercontent.com/111863692/190263393-9d97e8e6-c3c9-46a8-bcf4-c7bab9a54fd1.png)

7.jika sudah di sini bisa masukkan sesuai keterangan kalian, jika sudah click save & continue

![jenkins info](https://user-images.githubusercontent.com/111863692/190263774-89522e3d-7531-4ab3-928f-79eeb0813ea4.png)

8.pastikan url sudah sesuai dengan domain jenkins reverse proxynya, jika sudah save & finish

![jenkinsinstall8](https://user-images.githubusercontent.com/111863692/190263920-adab1b6a-de2a-4dae-8568-81fde9a51bb6.png)

9.maka jenkins telah siap digunakan

![jenkinsinstall9](https://user-images.githubusercontent.com/111863692/190264435-06335903-e748-4c3b-abae-91541e30b4e5.png)

10.install ssh agent plugin, pilih manage jenkins pada menu bagian kiri

![managecredj1](https://user-images.githubusercontent.com/111863692/190264897-a727eb4f-7106-4472-94e3-c3ef9d484c8f.png)

lalu click manage plugins 

11.click available 

 search ssh agent

![installjenkins10](https://user-images.githubusercontent.com/111863692/190265126-3362c59e-37a8-4f49-b1ea-c9d8a5ef6e9b.png)

click install without restart

12. jika ingin cek install plugins bisa ke configure system

![managecredj1](https://user-images.githubusercontent.com/111863692/190265380-ca608ce7-4919-4f66-ade5-46888c97d32f.png)

![installjenins12](https://user-images.githubusercontent.com/111863692/190265407-c08d471c-ffdf-4b0a-9f73-514349aae2c7.png)

13.set up credential jenkins, pastikan kalian sudah generate sshkey `ssh-keygen` diserver aplikasi yang digunakan karena kita akan membutuhkan private ssh

![jenkinscrrede6](https://user-images.githubusercontent.com/111863692/190265835-bd466b49-c322-4f6f-a9df-f9954961ba2a.png)

![jeninscrede6](https://user-images.githubusercontent.com/111863692/190268064-42c7136d-bb1b-42b6-b4ac-1f010ba18a0e.png)

lalu copy semua isi dari (id_rsa) private ssh

14.click manage credentials

![managecredj1](https://user-images.githubusercontent.com/111863692/190266219-4dc7097f-25d9-444c-bcdd-e809b5536b29.png)

15.lalu click global untuk add credentials key

![jenkinscredent11](https://user-images.githubusercontent.com/111863692/190266505-0ee31d71-db71-45f4-acef-8592b5881ace.png)

16.lalu click add credentials, pada bagian kind pilih ssh username with private key

![jenkinscrede5](https://user-images.githubusercontent.com/111863692/190266795-e0ab76aa-6087-4d0d-bbcd-d8f24d36772f.png)

sisanya kalian bisa buat sesuai preferensi kalian

17.pada bagian bawah click enter directly pada private key, lalu paste private key yang sudah kalian copy tadi

![jenkinscrede8](https://user-images.githubusercontent.com/111863692/190267329-b53c39b9-9f04-4f98-bd24-c0f26eb72347.png)

lalu create


