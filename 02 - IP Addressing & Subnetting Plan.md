---
title: IP Addressing & Subnetting Plan - KampusKita University
tags:
  - ip-addressing
  - subnetting
  - networking
  - kampuskita-network
---

# IP Addressing & Subnetting Plan — KampusKita University

> 🧭 **Navigasi Cepat:** [[00 - KampusKita Network MOC|🏠 Hub MOC]] • [[01 - Network Architecture & Topology|📐 Topologi Gambar 1]] • [[03 - Core Router Configuration & ACL|🛡️ Router & ACL]]  
> *(💡 Tips Obsidian: Tahan tombol **Ctrl** lalu klik link, atau klik langsung pada mode **Reading View**).*

Skema pengalamatan IP di KampusKita University membagi jaringan kelas C privat `192.168.0.0/16` menjadi 4 subnet `/24` terisolasi yang diatur oleh **ROUTER UTAMA**.

---

## 1. Tabel Rangkuman Subnetting 4 Zona

| Zona (Sesuai Gambar 1) | Network Address | Subnet Mask | Interface Router | IP Gateway | Tipe IP Klien | Keterangan |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Zona Server** *(Cyan)* | `192.168.10.0/24` | `255.255.255.0` | `Fa0/0` | `192.168.10.1` | Statis | Layanan Inti (DNS, Web, FTP, Mail) |
| **Zona Lab** *(Orange)* | `192.168.20.0/24` | `255.255.255.0` | `Fa1/0` | `192.168.20.1` | DHCP | 5 Unit PC Praktikum |
| **Zona Admin** *(Kuning)* | `192.168.30.0/24` | `255.255.255.0` | `Fa2/0` | `192.168.30.1` | DHCP | 3 Unit PC Pegawai |
| **Zona Umum** *(Pink)* | `192.168.40.0/24` | `255.255.255.0` | `Fa3/0` | `192.168.40.1` | DHCP | Hotspot (3 HomeRouter & Klien) |

---

## 2. Alokasi Detail Perangkat Berdasarkan Gambar 1

### A. Zona Server Farm (`192.168.10.0/24`) — Pengaturan Statis
- `ROUTER UTAMA (Fa0/0)`: `192.168.10.1`
- `DNS-SERVER`: `192.168.10.2` (Domain: `kampuskita.ac.id`)
- `WEB-SERVER`: `192.168.10.3` (HTTP/HTTPS portal siakad)
- `FTP-SERVER`: `192.168.10.4` (Cloud storage repositori)
- `MAIL-SERVER`: `192.168.10.5` (SMTP/POP3 mail service)
- *DNS Server untuk semua server disetel ke:* `192.168.10.2`

### B. Zona Lab Komputer (`192.168.20.0/24`) — DHCP Server Dedicated
- `ROUTER UTAMA (Fa1/0)`: `192.168.20.1`
- `DHCP-SERVER-LAB-KOMPUTER` (Statis): `192.168.20.2`
- **Dynamic DHCP Pool:**
  - Range Alamat: `192.168.20.10` s.d. `192.168.20.254`
  - Gateway disebarkan: `192.168.20.1`
  - DNS Server disebarkan: `192.168.10.2`
- **End Devices Terhubung:**
  - `PC-LAB-1`, `PC-LAB-2`, `PC-LAB-3`, `PC-LAB-4`, `PC-LAB-5` (Otomatis dapat IP via DHCP).

### C. Zona Bagian Admin (`192.168.30.0/24`) — DHCP Server Dedicated
- `ROUTER UTAMA (Fa2/0)`: `192.168.30.1`
- `DHCP-SERVER-ADMIN` (Statis): `192.168.30.2`
- **Dynamic DHCP Pool:**
  - Range Alamat: `192.168.30.10` s.d. `192.168.30.254`
  - Gateway disebarkan: `192.168.30.1`
  - DNS Server disebarkan: `192.168.10.2`
- **End Devices Terhubung:**
  - `PC-ADMIN-1`, `PC-ADMIN-2`, `PC-ADMIN-3` (Otomatis dapat IP via DHCP).

### D. Zona Bagian Umum / Hotspot (`192.168.40.0/24`)
- `ROUTER UTAMA (Fa3/0)`: `192.168.40.1`
- `DHCP-SERVER-UMUM` (Statis): `192.168.40.2`
- **Dynamic DHCP Pool:**
  - Range Alamat: `192.168.40.10` s.d. `192.168.40.254`
  - Gateway disebarkan: `192.168.40.1`
  - DNS Server disebarkan: `192.168.10.2`

#### Skema Penghubung 3x `HomeRouter-PT-AC`:
Ada 2 pilihan implementasi di Packet Tracer:

1. **Metode Rekomendasi (Bridge / Access Point Mode):**
   - Kabel dari `SW-UMUM` dicolokkan ke port **Ethernet 1** (port LAN) pada setiap HomeRouter (bukan port Internet).
   - Nonaktifkan DHCP Server internal di dalam menu GUI HomeRouter.
   - Hasil: Semua perangkat klien nirkabel (`LaptopTamu-1`, `SmartphoneTamu-1`, `LaptopMahasiswa`, `LaptopDosen`) langsung mendapatkan IP `192.168.40.X` dari `DHCP-SERVER-UMUM`.
2. **Metode WAN / Internet Port (NAT Mode):**
   - Kabel dari `SW-UMUM` dicolokkan ke port **Internet** pada HomeRouter.
   - Internet Port disetel ke DHCP (otomatis dapat IP `192.168.40.X` dari `DHCP-SERVER-UMUM`).
   - Setiap HomeRouter memancarkan SSID dan membagikan subnet internalnya masing-masing.

*(Metode 1 lebih mudah diverifikasi saat pengujian ACL pemblokiran Mail Server untuk subnet Tamu).*

---

## 🔗 Referensi Silang
- Konfigurasi Core Router: [[03 - Core Router Configuration & ACL]]
- Setup Layanan Server: [[04 - Server Farm Services]]
- Setup 3 HomeRouter & DHCP: [[05 - DHCP & Wireless Access Points]]
