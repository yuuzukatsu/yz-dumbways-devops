# Day 3

# Docker

## Buat Script Untuk Instalasi Docker Secara Otomatis

```
#!/bin/bash

sudo apt-get update

sudo apt-get install \ ca-certificates \ curl \ gnupg \ lsb-release \ apt-transport-https \ software-properties-common

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

docker -v
```

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/14.png)

## Frontend App in Docker

### Buat file bernama Dockerfile dan masukkan konfigurasi di bawah ini

```
FROM node:[version]
WORKDIR [directory]
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "npm", "start" ]
```

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/1.png)

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/2.png)

### Jalankan perintah build untuk melakukan build Dockerfile yang telah dibuat agar dapat menjadi Docker Images.

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/3.png)

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/4.png)

### Push images yang ada di local ke dalam server di Dockerhub/docker registry.

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/5.png)

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/11.png)

### Buat Docker Container

```
docker container create --name (name) -p (custom-port):(application-port) (images):(tag)
```

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/8.png)

### Jalankan Docker Container

docker container start (name)

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/9.png)

### Buat file bernama docker-compose.yaml

```
version: '3.8'
services:
 frontend:
   build: .
   container_name: dumbways-frontend
   image: malikalrk/dumbways-fe:1.0
   stdin_open: true
   ports:
    - 3000:3000
```

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/10.png)

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/15.png)

### Jalankan docker compose yang telah dibuat

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/12.png)

### Integrasikan Frontend dan Backend

Edit file API

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/6.png)

### Coba Akses Aplikasi pada Web

![](https://github.com/yuuzukatsu/Stage-2-Devops-Malik/raw/main/media/day3/13.png)


## Deploy Backend Dumbflix dan Database dalam 1 Docker Compose

### Step 1

Clone Repo Backend Dumbflix
```
git clone https://github.com/dumbwaysdev/dumbflix-backend.git
```
![](./images/media/image1.png) 

### Step 2

Ikuti instruksi didalam `README.md`

![](./images/media/image2.png) 

Edit file `~/dumbflix-backend/config/config.json` Sesuaikan dengan
database yang akan dibuat

![](./images/media/image3.png) 

### Step 3

Buat file `Dockerfile`, dan isikan berikut
```
FROM node:10
WORKDIR /app
COPY . .
RUN npm install
RUN npm install sequelize-cli
RUN wget https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh
RUN chmod +x wait-for-it.sh
RUN echo '#!/bin/bash' > start-server.sh
RUN echo 'npx sequelize-cli db:migrate' >> start-server.sh
RUN echo 'npm start' >> start-server.sh
RUN chmod +x start-server.sh
EXPOSE 5000
CMD ["npm", "start"]
```
![](./images/media/image4.png) 

### Step 4

Buat file `docker-compose.yml` dan isikan berikut
```
version: '3.8'
services:
  backend:
    build: .
    depends_on:
      - db
    ports:
      - '5000:5000'
    expose:
      - '5000'
    command: ./wait-for-it.sh db:3306 -s -t 0 -- ./start-server.sh
  db:
    image: mysql:8
    restart: always
    environment:
      MYSQL_DATABASE: 'dumbflix'
      MYSQL_USER: 'dumbflix'
      MYSQL_PASSWORD: 'dumbflix'
      MYSQL_ROOT_PASSWORD: 'P4ssword!'
    ports:
      - '3306:3306'
    expose:
      - '3306'
    volumes:
      - database:/var/lib/mysql

volumes:
```

### Step 5

Jalankan command docker compose untuk mulai build
```
docker compose up -d
```
![](./images/media/image5.png) 

### Step 6

Cek aplikasi sudah berjalan

![image](https://user-images.githubusercontent.com/67664879/189705438-9fd029f2-5bc0-43ca-905a-2232871a1277.png)
![image](https://user-images.githubusercontent.com/67664879/189705577-8425fba6-0e60-45a2-9d30-cf9c15f9b0cc.png)
![image](https://user-images.githubusercontent.com/67664879/189705944-e3ef6d93-126f-4d06-97cc-8069c5d5715d.png)


## Upload Docker Images ke hub.docker.com

### Step 1

Login ke docker dengan command berikut
```
docker login
```
![](./images/media/image7.png)

### Step 2

Jalankan command berikut untuk memberi tag ke images yang akan di upload
```
docker tag <nama images>:latest <username>/dumbflix-backend:v1
```
![](./images/media/image8.png) 

### Step 3

Lakukan push dengan command berikut
```
docker image push yuuzukatsu/dumbflix-backend:v1
```
![](./images/media/image9.png) 


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
