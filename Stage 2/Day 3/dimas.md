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
