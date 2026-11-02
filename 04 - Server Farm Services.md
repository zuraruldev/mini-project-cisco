---
title: Server Farm Services - KampusKita University
tags:
  - server
  - dns
  - web
  - ftp
  - email
  - kampuskita-network
---

# Server Farm Services — KampusKita University

> 🧭 **Navigasi Cepat:** [[00 - KampusKita Network MOC|🏠 Hub MOC]] • [[02 - IP Addressing & Subnetting Plan|🔢 Rencana IP]] • [[06 - End Device Verification & Email Testing|🧪 Uji Layanan]]  
> *(💡 Tips Obsidian: Tahan tombol **Ctrl** lalu klik link, atau klik langsung pada mode **Reading View**).*

Empat server utama diletakkan di subnet `192.168.10.0/24` dengan konfigurasi IP statis dan gateway `192.168.10.1`.

---

## 1. DNS Server (`192.168.10.2`)

DNS Server adalah komponen sentral agar seluruh end-device dapat mengakses server menggunakan nama domain daripada alamat IP angka.

### Pengaturan IP:
- **IP Address:** `192.168.10.2`
- **Subnet Mask:** `255.255.255.0`
- **Default Gateway:** `192.168.10.1`
- **DNS Server:** `127.0.0.1` (atau `192.168.10.2`)

### Konfigurasi Service → DNS:
- **DNS Service:** `ON`
- Buat entri A Record:
  1. Name: `kampuskita.ac.id` → Address: `192.168.10.3`
  2. Name: `www.kampuskita.ac.id` → Address: `192.168.10.3`
  3. Name: `ftp.kampuskita.ac.id` → Address: `192.168.10.4`
  4. Name: `mail.kampuskita.ac.id` → Address: `192.168.10.5`

---

## 2. Web Server (`192.168.10.3`)

Menyediakan portal kampus, Siakad, dan Elearning.

### Pengaturan IP:
- **IP Address:** `192.168.10.3`
- **Subnet Mask:** `255.255.255.0`
- **Default Gateway:** `192.168.10.1`
- **DNS Server:** `192.168.10.2`

### Konfigurasi Service → HTTP:
- **HTTP & HTTPS:** `ON`
- Edit file `index.html`:
  ```html
  <html>
    <head><title>KampusKita University</title></head>
    <body style="font-family:sans-serif; text-align:center; padding:40px;">
      <h1>Selamat Datang di KampusKita University</h1>
      <p>Portal Resmi Layanan Akademik & Kemahasiswaan</p>
      <hr>
      <p>Layanan: [Siakad] | [E-Learning] | [FTP Cloud] | [Webmail]</p>
    </body>
  </html>
  ```

---

## 3. FTP Server (`192.168.10.4`)

Digunakan sebagai tempat penyimpanan cloud repositori kampus.

### Pengaturan IP:
- **IP Address:** `192.168.10.4`
- **Subnet Mask:** `255.255.255.0`
- **Default Gateway:** `192.168.10.1`
- **DNS Server:** `192.168.10.2`

### Konfigurasi Service → FTP:
- **FTP Service:** `ON`
- Buat Akun Pengguna:
  - User 1: `admin` | Password: `admin123` | Permissions: `[x] Read [x] Write [x] Delete [x] Rename [x] List`
  - User 2: `mahasiswa` | Password: `mhs123` | Permissions: `[x] Read [ ] Write [ ] Delete [ ] Rename [x] List`

---

## 4. Mail Server (`192.168.10.5`)

Layanan email civitas akademika dengan protokol SMTP dan POP3.

### Pengaturan IP:
- **IP Address:** `192.168.10.5`
- **Subnet Mask:** `255.255.255.0`
- **Default Gateway:** `192.168.10.1`
- **DNS Server:** `192.168.10.2`

### Konfigurasi Service → EMAIL:
- **SMTP Service:** `ON`
- **POP3 Service:** `ON`
- **Domain Name:** `kampuskita.ac.id` (Klik **Set**)
- **User Accounts:**
  - `admin` / `pass123`
  - `dosen` / `pass123`
  - `mhs1` / `pass123`
  - `mhs2` / `pass123`

---

## 🔗 Referensi Silang
- Pengujian Akun Email: [[06 - End Device Verification & Email Testing]]
- Pembatasan Firewall Tamu: [[03 - Core Router Configuration & ACL]]
