# Dokumentasi Instalasi Linkding

## 1. Persiapan

Linkding diinstal pada VM lokal menggunakan Docker.

### Spesifikasi VM

- Sistem Operasi: Ubuntu
- Username: asus
- IP Address: 172.17.77.48
- Port Linkding: 9090

## 2. Instalasi Docker

Docker digunakan untuk menjalankan aplikasi Linkding.

### Mengecek Status Docker

Perintah:

```bash
sudo systemctl status docker
```

Hasil:

```text
Active: active (running)
```

Hal ini menunjukkan bahwa Docker berhasil berjalan pada VM.

### Mengecek Versi Docker

```bash
docker --version
```

### Mengecek Docker Compose

```bash
docker compose version
```

## 3. Instalasi Linkding

### 3.1 Membuat Direktori

```bash
mkdir -p ~/linkding
cd ~/linkding
```

### 3.2 Mengunduh Docker Compose

```bash
wget https://raw.githubusercontent.com/sissbruecker/linkding/master/docker-compose.yml
```

### 3.3 Mengunduh File Environment

```bash
wget https://raw.githubusercontent.com/sissbruecker/linkding/master/.env.sample
```

Kemudian membuat file `.env`:

```bash
cp .env.sample .env
```

Struktur file:

```text
linkding/
├── .env
├── .env.sample
└── docker-compose.yml
```

### 3.4 Menjalankan Linkding

Linkding dijalankan menggunakan Docker Compose:

```bash
sudo docker compose up -d
```

Hasil:

```text
Image sissbruecker/linkding:latest Pulled
Network linkding_default Created
Container linkding Started
```

### 3.5 Mengecek Container

```bash
sudo docker ps
```

Hasil:

```text
STATUS: Up (healthy)
PORTS: 0.0.0.0:9090->9090/tcp
NAMES: linkding
```
```markdown
![Container Linkding Berjalan](images/03-linkding-container.png)
Hal ini menunjukkan bahwa container Linkding berhasil berjalan.

## 4. Akses Linkding

Linkding diakses melalui browser menggunakan alamat:

```text
http://172.17.77.48:9090
```

Aplikasi berhasil dibuka melalui browser.

## 5. Login

Akun pengguna dibuat menggunakan perintah:

```bash
sudo docker compose exec linkding python manage.py createsuperuser
```

Setelah akun dibuat, pengguna dapat login ke aplikasi Linkding.

## 6. Pengisian Konten

Setelah berhasil login, dilakukan penambahan bookmark menggunakan fitur **Add bookmark**.

Bookmark yang ditambahkan:

- Title: GitHub
- Tag: programming
- Tag: referensi

Bookmark berhasil muncul pada halaman **Bookmarks**.

## 7. Hasil Instalasi

Hasil instalasi menunjukkan bahwa:

- Docker berhasil berjalan pada VM.
- Linkding berhasil diinstal menggunakan Docker Compose.
- Container Linkding berstatus `Up (healthy)`.
- Linkding dapat diakses melalui browser.
- Pengguna dapat login.
- Bookmark berhasil ditambahkan.
- Tag berhasil digunakan.

## 8. Troubleshooting

Saat menjalankan:

```bash
docker compose up -d
```

muncul error:

```text
permission denied while trying to connect to the Docker API
```

Masalah tersebut diatasi dengan menjalankan:

```bash
sudo docker compose up -d
```

Setelah menggunakan `sudo`, Linkding berhasil dijalankan.
