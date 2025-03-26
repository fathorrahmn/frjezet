# Comprehensive Guide to STB Dashboard Setup and Configuration: Installation, Deployment, Database Migration with Docker and Microservices

* [1. Membuat SSH Key Di linux (WSL)](https://github.com/e-inwork/stb/README#mengunduh-repository)
* [2. Git Clone dari GitHub menggunakan SSH Key](https://github.com/datascienceid/README#memperbarui-repository)
* [Mengunggah perubahan ke dalam repository](https://github.com/datascienceid/README#mengunggah-perubahan)
* [Menghapus file](https://github.com/datascienceid/README#menghapus-file)
* [Branching](https://github.com/datascienceid/README#branching)
* [Perintah tambahan](https://github.com/datascienceid/README#perintah-tambahan)
* [gitignore](https://github.com/datascienceid/README#gitignore)

## 1. Membuat SSH Key Di linux (WSL)

* Buka Terminal WSL

* Jalankan perintah berikut:
   ```
   ssh-keygen
   ```
* Masukkan lokasi penyimpanan key (tekan ENTER untuk default ~/.ssh/id_xxx):
   ```
   Enter file in which to save the key (/home/user/.ssh/id_xxx): [Tekan ENTER]
   ```
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

## 2. Git Clone dari GitHub menggunakan SSH Key
* Masuk ke GitHub → Settings → SSH and GPG keys → New SSH key
* Paste SSH Public Key
* Klik Add SSH key
* Setelah SSH key berhasil dikonfigurasi, jalankan perintah berikut untuk clone repository:
   ```
   git clone git@github.com:e-inwork/stb.git
   ```

## STB DASHBOARD SETUP AND CONFIGURATION : 

## 3 Mengunggah Perubahan

Jangan lupa untuk melakukan pull terlebih dahulu sebelum melakukan push.

**Tambah file baru atau ubah file**

```
git add <nama file>
```

**Tambah beberapa file baru atau ubah beberapa file sekaligus**
```
git add .
```

**Konfirmasi penambahan atau perubahan file**

```
git commit -m "<pesan commit>"
```

**Ubah dan konfirmasi modifikasi beberapa file sekaligus**

```
git commit -a -m "<pesan commit>"
```

**Kirim perubahan ke dalam repository**

```
git push origin <nama branch>
```

#### Contoh

```
user@host:~$ git add README.md

user@host:~$ git commit -m "Menambahkan readme"
[master 224c510] Menambahkan readme
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

user@host~$ git push origin master
Counting objects: 3, done.
Delta compression using up to 16 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 271 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local objects.
To https://github.com/datascienceid/instagram.git
   fec3a1f..224c510  master -> master
```

## Menghapus File

Hapus file dari repository menggunakan perintah `git rm`, diikuti dengan `git commit`, dan `git push`.

```
git rm <nama file>
```

#### Contoh

```
user@host~$ git rm README.md
rm 'README.md'

user@host~$ git commit -m
[master 658a76e] Menghapus README
 1 file changed, 1 deletion(-)
 delete mode 100644 README.md

user@host~$
Counting objects: 3, done.
Delta compression using up to 16 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 236 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local objects.
To https://github.com/datascienceid.git
   224c510..658a76e  master -> master
```

## Branching

Branch digunakan untuk mengembangkan fitur baru atau mengubah source code tanpa
memberikan dampak kepada branch lain. Branch master adalah branch default dari
sebuah repository. Gunakan branch lain untuk melakukan pengembangan dan
gabungkan kembali ke dalam branch master.

### Melihat branch yang terdapat di dalam repository lokal

```
git branch
```

```
user@host~$ git branch
* master
```

Tanda asterisk (\*) menandakan branch yang sedang aktif.

### Melihat branch yang terdapat di dalam repository lokal

```
git branch -r
```

```
user@host~$ git branch -r
  origin/HEAD -> origin/master
  origin/master
```

### Membuat branch baru di dalam repository lokal dan kirim ke repository remote

**Buat branch baru**

```
git branch <nama branch baru>
```

**Aktifkan branch baru**

```
git checkout <nama branch baru>
```

**Konfirmasi perubahan**

```
git commit -m "<pesan konfirmasi>"
```

**Unggah branch baru ke dalam repository remote**

```
git push origin <nama branch baru>
```

#### Contoh

```
user@host~$ git branch development

user@host~$ git checkout development
Switched to branch 'development'

user@host~$ git commit -m "Menambah branch development"
On branch development
nothing to commit, working tree clean

user@host~$ git push origin development
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create pull request for new:
remote:   https://github.com/datascienceid/instagram/pull-requests/new?source=new&t=1
remote:
To https://github.com/datascienceid/instagram.git
 * [new branch]      development -> development
```

### Menambahkan branch dari repository remote ke dalam repository lokal

```
git branch <nama branch remote>
git pull origin <nama branch remote>
git checkout <nama branch remote>
```

#### Contoh

```
user@host~$ git branch development

user@host~$ git pull origin development
 * branch            new        -> FETCH_HEAD
 * [new branch]      new        -> origin/development
Already up-to-date.

user@host~$ git checkout development
Switched to branch 'development'
```

### Menggabungkan branch lain ke dalam branch aktif

**Aktifkan branch yang diinginkan**

```
git checkout <nama branch aktif>
```

**Perbarui branch local**

```
git pull origin <nama branch aktif>
```

**Penggabungan**

```
git merge <nama branch yang akan digabungkan>
```

**Cek dan selesaikan konflik akibat penggabungan branch**

```
git status
```

**Konfirmasi dan unggah penggabungan branch**

```
git commit -m "<pesan konfirmasi>" -a
git push origin <nama branch aktif>
```

#### Menghapus branch

**Pada repository remote**

```
git push origin :<nama branch>
```

**Pada repository lokal**

```
git branch <nama branch> -d
```

## Perintah tambahan

Dapatkan status dari repository

```
git status
```

Mengecek perubahan yang belum terkonfirmasi dalam repository

```
git diff
```

Mengecek perubahan yang sudah terkonfirmasi dalam repository

```
git show
```

Dapatkan log dari sebuah repository

```
git log
```

Dapatkan hanya beberapa log terakhir dari sebuah repository (contoh: 3)

```
git log -3
```

## gitignore
Ada kalanya kita melihat file gitignore di suatu repository. Apakah itu gitignore? gitignore adalah file yang berisi instruksi kepada git repository untuk tidak men-track files tertentu. Ini sangat berguna untuk meng-exclude files yang mungkin tidak berguna atau tidak perlu di push ke repository. Contoh: .DS_Store di Mac, binary files, `__pycache__`, etc. 

File gitignore dimulai dengan titik (`.`) di Unix-based system (Mac dan Linux) untuk menandakan dia adalah hidden file. Di Windows, buat file gitignore dengan memberi nama `.gitignore`.

#### Contoh

Untrack file tertentu
```
# tagar digunakan untuk commenting
# Mac
.DS_Store

# Spreadsheet
*.xls
*.xlsx

# Compressed file
*.zip
*.rar
*.gz
```

Untrack folder tertentu
```
# tagar digunakan untuk commenting
# python
__pycache__/

# virtual environment
env/
venv/
```

Contoh koleksi gitignore yang berguna  
https://github.com/github/gitignore
