# Project KDJK - Implementasi Linkding

## Anggota Kelompok

- Azizah Az-Zahra (M0403241048)
- Mirabel Nasywa Rajendraputri (M0403241067)
- Muhammad Riady Hendrawan (M0403241103)
- Muhammad Fauzan Rizvi (M0403241142)
- Dini Aulia Wulandari (M0403241143)

## Deskripsi

Project ini merupakan implementasi aplikasi web self-hosted yang dipilih dari repository Awesome Selfhosted.

Aplikasi yang dipilih adalah **Linkding**, yaitu aplikasi web self-hosted untuk menyimpan, mengelola, dan mengorganisasi bookmark atau URL.

## Tujuan

1. Menginstal Linkding pada VM lokal.
2. Melakukan konfigurasi aplikasi.
3. Mengisi konten aplikasi.
4. Menguji fitur utama aplikasi.
5. Melakukan demo penggunaan.
6. Membandingkan Linkding dengan aplikasi web sejenis.

## Progress

- [x] Menentukan aplikasi
- [x] Membuat repository GitHub
- [x] Menambahkan anggota kelompok
- [x] Instalasi Linkding pada VM
- [x] Konfigurasi dasar
- [x] Pengisian konten awal
- [ ] Pengujian fitur utama
- [x] Dokumentasi instalasi
- [ ] Demo
- [ ] Perbandingan dengan aplikasi sejenis

---

# Sekilas Tentang

Linkding adalah aplikasi web self-hosted yang digunakan untuk mengelola bookmark atau URL secara mandiri. Aplikasi ini memiliki pendekatan yang sederhana dan berfokus pada penyimpanan serta pengorganisasian bookmark.

Linkding dapat digunakan untuk menyimpan bookmark dan mengelompokkannya menggunakan tag. Aplikasi ini juga menyediakan fitur pencarian bookmark, notes, import dan export bookmark, REST API, serta dukungan multi-user. Linkding dirancang untuk dijalankan menggunakan container seperti Docker dan menggunakan SQLite sebagai database secara default.

Pada project ini, Linkding diinstal pada VM lokal menggunakan Docker dan Docker Compose.

# Instalasi

## Kebutuhan Sistem

Prasyarat yang digunakan dalam instalasi:

- VM lokal
- Sistem operasi Ubuntu
- Koneksi internet
- Docker
- Docker Compose
- Web browser

Pada pengujian project ini digunakan:

- Username VM: `asus`
- IP Address VM: `172.17.77.48`
- Port Linkding: `9090`

## Proses Instalasi

### 1. Memperbarui repository Ubuntu

```bash
sudo apt update
```

### 2. Menginstal kebutuhan Docker

```bash
sudo apt install ca-certificates curl
```

Kemudian membuat direktori keyrings:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Mengunduh GPG key Docker:

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

Memberikan permission:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Menambahkan repository Docker:

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

Kemudian:

```bash
sudo apt update
```

### 3. Menginstal Docker dan Docker Compose

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Mengecek status Docker:

```bash
sudo systemctl status docker
```

Hasil:

```text
Active: active (running)
```

Mengecek Docker:

```bash
docker --version
```

Mengecek Docker Compose:

```bash
docker compose version
```

### 4. Membuat direktori Linkding

```bash
mkdir -p ~/linkding
cd ~/linkding
```

### 5. Mengunduh file Docker Compose

```bash
wget https://raw.githubusercontent.com/sissbruecker/linkding/master/docker-compose.yml
```

### 6. Mengunduh file environment

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

### 7. Menjalankan Linkding

```bash
sudo docker compose up -d
```

Hasil proses:

```text
Image sissbruecker/linkding:latest Pulled
Network linkding_default Created
Container linkding Started
```

### 8. Mengecek container

```bash
sudo docker ps
```

Hasil pengujian menunjukkan:

```text
STATUS: Up (healthy)
PORTS: 0.0.0.0:9090->9090/tcp
NAMES: linkding
```

![Container Linkding Berjalan](dokumentasi/images/03-linkding-container.png)

### 9. Membuat akun pengguna

Setelah container berhasil berjalan, dibuat akun pengguna dengan perintah:

```bash
sudo docker compose exec linkding python manage.py createsuperuser
```

Kemudian dimasukkan username, email, dan password.

### 10. Mengakses aplikasi

Linkding dapat diakses menggunakan:

```text
http://172.17.77.48:9090
```

![Dashboard Linkding](dokumentasi/images/04-linkding-dashboard.png)

# Konfigurasi

Konfigurasi dasar Linkding dilakukan melalui file `.env`.

Pada instalasi ini digunakan konfigurasi Docker dengan port:

```text
9090
```

Port tersebut dipetakan ke port `9090` pada container Linkding.

Linkding menggunakan SQLite sebagai database default dan data aplikasi disimpan pada direktori data yang dipetakan ke container.

Tidak digunakan domain atau reverse proxy karena aplikasi dijalankan untuk kebutuhan pengujian pada VM lokal.

# Maintenance

Maintenance dilakukan untuk memastikan container Linkding tetap berjalan dan data aplikasi dapat dipelihara.

## Mengecek container

```bash
sudo docker ps
```

## Melihat log

```bash
sudo docker compose logs
```

## Restart aplikasi

```bash
sudo docker compose restart
```

## Backup

Linkding menyediakan mekanisme backup penuh untuk data aplikasi. Backup dapat dilakukan dengan perintah:

```bash
sudo docker exec -it linkding python manage.py full_backup /etc/linkding/data/backup.zip
```

Kemudian file backup dapat disalin ke host:

```bash
sudo docker cp linkding:/etc/linkding/data/backup.zip backup.zip
```

# Otomatisasi

Pada project ini, proses menjalankan aplikasi dilakukan menggunakan Docker Compose:

```bash
sudo docker compose up -d
```

Docker Compose juga menggunakan mekanisme restart container sehingga container dapat dijalankan kembali oleh Docker setelah service aktif, selama container tidak dihentikan secara manual.

Untuk tahap selanjutnya, perintah instalasi dan maintenance dapat dikembangkan menjadi shell script agar proses dapat dijalankan secara otomatis.

# Cara Pemakaian

## 1. Login

Buka browser kemudian akses:

```text
http://172.17.77.48:9090
```

Setelah halaman terbuka, pengguna melakukan login menggunakan akun yang telah dibuat.

## 2. Dashboard

Setelah login, pengguna diarahkan ke halaman bookmark.

![Dashboard Linkding](dokumentasi/images/04-linkding-dashboard.png)

## 3. Menambahkan Bookmark

Untuk menambahkan bookmark, pengguna memilih tombol **Add bookmark**.

Pada pengujian awal, bookmark yang ditambahkan adalah:

- URL: `https://github.com/`
- Title: `GitHub`
- Tag: `programming`
- Tag: `referensi`

## 4. Mengorganisasi Bookmark

Tag digunakan untuk mengelompokkan bookmark berdasarkan kategori.

Contoh:

```text
programming
referensi
```

Hasil bookmark:

![Daftar Bookmark Linkding](dokumentasi/images/05-linkding-bookmarks.png)

## 5. Fungsi Utama

Fungsi yang dapat digunakan pada Linkding antara lain:

- Menambahkan bookmark
- Mengedit bookmark
- Menghapus bookmark
- Mencari bookmark
- Menggunakan tag
- Menambahkan notes
- Import dan export bookmark
- Mengakses bookmark melalui API

Linkding menyediakan REST API yang dapat digunakan oleh aplikasi pihak ketiga untuk mengelola bookmark.

# Pembahasan

## Kelebihan

Berdasarkan hasil instalasi dan penggunaan, Linkding memiliki beberapa kelebihan:

- Instalasi relatif sederhana menggunakan Docker.
- Antarmuka sederhana dan berfokus pada pengelolaan bookmark.
- Bookmark dapat dikelompokkan menggunakan tag.
- Mendukung import dan export bookmark.
- Menyediakan REST API.
- Dapat dijalankan secara self-hosted.
- Menggunakan SQLite sebagai database default sehingga konfigurasi awal relatif sederhana.

## Kekurangan

Beberapa kekurangan yang ditemukan:

- Pengguna perlu memiliki pemahaman dasar mengenai Docker dan terminal untuk instalasi self-hosted.
- Tampilan lebih minimal dibandingkan beberapa aplikasi bookmark manager lain.
- Pada project ini, aplikasi hanya dapat diakses melalui IP VM lokal karena belum menggunakan domain atau reverse proxy.

## Perbandingan dengan Linkwarden

Linkding dibandingkan dengan **Linkwarden**, yaitu aplikasi bookmark manager self-hosted yang juga berfokus pada pengelolaan dan penyimpanan link.

Linkding memiliki pendekatan yang lebih sederhana dan minimal untuk mengelola bookmark. Sementara itu, Linkwarden menyediakan fitur yang lebih berorientasi pada preservation dan kolaborasi. Linkwarden menggunakan konsep link, collection, dan tags, serta dapat menyimpan salinan halaman dalam bentuk screenshot dan PDF. Linkwarden juga menyediakan fitur pembacaan dan anotasi serta kolaborasi pada collection.

Perbandingan:

| Aspek | Linkding | Linkwarden |
|---|---|---|
| Fokus utama | Bookmark management | Bookmark management dan preservation |
| Self-hosted | Ya | Ya |
| Tag | Ya | Ya |
| Collection | Lebih sederhana | Ya |
| Screenshot halaman | Tidak menjadi fokus utama | Ya |
| PDF halaman | Tidak menjadi fokus utama | Ya |
| Kolaborasi | Multi-user | Collaboration pada collection |
| REST API | Ya | Ya |
| Kompleksitas | Relatif sederhana | Lebih kompleks |

Berdasarkan perbandingan tersebut, Linkding lebih sesuai untuk pengguna yang membutuhkan pengelolaan bookmark secara sederhana dan ringan. Linkwarden lebih sesuai untuk kebutuhan pengarsipan halaman web dan kolaborasi yang lebih luas.

# Referensi

1. Linkding - Installation  
   https://linkding.link/installation/

2. Linkding - API  
   https://linkding.link/api/

3. Linkding - Official Repository  
   https://github.com/sissbruecker/linkding

4. Linkwarden - Documentation  
   https://docs.linkwarden.app/

5. Template laporan Project Akhir KDJK

6. Contoh laporan tahun sebelumnya - OneStyd/PrestaShop  
   https://github.com/OneStyd/prestashop
