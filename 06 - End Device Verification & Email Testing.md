---
title: End Device Verification & Email Testing - KampusKita University
tags:
  - testing
  - verification
  - email-test
  - packet-tracer
  - kampuskita-network
hub: "[[00 - KampusKita Network MOC]]"
related:
  - "[[01 - Network Architecture & Topology]]"
  - "[[03 - Core Router Configuration & ACL]]"
  - "[[04 - Server Farm Services]]"
  - "[[07 - Implementation Roadmap & EdLink Submission]]"
---

# End Device Verification & Email Testing — KampusKita University

> **MOC:** [[00 - KampusKita Network MOC]] | **Topologi Gambar 1:** [[01 - Network Architecture & Topology]] | **Router & ACL:** [[03 - Core Router Configuration & ACL]]

Matriks pengujian berikut disusun berdasarkan nama-nama perangkat riil pada **Gambar 1** untuk membuktikan bahwa seluruh end-device dapat saling berkomunikasi, dapat berkirim email, dan aturan pemblokiran ACL bekerja 100%.

---

## 1. Matriks Pengujian End-to-End

| Kode | Perangkat Sumber | Perangkat Tujuan | Protokol / Pengujian | Hasil yang Diharapkan |
| :--- | :--- | :--- | :--- | :--- |
| **U-01** | `PC-LAB-1` | `DHCP-SERVER-LAB-KOMPUTER` | DHCP Discovery | Menerima IP `192.168.20.X`, DNS `192.168.10.2` |
| **U-02** | `PC-ADMIN-1` | `DHCP-SERVER-ADMIN` | DHCP Discovery | Menerima IP `192.168.30.X`, DNS `192.168.10.2` |
| **U-03** | `SmartphoneTamu-1` | `DHCP-SERVER-UMUM` | DHCP Discovery | Menerima IP `192.168.40.X`, DNS `192.168.10.2` |
| **U-04** | `PC-LAB-3` | `kampuskita.ac.id` | HTTP (Web Browser) | Terbuka halaman portal KampusKita |
| **U-05** | `PC-ADMIN-2` | `ftp.kampuskita.ac.id` | FTP (Command Prompt) | Berhasil login dengan user `admin` |
| **U-06** | `PC-ADMIN-1` | `LaptopDosen-1` | Email Send (SMTP port 25) | Status `Send Success` |
| **U-07** | `LaptopDosen-1` | `MAIL-SERVER` | Email Receive (POP3 port 110) | Email dari admin masuk dan dapat di-Reply |
| **U-08** | `LaptopMahasiswa-1` | `PC-LAB-5` | ICMP Ping antar subnet | Reply from `192.168.20.X`: bytes=32 time<1ms |
| **U-09** | `SmartphoneTamu-1` | `kampuskita.ac.id` | HTTP Browser | **Berhasil Terbuka** (Akses Web diizinkan) |
| **U-10** | `SmartphoneTamu-1` | `MAIL-SERVER` (`192.168.10.5`)| ICMP Ping / Email POP3 | **GAGAL / Destination host unreachable** *(Bukti ACL Aktif!)* |

---

## 2. Praktik Konfigurasi Email Antar Perangkat

### Skenario: Admin berkirim email dengan Dosen
1. **Konfigurasi Akun pada `PC-ADMIN-1`:**
   - Buka Desktop ➔ **Email**:
     - Your Name: `Staf Administrasi`
     - Email Address: `admin@kampuskita.ac.id`
     - Incoming Mail Server: `mail.kampuskita.ac.id` (atau `192.168.10.5`)
     - Outgoing Mail Server: `mail.kampuskita.ac.id`
     - User Name: `admin` | Password: `pass123`
     - Klik **Save**.
2. **Konfigurasi Akun pada `LaptopDosen-1`:**
   - Buka Desktop ➔ **Email**:
     - Your Name: `Dosen Pengampu`
     - Email Address: `dosen@kampuskita.ac.id`
     - Incoming & Outgoing: `mail.kampuskita.ac.id`
     - User Name: `dosen` | Password: `pass123`
     - Klik **Save**.
3. **Kirim Email dari `PC-ADMIN-1`:**
   - Klik **Compose**:
     - To: `dosen@kampuskita.ac.id`
     - Subject: `Permohonan Nilai Akhir Mini Project`
     - Isi: `Yth. Dosen, mohon kesediaan waktu untuk pemaparan hasil topologi.`
     - Klik **Send**. Pastikan muncul teks `Send Success` di pojok kiri bawah.
4. **Terima & Balas dari `LaptopDosen-1`:**
   - Klik tombol **Receive**.
   - Email dari `admin@kampuskita.ac.id` akan tampil di daftar kotak masuk.
   - Klik email ➔ Klik **Reply** ➔ Ketik balasan ➔ Klik **Send**.

---

## 3. Pembuktian Aturan ACL (Blokir WiFi Tamu ke Mail Server)

Buka `SmartphoneTamu-1` (atau `LaptopTamu-1`):

1. **Uji Akses Web (Harus Berhasil):**
   - Buka *Desktop ➔ Web Browser*.
   - URL: `kampuskita.ac.id`.
   - Web terbuka sukses karena aturan baris `20 permit ip any any` pada ACL 101.
2. **Uji Akses Mail Server (Harus Diblokir):**
   - Buka *Desktop ➔ Command Prompt*.
   - Ketik: `ping 192.168.10.5` (IP `MAIL-SERVER`).
   - Hasil yang tampil:
     ```text
     Pinging 192.168.10.5 with 32 bytes of data:
     Reply from 192.168.40.1: Destination host unreachable.
     Reply from 192.168.40.1: Destination host unreachable.
     ```
   - Screenshot tampilan ini sebagai bukti utama laporan PDF untuk rubrik nilai A!

---

## 🔗 Referensi Silang
- Checklist Rubrik Laporan: [[07 - Implementation Roadmap & EdLink Submission]]
- Pengaturan ACL Router: [[03 - Core Router Configuration & ACL]]
- Setup Server: [[04 - Server Farm Services]]
