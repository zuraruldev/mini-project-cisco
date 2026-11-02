---
title: Person 3 - DHCP & Wireless Specialist
tags:
  - role-guide
  - person-3
  - dhcp
  - wireless
  - access-point
  - kampuskita-network
---

# Person 3 — DHCP & Wireless Specialist

> 🧭 **Navigasi Cepat:** [[00 - Team Overview & Roadmap|👥 Overview Tim]] • [[00 - KampusKita Network MOC|🏠 Hub MOC Utama]]  
> *(💡 Tips Obsidian: Tahan tombol **Ctrl** lalu klik link, atau klik langsung pada mode **Reading View**).*

Sebagai **Person 3**, Anda bertanggung jawab atas distribusi IP otomatis menggunakan **3 unit DHCP Server Dedicated** (pada Lab Komputer, Bagian Admin, dan Bagian Umum), serta mengonfigurasi **3 unit Wireless Router (`HomeRouter-PT-AC`)** di Bagian Umum (Tamu, Mahasiswa, Dosen).

---

## 🛠️ Daftar Tugas Spesifik Anda

- [ ] **Tugas 3.1:** Konfigurasi IP statis dan layanan DHCP pada `DHCP-SERVER-LAB-KOMPUTER`.
- [ ] **Tugas 3.2:** Konfigurasi IP statis dan layanan DHCP pada `DHCP-SERVER-ADMIN`.
- [ ] **Tugas 3.3:** Konfigurasi IP statis dan layanan DHCP pada `DHCP-SERVER-UMUM`.
- [ ] **Tugas 3.4:** Setup 3 unit `HomeRouter-PT-AC` (SSID & Keamanan: `WIFI-TAMU` Open, `WIFI-MAHASISWA`, `WIFI-DOSEN`).
- [ ] **Tugas 3.5:** Pasang modul nirkabel `WPC300N` pada semua laptop klien.
- [ ] **Tugas 3.6:** Verifikasi perolehan IP DHCP pada semua PC, Laptop, dan Smartphone.

---

## 📋 Langkah Eksekusi Teknis

### 1. Setup 3 Unit Dedicated DHCP Server

#### A. `DHCP-SERVER-LAB-KOMPUTER` (Zona Orange)
1. **IP Statis (Desktop ➔ IP Configuration):**
   - IP: `192.168.20.2` | Mask: `255.255.255.0` | Gateway: `192.168.20.1` | DNS: `192.168.10.2`
2. **Layanan DHCP (Services ➔ DHCP):**
   - Service: **`ON`** | Pool Name: `serverPool`
   - Default Gateway: `192.168.20.1`
   - DNS Server: `192.168.10.2`
   - Start IP: `192.168.20.10` | Mask: `255.255.255.0` | Max Users: `100`
   - Klik **Save**.

#### B. `DHCP-SERVER-ADMIN` (Zona Kuning)
1. **IP Statis:**
   - IP: `192.168.30.2` | Mask: `255.255.255.0` | Gateway: `192.168.30.1` | DNS: `192.168.10.2`
2. **Layanan DHCP:**
   - Service: **`ON`** | Pool Name: `serverPool`
   - Default Gateway: `192.168.30.1`
   - DNS Server: `192.168.10.2`
   - Start IP: `192.168.30.10` | Mask: `255.255.255.0` | Max Users: `50`
   - Klik **Save**.

#### C. `DHCP-SERVER-UMUM` (Zona Pink)
1. **IP Statis:**
   - IP: `192.168.40.2` | Mask: `255.255.255.0` | Gateway: `192.168.40.1` | DNS: `192.168.10.2`
2. **Layanan DHCP:**
   - Service: **`ON`** | Pool Name: `serverPool`
   - Default Gateway: `192.168.40.1`
   - DNS Server: `192.168.10.2`
   - Start IP: `192.168.40.10` | Mask: `255.255.255.0` | Max Users: `150`
   - Klik **Save**.

---

### 2. Setup 3x `HomeRouter-PT-AC` (Zona Pink)

Hubungkan kabel Copper Straight-Through dari `SW-UMUM` ke port **Ethernet 1** pada masing-masing HomeRouter (Mode Bridge/AP). Pada tab **GUI ➔ Basic Setup**, matikan internal DHCP (`DHCP Server: Disabled`) lalu klik **Save Settings**.

#### 1. HomeRouter `WIFI-TAMU`
- Buka **GUI ➔ Wireless ➔ Basic Wireless Settings**:
  - Network Name (SSID): `WIFI-TAMU` ➔ Klik **Save Settings**.
- Buka sub-menu **Wireless Security**:
  - Security Mode: **`Disabled`** *(Sesuai soal: WIFI TAMU TIDAK MENGGUNAKAN PASSWORD)* ➔ Klik **Save Settings**.

#### 2. HomeRouter `WIFI-MAHASISWA`
- Buka **GUI ➔ Wireless ➔ Basic Wireless Settings**:
  - Network Name (SSID): `WIFI-MAHASISWA` ➔ Klik **Save Settings**.
- Buka sub-menu **Wireless Security**:
  - Security Mode: `WPA2 Personal` | Passphrase: `MahasiswaKampus2026!` ➔ Klik **Save Settings**.

#### 3. HomeRouter `WIFI-DOSEN`
- Buka **GUI ➔ Wireless ➔ Basic Wireless Settings**:
  - Network Name (SSID): `WIFI-DOSEN` ➔ Klik **Save Settings**.
- Buka sub-menu **Wireless Security**:
  - Security Mode: `WPA2 Personal` | Passphrase: `DosenKampus2026!` ➔ Klik **Save Settings**.

---

### 3. Pemasangan Modul Wireless Laptop Klien
Pada setiap Laptop (`LaptopTamu-1`, `LaptopMahasiswa-1 & 2`, `LaptopDosen-1 & 2`):
1. Buka tab **Physical** ➔ Matikan sakelar daya Laptop (**Power OFF**).
2. Tarik keluar modul LAN kabel (`PT-HOST-NM-1CFE`) ke kolom modul sebelah kiri.
3. Tarik modul nirkabel **`WPC300N`** ke slot kosong laptop.
4. Hidupkan kembali sakelar daya Laptop (**Power ON**).
5. Buka tab **Desktop ➔ PC Wireless ➔ Connect**:
   - Pilih SSID yang sesuai dan masukkan password (untuk Mahasiswa/Dosen), lalu klik **Connect**.

---

## 🔍 Checklist Verifikasi
Buka `PC-LAB-1` s.d. `5` dan `PC-ADMIN-1` s.d. `3`, ubah opsi IP ke **DHCP**. Pastikan statusnya `DHCP request successful` dan IP yang didapat berawalan `192.168.20.X` atau `192.168.30.X`.
Jika semua sukses, laporkan kepada **Person 4** untuk memulai pengujian end-to-end!
