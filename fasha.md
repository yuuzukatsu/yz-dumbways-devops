# reverse proxy and use cerbot

menggunakan dns cloudflare
1.masuk ke cloudflare lalu ke bagian dns, masukkan nama domain dan ip dituju

frontend

![kel1tokennew2](https://user-images.githubusercontent.com/111863692/189657922-6069f12f-f3d2-4dee-9ea0-837ef85c8efa.png)

backend

![kel1tokennew](https://user-images.githubusercontent.com/111863692/189657890-6c3b843d-5b36-4464-9f3f-bac5d60eb91a.png)

reverse proxy for docker

1.update server terlebih dahulu `sudo apt update` lalu install nginx `sudo apt install nginx`

![kel1ngx](https://user-images.githubusercontent.com/111863692/189653984-466f5708-d3d5-45bd-81b1-e4509fdc04c4.png)

2.buat direktori pada /etc/nginx/ 

![kel1nginx1 5](https://user-images.githubusercontent.com/111863692/189654453-8a96c9a8-6d80-49f9-93f9-45792ad8c42d.png)

3.buat file di direktori tersebut sudo nano `frontend.conf` dan masukkan konfigurasi reverse proxy sesuai dicloudflare tadi untuk frontend kita gunakan port 3000

![kel1ngx7](https://user-images.githubusercontent.com/111863692/189655006-26b67d7e-7be9-4b4f-998a-9f24e79cc9da.png)

lakukan yang sama untuk backend untuk backend kita gunakan port 5000

![kel1nginx 5000](https://user-images.githubusercontent.com/111863692/189656437-8aa147f9-c7a7-4f70-b767-0bc754f2d421.png)

4.jika sudah masuk file nginx.conf di /etc/nginx/nginx.conf

![kel1ngx5](https://user-images.githubusercontent.com/111863692/189656708-eda2928d-0972-4f21-8e05-8bc30bf33107.png)

lakukan include /etc/nginx/nama_dir_yang_dibuat_tadi/*;

5.lakukan pengecekkan apakah syntax nginx tidak terjadi masalah `sudo nano nginx -t`, jika benar restart nginx `sudo systemctl restart nginx`

![kel1ngx6](https://user-images.githubusercontent.com/111863692/189657169-0446c049-7112-48a7-bd1e-f699a5b7d4a8.png)

menggunakan certbot untuk ip yang digunakan

1.install snapd dan refresh ke versi terbaru jika sudah terinstall `sudo snap install snap-core; sudo snap refresh core`

![kel1certbot1](https://user-images.githubusercontent.com/111863692/189658288-d7f9ef7b-2b68-4635-82ba-66d2aefc8fd4.png)

2.install certbot `sudo snap install --classic certbot

![kel1certbot2](https://user-images.githubusercontent.com/111863692/189658405-d1b0e73e-9020-4ce4-a9d0-9a215cfd075f.png)

3.memastikan perintah certbot berjalan

![kel1certbot3](https://user-images.githubusercontent.com/111863692/189658521-ae69987a-49d1-4b99-b6f0-be835b5cb16e.png)

4.lalu jalankan `sudo cerbot --nginx, pilih domain yang ingin menggunakan ssl certificate

![certbotnginxkel1](https://user-images.githubusercontent.com/111863692/189659125-9b87b4ca-f0f6-4661-9ee7-d08e1f2c712b.png)

nanti akan di tanya email dan lain lain sesuai preferensi kalian

5.jika sudah kalian bisa cek di file reverse proxy kalian yang sudah dikonfirmasi menggunakan ssl 

![certbotfrontend](https://user-images.githubusercontent.com/111863692/189659528-13c2d906-911c-46cd-aa17-9612e8aeb392.png)

![bckendcertbot](https://user-images.githubusercontent.com/111863692/189660165-cfc9a838-a608-420f-a396-52816ba38760.png)
