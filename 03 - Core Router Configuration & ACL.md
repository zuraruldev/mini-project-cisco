---
title: Core Router Configuration & ACL - KampusKita University
tags:
  - router
  - acl
  - security
  - kampuskita-network
hub: "[[00 - KampusKita Network MOC]]"
related:
  - "[[01 - Network Architecture & Topology]]"
  - "[[02 - IP Addressing & Subnetting Plan]]"
  - "[[06 - End Device Verification & Email Testing]]"
---

# Core Router Configuration & ACL — KampusKita University

> **MOC:** [[00 - KampusKita Network MOC]] | **Topologi Gambar 1:** [[01 - Network Architecture & Topology]] | **Alamat IP:** [[02 - IP Addressing & Subnetting Plan]]

Pada **Gambar 1**, router pusat dinamai **ROUTER UTAMA** (model `Router-PT`). Router ini berfungsi sebagai default gateway untuk 4 zona dan menjadi gerbang keamanan utama untuk menegakkan aturan pemblokiran akses tamu ke Mail Server menggunakan **Extended Access Control List (ACL)**.

---

## 1. Konfigurasi 4 Interface Gateway (CLI Router-PT)

> **Catatan Port Hardware pada Router-PT:**
> Jika menggunakan model `Router-PT`, pastikan router dalam keadaan **Power OFF**, lalu pasang modul interface FastEthernet tambahan (misalnya modul `PT-ROUTER-NM-1CFE` atau `PT-ROUTER-NM-1FFE`) pada slot yang tersedia agar router memiliki minimal 4 port FastEthernet (`Fa0/0`, `Fa1/0`, `Fa2/0`, `Fa3/0`), kemudian hidupkan kembali (**Power ON**).

Masuk ke tab **CLI** pada **ROUTER UTAMA**:

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname ROUTER_UTAMA
ROUTER_UTAMA(config)# no ip domain-lookup

! 1. Interface ke SW-SERVER (Zona Biru Muda - Server Farm)
ROUTER_UTAMA(config)# interface FastEthernet0/0
ROUTER_UTAMA(config-if)# description Gateway ke SW-SERVER
ROUTER_UTAMA(config-if)# ip address 192.168.10.1 255.255.255.0
ROUTER_UTAMA(config-if)# no shutdown
ROUTER_UTAMA(config-if)# exit

! 2. Interface ke SW-LAB-KOMPUTER (Zona Orange - Lab Komputer)
ROUTER_UTAMA(config)# interface FastEthernet1/0
ROUTER_UTAMA(config-if)# description Gateway ke SW-LAB-KOMPUTER
ROUTER_UTAMA(config-if)# ip address 192.168.20.1 255.255.255.0
ROUTER_UTAMA(config-if)# no shutdown
ROUTER_UTAMA(config-if)# exit

! 3. Interface ke SW-ADMIN (Zona Kuning - Bagian Admin)
ROUTER_UTAMA(config)# interface FastEthernet2/0
ROUTER_UTAMA(config-if)# description Gateway ke SW-ADMIN
ROUTER_UTAMA(config-if)# ip address 192.168.30.1 255.255.255.0
ROUTER_UTAMA(config-if)# no shutdown
ROUTER_UTAMA(config-if)# exit

! 4. Interface ke SW-UMUM (Zona Pink - Bagian Umum & Tamu)
ROUTER_UTAMA(config)# interface FastEthernet3/0
ROUTER_UTAMA(config-if)# description Gateway ke SW-UMUM
ROUTER_UTAMA(config-if)# ip address 192.168.40.1 255.255.255.0
ROUTER_UTAMA(config-if)# no shutdown
ROUTER_UTAMA(config-if)# exit
```

*(Jika Anda menggunakan Router tipe 2911 atau 2811, ganti nama port `FastEthernet` dengan `GigabitEthernet`).*

---

## 2. Implementasi Extended ACL Pemblokiran Mail Server

### Ketentuan Soal Mini Project:
> *"Untuk yang terhubung dengan jaringan WIFI TAMU, tidak diperkenankan untuk mengakses Mail Server."*

### Logika Aturan:
- **Trafik Asal:** Seluruh klien di Subnet Umum/Tamu `192.168.40.0/24` (Wildcard mask: `0.0.0.255`).
- **Trafik Tujuan Terlarang:** Host `MAIL-SERVER` (`192.168.10.5`).
- **Aksi:** `deny ip` (blokir akses SMTP, POP3, ICMP Ping, dan komunikasi langsung ke IP tersebut).
- **Trafik Lain:** `permit ip any any` (tamu tetap diizinkan membuka Web Server `192.168.10.3`, DNS `192.168.10.2`, dan FTP `192.168.10.4`).

```cisco
! Membuat Extended ACL nomor 101
ROUTER_UTAMA(config)# access-list 101 deny ip 192.168.40.0 0.0.0.255 host 192.168.10.5
ROUTER_UTAMA(config)# access-list 101 permit ip any any

! Menerapkan ACL pada interface FastEthernet3/0 (Inbound dari Subnet Umum)
ROUTER_UTAMA(config)# interface FastEthernet3/0
ROUTER_UTAMA(config-if)# ip access-group 101 in
ROUTER_UTAMA(config-if)# exit

! Simpan konfigurasi ke NVRAM
ROUTER_UTAMA(config)# end
ROUTER_UTAMA# write memory
```

---

## 3. Perintah Diagnostik & Verifikasi di Router CLI

Gunakan perintah ini saat mempresentasikan hasil topologi di depan dosen pengampu:

```cisco
! Melihat status seluruh port router (pastikan semua Up dan IP benar)
ROUTER_UTAMA# show ip interface brief

! Melihat tabel routing (harus ada 4 network directly connected C 192.168.x.0/24)
ROUTER_UTAMA# show ip route

! Melihat aturan ACL dan jumlah paket yang berhasil diblokir (hit counter)
ROUTER_UTAMA# show access-lists
```

Output `show access-lists` akan menampilkan:
```text
Extended IP access list 101
    10 deny ip 192.168.40.0 0.0.0.255 host 192.168.10.5 (xx matches)
    20 permit ip any any (xx matches)
```
Setiap kali `SmartphoneTamu-1` atau `LaptopTamu-1` mencoba melakukan ping atau koneksi ke `MAIL-SERVER`, hit counter pada baris `10 deny` akan bertambah!

---

## 🔗 Referensi Silang
- Pengujian End Device: [[06 - End Device Verification & Email Testing]]
- Konfigurasi Mail Server: [[04 - Server Farm Services]]
- Desain Topologi Gambar 1: [[01 - Network Architecture & Topology]]
