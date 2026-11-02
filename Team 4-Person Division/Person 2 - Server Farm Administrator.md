---
title: Person 2 - Server Farm Administrator
tags:
  - role-guide
  - person-2
  - server
  - dns
  - web
  - ftp
  - email
  - kampuskita-network
---

# Person 2 — Server Farm Administrator

> 🧭 **Navigasi Cepat:** [[00 - Team Overview & Roadmap|👥 Overview Tim]] • [[00 - KampusKita Network MOC|🏠 Hub MOC Utama]]  
> *(💡 Tips Obsidian: Tahan tombol **Ctrl** lalu klik link, atau klik langsung pada mode **Reading View**).*

Sebagai **Person 2**, Anda bertanggung jawab penuh atas konfigurasi 4 unit server fisik pada **Zona Server Farm (Cyan)**: `DNS-SERVER`, `WEB-SERVER`, `FTP-SERVER`, dan `MAIL-SERVER`. Seluruh server di-setting menggunakan **IP Statis**.

---

## 🛠️ Daftar Tugas Spesifik Anda

- [ ] **Tugas 2.1:** Menghubungkan 4 unit `Server-PT` ke `SW-SERVER` menggunakan kabel Copper Straight-Through.
- [ ] **Tugas 2.2:** Mengisi konfigurasi IP statis, Subnet Mask, Gateway, dan DNS pada ke-4 server.
- [ ] **Tugas 2.3:** Mengaktifkan layanan DNS dan mendaftarkan 4 buah domain A-Record.
- [ ] **Tugas 2.4:** Mengaktifkan HTTP/HTTPS pada Web Server dan memodifikasi file `index.html`.
- [ ] **Tugas 2.5:** Mengaktifkan FTP Server dan membuat akun `admin` serta `mahasiswa`.
- [ ] **Tugas 2.6:** Mengaktifkan Mail Server (SMTP & POP3), mendaftarkan domain `kampuskita.ac.id`, dan membuat akun email pengguna.

---

## 📋 Langkah Eksekusi Teknis

### 1. Pengaturan IP Statis 4 Server (Desktop ➔ IP Configuration)

| Nama Server | IP Address | Subnet Mask | Default Gateway | DNS Server |
| :--- | :--- | :--- | :--- | :--- |
| **`DNS-SERVER`** | `192.168.10.2` | `255.255.255.0` | `192.168.10.1` | `127.0.0.1` |
| **`WEB-SERVER`** | `192.168.10.3` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` |
| **`FTP-SERVER`** | `192.168.10.4` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` |
| **`MAIL-SERVER`** | `192.168.10.5` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` |

---

### 2. Konfigurasi Layanan Per Server (Tab Services)

#### A. `DNS-SERVER` (`192.168.10.2`)
1. Buka tab **Services** ➔ klik menu **DNS**.
2. Ubah radio button **DNS Service** menjadi **`ON`**.
3. Tambahkan 4 record berikut (klik **Add** setelah mengisi masing-masing):
   - Name: `kampuskita.ac.id` ➔ Address: `192.168.10.3` (Type: A Record)
   - Name: `www.kampuskita.ac.id` ➔ Address: `192.168.10.3` (Type: A Record)
   - Name: `ftp.kampuskita.ac.id` ➔ Address: `192.168.10.4` (Type: A Record)
   - Name: `mail.kampuskita.ac.id` ➔ Address: `192.168.10.5` (Type: A Record)

#### B. `WEB-SERVER` (`192.168.10.3`)
1. Buka tab **Services** ➔ klik menu **HTTP**.
2. Pastikan **HTTP** dan **HTTPS** dalam status **`ON`**.
3. Pada baris file `index.html`, klik tautan **edit**, masukkan kode HTML:
   ```html
   <html>
     <body style="font-family: sans-serif; text-align: center; padding-top: 50px;">
       <h1 style="color: #0b5394;">Portal Resmi KampusKita University</h1>
       <p>Selamat datang di sistem informasi akademik terpadu.</p>
       <hr style="width: 60%;">
       <p>Layanan: [SIAKAD] | [E-LEARNING] | [CLOUD REPOSITORY] | [WEBMAIL]</p>
     </body>
   </html>
   ```
4. Klik **Save** ➔ Konfirmasi **Yes** (Overwrite).

#### C. `FTP-SERVER` (`192.168.10.4`)
1. Buka tab **Services** ➔ klik menu **FTP**.
2. Ubah **FTP Service** menjadi **`ON`**.
3. Buat 2 akun pengguna:
   - **User 1:** Username `admin` | Password `admin123`  
     Centang semua permission: `[x] Read, [x] Write, [x] Delete, [x] Rename, [x] List` ➔ Klik **Add**.
   - **User 2:** Username `mahasiswa` | Password `mhs123`  
     Centang permission: `[x] Read, [ ] Write, [ ] Delete, [ ] Rename, [x] List` ➔ Klik **Add**.

#### D. `MAIL-SERVER` (`192.168.10.5`)
1. Buka tab **Services** ➔ klik menu **EMAIL**.
2. Pastikan **SMTP Service** dan **POP3 Service** berstatus **`ON`**.
3. Pada kolom **Domain Name**, ketik: `kampuskita.ac.id` ➔ Klik tombol **Set**.
4. Buat akun email pengguna (isi User & Password, lalu klik tanda `+`):
   - User: `admin` | Password: `pass123`
   - User: `dosen` | Password: `pass123`
   - User: `mahasiswa` | Password: `pass123`

---

## 🔍 Uji Cepat Mandiri
Buka tab *Desktop ➔ Web Browser* pada Web Server itu sendiri, ketik URL `http://kampuskita.ac.id`. Halaman HTML harus tampil sempurna. Jika tampil, infokan ke **Person 4** bahwa Server Farm siap diuji!
