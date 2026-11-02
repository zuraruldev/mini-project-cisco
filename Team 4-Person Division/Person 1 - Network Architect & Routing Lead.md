---
title: Person 1 - Network Architect & Routing Lead
tags:
  - role-guide
  - person-1
  - router
  - cisco-cli
  - acl
  - kampuskita-network
---

# Person 1 — Network Architect & Routing Lead

> 🧭 **Navigasi Cepat:** [[00 - Team Overview & Roadmap|👥 Overview Tim]] • [[00 - KampusKita Network MOC|🏠 Hub MOC Utama]]  
> *(💡 Tips Obsidian: Tahan tombol **Ctrl** lalu klik link, atau klik langsung pada mode **Reading View**).*

Sebagai **Person 1**, Anda adalah arsitek jaringan utama yang bertanggung jawab atas struktur fisik kanvas Cisco Packet Tracer, konfigurasi gerbang gateway **ROUTER UTAMA**, dan aturan keamanan **Extended ACL**.

---

## 🛠️ Daftar Tugas Spesifik Anda

- [ ] **Tugas 1.1:** Meletakkan perangkat utama di kanvas Packet Tracer sesuai **Gambar 1**.
- [ ] **Tugas 1.2:** Menambahkan modul port interface pada `ROUTER UTAMA` (`Router-PT`).
- [ ] **Tugas 1.3:** Menghubungkan seluruh kabel Copper Straight-Through antar Router dan 4 Switch.
- [ ] **Tugas 1.4:** Memasukkan konfigurasi CLI Router untuk 4 subnet gateway.
- [ ] **Tugas 1.5:** Mengonfigurasi Extended ACL 101 untuk memblokir Subnet Tamu ke Mail Server.
- [ ] **Tugas 1.6:** Menyiapkan jawaban teknis saat sesi tanya jawab dengan dosen pengampu.

---

## 📋 Langkah Eksekusi Teknis

### 1. Persiapan Port Hardware `Router-PT`
1. Tarik **1x Router-PT** ke kanvas Packet Tracer, beri display name `ROUTER UTAMA`.
2. Klik Router ➔ Tab **Physical**:
   - Matikan sakelar daya router (**Power Switch OFF**).
   - Cari modul **`PT-ROUTER-NM-1CFE`** (atau `PT-ROUTER-NM-1FFE`) di kolom kiri.
   - Tarik modul tersebut ke slot kosong pada router hingga router memiliki total minimal 4 port FastEthernet (`Fa0/0`, `Fa1/0`, `Fa2/0`, `Fa3/0`).
   - Hidupkan kembali sakelar daya router (**Power Switch ON**).

### 2. Penataan 4 Switch Distribusi
Tarik 4 unit Switch **`2960-24TT`** berjejer di bawah router:
- `SW-SERVER` (Port `Gig0/1` terhubung ke Router `Fa0/0`)
- `SW-LAB-KOMPUTER` (Port `Gig0/1` terhubung ke Router `Fa1/0`)
- `SW-ADMIN` (Port `Gig0/1` terhubung ke Router `Fa2/0`)
- `SW-UMUM` (Port `Gig0/1` terhubung ke Router `Fa3/0`)

### 3. Skrip CLI Konfigurasi Router (Salin & Tempel)
Buka tab **CLI** pada **ROUTER UTAMA**:

```cisco
enable
configure terminal
hostname ROUTER_UTAMA
no ip domain-lookup

! Gateway Subnet 1 (Zona Server Farm - 192.168.10.0/24)
interface FastEthernet0/0
 description Gateway ke SW-SERVER
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

! Gateway Subnet 2 (Zona Lab Komputer - 192.168.20.0/24)
interface FastEthernet1/0
 description Gateway ke SW-LAB-KOMPUTER
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit

! Gateway Subnet 3 (Zona Bagian Admin - 192.168.30.0/24)
interface FastEthernet2/0
 description Gateway ke SW-ADMIN
 ip address 192.168.30.1 255.255.255.0
 no shutdown
 exit

! Gateway Subnet 4 (Zona Bagian Umum - 192.168.40.0/24)
interface FastEthernet3/0
 description Gateway ke SW-UMUM
 ip address 192.168.40.1 255.255.255.0
 no shutdown
 exit
```

### 4. Konfigurasi Extended ACL 101 (Keamanan WiFi Tamu)
Setelah Person 2 dan 3 selesai memasang perangkat, terapkan aturan ACL pada router:

```cisco
! Blokir subnet tamu (192.168.40.0/24) ke Mail Server (192.168.10.5)
access-list 101 deny ip 192.168.40.0 0.0.0.255 host 192.168.10.5
access-list 101 permit ip any any

! Terapkan ke interface Subnet Umum (Inbound)
interface FastEthernet3/0
 ip access-group 101 in
 exit

end
write memory
```

---

## 🔍 Perintah Verifikasi untuk Demonstrasi ke Dosen

```cisco
ROUTER_UTAMA# show ip interface brief   ! Buktikan 4 interface statusnya 'Up/Up'
ROUTER_UTAMA# show ip route             ! Tunjukkan 4 rute directly connected (C)
ROUTER_UTAMA# show access-lists         ! Tunjukkan counter paket match yang diblokir
```
