---
title: DHCP & Wireless Access Points - KampusKita University
tags:
  - dhcp
  - wireless
  - wifi
  - access-point
  - networking
  - kampuskita-network
hub: "[[00 - KampusKita Network MOC]]"
related:
  - "[[01 - Network Architecture & Topology]]"
  - "[[02 - IP Addressing & Subnetting Plan]]"
  - "[[06 - End Device Verification & Email Testing]]"
---

# DHCP & Wireless Access Points — KampusKita University

> **MOC:** [[00 - KampusKita Network MOC]] | **Topologi Gambar 1:** [[01 - Network Architecture & Topology]] | **IP Plan:** [[02 - IP Addressing & Subnetting Plan]]

Sesuai ilustrasi **Gambar 1**, terdapat **3 buah Dedicated DHCP Server** fisik pada masing-masing switch cabang (`SW-LAB-KOMPUTER`, `SW-ADMIN`, dan `SW-UMUM`), serta **3 unit Wireless Router** (`HomeRouter-PT-AC`) di Bagian Umum untuk memancarkan jaringan nirkabel bagi Tamu, Mahasiswa, dan Dosen.

---

## 1. Konfigurasi 3 Server DHCP Dedicated

### A. `DHCP-SERVER-LAB-KOMPUTER` (Zona Orange)
- **Koneksi:** Kabel Copper Straight-Through ke `SW-LAB-KOMPUTER`.
- **IP Konfigurasi Statis:**
  - IP Address: `192.168.20.2`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `192.168.20.1`
  - DNS Server: `192.168.10.2`
- **Tab Services ➔ DHCP:**
  - Service: `ON`
  - Pool Name: `serverPool`
  - Default Gateway: `192.168.20.1`
  - DNS Server: `192.168.10.2`
  - Start IP Address: `192.168.20.10`
  - Subnet Mask: `255.255.255.0`
  - Maximum number of Users: `100`
  - Klik **Save**.
- **Klien yang dilayani:** `PC-LAB-1` s.d. `PC-LAB-5` (Pilih opsi DHCP pada Desktop PC).

---

### B. `DHCP-SERVER-ADMIN` (Zona Kuning)
- **Koneksi:** Kabel Copper Straight-Through ke `SW-ADMIN`.
- **IP Konfigurasi Statis:**
  - IP Address: `192.168.30.2`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `192.168.30.1`
  - DNS Server: `192.168.10.2`
- **Tab Services ➔ DHCP:**
  - Service: `ON`
  - Pool Name: `serverPool`
  - Default Gateway: `192.168.30.1`
  - DNS Server: `192.168.10.2`
  - Start IP Address: `192.168.30.10`
  - Subnet Mask: `255.255.255.0`
  - Maximum number of Users: `50`
  - Klik **Save**.
- **Klien yang dilayani:** `PC-ADMIN-1`, `PC-ADMIN-2`, `PC-ADMIN-3`.

---

### C. `DHCP-SERVER-UMUM` (Zona Pink)
- **Koneksi:** Kabel Copper Straight-Through ke `SW-UMUM`.
- **IP Konfigurasi Statis:**
  - IP Address: `192.168.40.2`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `192.168.40.1`
  - DNS Server: `192.168.10.2`
- **Tab Services ➔ DHCP:**
  - Service: `ON`
  - Pool Name: `serverPool`
  - Default Gateway: `192.168.40.1`
  - DNS Server: `192.168.10.2`
  - Start IP Address: `192.168.40.10`
  - Subnet Mask: `255.255.255.0`
  - Maximum number of Users: `150`
  - Klik **Save**.

---

## 2. Konfigurasi 3 Unit Wireless Router (`HomeRouter-PT-AC`)

Pada Gambar 1, di Zona Bagian Umum terdapat 3 perangkat `HomeRouter-PT-AC`:
1. `WIFI-TAMU`
2. `WIFI-MAHASISWA`
3. `WIFI-DOSEN`

### Opsi A (Sangat Disarankan): Setup Mode Bridge / Access Point
Hubungkan kabel Copper Straight-Through dari `SW-UMUM` ke port **Ethernet 1** (port LAN) pada setiap HomeRouter (bukan port Internet).
Pada menu tab **GUI ➔ Basic Setup**:
- Nonaktifkan DHCP Server internal (`DHCP Server: Disabled`), klik **Save Settings**.
- Dengan cara ini, semua klien nirkabel akan langsung dilayani oleh `DHCP-SERVER-UMUM` dalam subnet `192.168.40.0/24`.

### Pengaturan Nirkabel (Wireless Settings):

#### 1. HomeRouter `WIFI-TAMU`
- Buka tab **GUI ➔ Wireless ➔ Basic Wireless Settings**:
  - Network Name (SSID) 2.4 GHz / 5 GHz: `WIFI-TAMU`
- Buka sub-menu **Wireless Security**:
  - Security Mode: **`Disabled`** *(Sesuai ketentuan soal: WIFI TAMU TIDAK MENGGUNAKAN PASSWORD)*
- Klien:
  - `LaptopTamu-1`: Pasang modul `WPC300N`, buka *PC Wireless ➔ Connect* ke `WIFI-TAMU`.
  - `SmartphoneTamu-1`: Buka *Config ➔ Wireless0* ➔ Masukkan SSID `WIFI-TAMU` (Auth: Disabled).

#### 2. HomeRouter `WIFI-MAHASISWA`
- Buka tab **GUI ➔ Wireless ➔ Basic Wireless Settings**:
  - Network Name (SSID): `WIFI-MAHASISWA`
- Buka sub-menu **Wireless Security**:
  - Security Mode: `WPA2 Personal`
  - Passphrase: `MahasiswaKampus2026!`
- Klien:
  - `LaptopMahasiswa-1` & `LaptopMahasiswa-2`: Connect ke SSID `WIFI-MAHASISWA` dengan password di atas.

#### 3. HomeRouter `WIFI-DOSEN`
- Buka tab **GUI ➔ Wireless ➔ Basic Wireless Settings**:
  - Network Name (SSID): `WIFI-DOSEN`
- Buka sub-menu **Wireless Security**:
  - Security Mode: `WPA2 Personal`
  - Passphrase: `DosenKampus2026!`
- Klien:
  - `LaptopDosen-1` & `LaptopDosen-2`: Connect ke SSID `WIFI-DOSEN` dengan password di atas.

---

## 🔗 Referensi Silang
- Pengujian Klien & Email: [[06 - End Device Verification & Email Testing]]
- Aturan Keamanan ACL: [[03 - Core Router Configuration & ACL]]
- Topologi Gambar 1: [[01 - Network Architecture & Topology]]
