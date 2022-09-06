# Day 5

# CI/CD with Cloudflare Pages

## Definisi Cloudflare Pages

Menurut saya, Cloudflare Pages merupakan salah satu feature dari
Cloudfare yang digunakan untuk CD(Continous Delivery)

## Deploy Aplikasi ke Cloudflare Pages dengan Github

### Step 1

Fork repository `https://github.com/dumbwaysdev/wayshub-frontend`

![](./images/media/image1.png)

![](./images/media/image2.png)

### Step 2

Clone repo tersebut ke server

`git clone <clone-url>`

![](./images/media/image3.png)

![](./images/media/image4.png)

### Step 3

Cek directory yang sudah dibuat (sesuai nama repo yang dibuat)

![](./images/media/image5.png) 

### Step 4

Buka Cloudflare Pages dan pilih `Create Project`

![](./images/media/image6.png) 

![](./images/media/image7.png) 

### Step 5

Pilih `Connect to Git`

![](./images/media/image8.png) 

### Step 6

Pilih `Connect GitHub`

![](./images/media/image9.png) 

### Step 7

Pilih `Only select repositories` dan pilih repo yang di fork sebelumnya

![](./images/media/image10.png) 

### Step 8

Klik repo yang diinginkan di `Select a repository` dan lalu klik `Begin Setup`

![](./images/media/image11.png) 

### Step 9

Isikan `Project name` dan pilih `Branch` yang akan di deploy

![](./images/media/image12.png) 

### Step 10

Jika aplikasi memiliki command build spesik, isikan berikut. Jika sudah
klik `Save and Deploy`

![](./images/media/image13.png) 

### Step 11

Setelah build selesai, akan muncul link yang bisa dibuka

![](./images/media/image14.png) 

![](./images/media/image15.png) 

### Step 12

Selanjutnya kita akan coba merubah file `index.html`. Lalu cari tag html
`<title>` dan rubah isinya

`nano public/index.html`

![](./images/media/image16.png) 

### Step 13

Lakukan `add`, `commit`, lalu lakukan `push`

```git add .```

```git commit -m "edit index.html"```

```git push origin main```

![](./images/media/image17.png) 

### Step 14

Jika cek di Cloudflare Pages, akan muncul ada commit baru dan sedang
di-build

![](./images/media/image18.png) 

### Step 15

Buka lagi web sebelumnya, title sudah berubah

![](./images/media/image19.png) 
