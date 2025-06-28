# blockIP
DaftarIP blacklist
Ini adalah IPabuse yang terdeteksi melakukan prilaku tidak berguna dan bukan manusia


---

### 🔐 Blokir IP Otomatis di Server Linux Menggunakan CSF dan GitHub

Artikel ini menjelaskan cara mengatur **ConfigServer Security & Firewall (CSF)** untuk **memblokir daftar IP jahat** dari file `ip.txt` yang Anda simpan di GitHub. Proses ini otomatis dan akan diperbarui setiap jam.

---

## Langkah-langkah Instalasi

### 1. Pastikan CSF Sudah Terpasang

Jika belum, install CSF dengan perintah berikut:

```bash
cd /usr/src
wget https://download.configserver.com/csf.tgz
tar -xzf csf.tgz
cd csf
sh install.sh
```

---

### 2. Buat File `ip.txt` di GitHub

Buat file bernama `ip.txt` di repository Anda. Isi dengan daftar IP satu per baris, misalnya:

```
175.111.91.36
103.77.107.199
136.243.220.212
3.70.77.181
64.227.159.167
167.71.210.71
```

Contoh URL file GitHub:

```
https://raw.githubusercontent.com/firdausekabkl/blockIP/main/ip.txt
```

---

### 3. Tambahkan Daftar Blokir ke CSF

Edit file blocklist CSF:

```bash
nano /etc/csf/csf.blocklists
```

Tambahkan baris ini:

```
GITHUBIP|3600|100|https://raw.githubusercontent.com/firdausekabkl/blockIP/main/ip.txt
```

Penjelasan:

* `GITHUBIP`: nama daftar blocklist (bebas)
* `3600`: update setiap 3600 detik (1 jam)
* `100`: maksimal 100 IP
* URL: lokasi `ip.txt` di GitHub

---

### 4. Aktifkan IPSET dan BLOCKLIST

Edit file konfigurasi utama CSF:

```bash
nano /etc/csf/csf.conf
```

Pastikan nilai berikut diatur:

```
LF_IPSET = "1"
BLOCKLIST = "1"
```

Simpan dan keluar dari editor.

---

### 5. Restart CSF

Setelah semua pengaturan selesai, restart CSF agar perubahan aktif:

```bash
csf -ra
```

---

## Verifikasi

Untuk mengecek apakah IP tertentu sudah diblokir:

```bash
csf -g 136.243.220.212
```

Jika IP ditemukan di IPSET, artinya pemblokiran sudah aktif:

```
IPSET: Set:bl_GITHUBIP Match:136.243.220.212
```

Atau cek semua IP dalam IPSET:

```bash
ipset list bl_GITHUBIP
```

---

## FAQ

### Apakah IP yang saya blokir manual akan tertimpa?

❌ Tidak. IP yang Anda masukkan secara manual di `/etc/csf/csf.deny` **tidak akan dihapus atau ditimpa** oleh sistem blocklist GitHub ini.

### Apakah `ip.txt` akan diambil otomatis?

✅ Ya. Selama CSF aktif dan blocklist diatur, CSF akan **mengupdate daftar IP setiap 1 jam** dari file tersebut.

---

## Penutup

Dengan cara ini, Anda dapat:

* Mengelola blokir IP dari satu file GitHub
* Memperbarui daftar blokir tanpa login ke server
* Menambahkan/menghapus IP secara cepat dari satu tempat

Cukup update `ip.txt`, dan server Anda akan otomatis memperbarui blokir IP-nya. Sangat cocok untuk Anda yang ingin manajemen IP yang **mudah, efisien, dan otomatis**.

Jika ingin dibuatkan script backup atau template GitHub-nya juga, silakan minta.
