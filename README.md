# Comprehensive Guide to STB Dashboard Setup and Configuration: Installation, Deployment, Database Migration with Docker and Microservices

* [Membuat SSH Key Di linux (WSL)](https://github.com/e-inwork/stb/README#mengunduh-repository)
* [Git Clone dari GitHub menggunakan SSH Key](https://github.com/datascienceid/README#memperbarui-repository)
* [Mengunggah perubahan ke dalam repository](https://github.com/datascienceid/README#mengunggah-perubahan)
* [Menghapus file](https://github.com/datascienceid/README#menghapus-file)
* [Branching](https://github.com/datascienceid/README#branching)
* [Perintah tambahan](https://github.com/datascienceid/README#perintah-tambahan)
* [gitignore](https://github.com/datascienceid/README#gitignore)

## Membuat SSH Key Di linux (WSL)

* Buka Terminal WSL
* Jalankan perintah berikut ```ssh-keygen```
* Masukkan lokasi penyimpanan key
  - tekan ENTER untuk default ```~/.ssh/id_xxx)```
  - tekan Enter file in which to save the key ```(/home/user/.ssh/id_xxx)```
* Buat passphrase (opsional, kosongkan jika tidak ingin password):
   ```
   Enter passphrase (empty for no passphrase): [Masukkan password atau tekan ENTER]
   ```
* Anda bisa melihat SSH Public Key dengan perintah dibawah dan tekan enter :
   ```
   cat ~/.ssh/id_xxx.pub
   ```
* Salin SSH Public Key 
   ```
   ssh-xxx AAA sampai akhir
   ```

## Git Clone dari GitHub menggunakan SSH Key
* Masuk ke GitHub → Settings → SSH and GPG keys → New SSH key
* Paste SSH Public Key
* Klik Add SSH key
* Buat dan tentukan repository di komputer lokal untuk clone, seperti contoh berikut.
   ```
   ubuntu@msi:~/project/ (git clone disini)
   ```
* Setelah SSH key berhasil dikonfigurasi, jalankan perintah berikut untuk clone repository:
   ```
   git clone git@github.com:e-inwork/stb.git
   ```
   
## STB DASHBOARD SETUP AND CONFIGURATION : 
Setelah git clone, cek repository: ```Ubuntu@MSI:~/projects/stb/microservice$```. 
Lalu, buka DB Service: ```Ubuntu@MSI:~/projects/stb/microservice$/db-service```  dan buka dengan perintah ```code .``` Enter.

## Jalankan Docker Desktop yang sudah terinstall di PC.

## Create Docker Service Network
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/db-service```.
* Buat service network ```docker network create sercive_network```

## DB Service
* Jalankan service network dengan ```docker compose up -d```
  
**Lakukan migrate Di DB Service**
* Masuk ke app container yoyo: ```docker compose run yoyo service bash``` Menandakan sudah masuk ke container yoyo.
* Cek tabel di DB Service (tabel belum ada dan perlu dicek):
  Jalankan ```docker ps```
  (Cek container yang berjalan, pastikan ada ```db-service-yoyo``` & ```db_service```.
* Masuk ke SQL dalam DB Service ```docker compose exec db_service bash``` Jangan gunakan run, karena container sudah berjalan!.
* Masuk ke user Postgres ```su – postgres```.
* Masuk ke database dengan psql ```psql```.
  - Cek daftar database: ```\l```.
  - Cek daftar tabel: ```\dt```.
* Jalankan migrasi tabel ```yoyo apply``` Saat diminta konfirmasi migrasi, ketik Y.

## Storage Service
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/storage-service```.
* Jalankan Storage Service dengan ```docker compose up -d```

## Proxy Service
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/proxy-service```
* Jalankan Proxy Service dengan ```docker compose up -d```

## Super User Service
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/superuser-service```
* Jalankan Super User Service dengan ```docker compose up -d```

## Group Service
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/group-service```
* Jalankan Group Service dengan ```docker compose up -d```

## User Service
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/user-service```
* Jalankan User Service dengan ```docker compose up -d```

## Group User Service
* Masuk ke repository ```Ubuntu@MSI:~/projects/stb/microservice$/groupuser-service```
* Jalankan Group User Service dengan ```docker compose up -d```

## Setelah semua berjalan, cek Proxy Service, envoy, dan port. Jika lengkap, 
* buka browser dan masukkan ```localhost:9901```
* Proxy bertujuan menyambungkan semua network ke satu port dengan berbagai jenis port.
* Untuk melihat yang sudah digabungkan dalam satu proxy, buka browser. Di daftar command, cari clusters untuk menampilkan berbagai proxy.
* Jika dalam cluster muncul superuser-service, user-service, group-service, dan lainnya, berarti proxy berjalan baik.
* Jika tidak, lakukan langkah berikut:
  - Masuk ke proxy-service: ```cd ~/projects/stb/microservice/proxy-service```
  - Matikan proxy: ```docker compose down```
  - Build ulang tanpa cache: ```docker compose build --no-cache```
  - Dan hidupkan kembali : ```docker compose up -d```

## Install NVM & NPM :
* masukkan perintah berikut ```curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash```
* Setelah instalasi, tambahkan NVM ke ```~/.bashrc``` atau ```~/.zshrc```:
  ```
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
  ```
* Setelah instalasi NVM selesai, buka terminal di super user dashboard: ```cd ~/projects/stb/dashaboard/superuser-dashboard```
* Instal Node.js versi 22: ```nvm install 22```
* Gunakan Node.js versi 22: ```nvm use 22``` _Catatan: Program hanya berjalan dengan Node.js 22 atau lebih_
* Instal dependensi: ```npm install```
* Jika saat ```npm install``` muncul perintah audit, jalankan ```npm audit fix```
* Setelah audit selesai, buat file ```.env.local``` di superuser-dashboard
  ```
  NEXT_PUBLIC_ENVOY_HOST=http://localhost:8080/
  ```

