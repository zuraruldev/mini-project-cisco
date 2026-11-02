---
title: Person 4 - QA Testing & Report Lead
tags:
  - role-guide
  - person-4
  - testing
  - edlink
  - report
  - kampuskita-network
---

# Person 4 — QA Testing & Report Lead

> 🧭 **Navigasi Cepat:** [[00 - Team Overview & Roadmap|👥 Overview Tim]] • [[00 - KampusKita Network MOC|🏠 Hub MOC Utama]]  
> *(💡 Tips Obsidian: Tahan tombol **Ctrl** lalu klik link, atau klik langsung pada mode **Reading View**).*

Sebagai **Person 4**, Anda memegang kendali kualitas akhir (*Quality Assurance*), pengujian live-test komunikasi data, pembuktian aturan ACL pemblokiran tamu, serta penyusunan **Laporan Resmi PDF** dan file `.pkt` untuk submit ke **EdLink**. Kualitas demonstrasi dan laporan menentukan kelompok Anda memperoleh **Nilai Akhir A**!

---

## 🛠️ Daftar Tugas Spesifik Anda

- [ ] **Tugas 4.1:** Melakukan pengujian matriks fungsional U-01 s.d. U-10.
- [ ] **Tugas 4.2:** Menguji dan mendokumentasikan live-test kirim dan terima email antar perangkat.
- [ ] **Tugas 4.3:** Membuktikan bahwa `SmartphoneTamu-1` diblokir ke `MAIL-SERVER` namun tetap bisa browsing Web.
- [ ] **Tugas 4.4:** Mengambil screenshot bukti pengujian dengan resolusi jelas.
- [ ] **Tugas 4.5:** Menyusun Laporan Akhir (PDF) sesuai format akademik.
- [ ] **Tugas 4.6:** Mengompres file `.pdf` dan `.pkt` menjadi `.zip` untuk diunggah ke EdLink.

---

## 📋 Prosedur Pengujian Matriks (U-01 s.d. U-10)

| ID Uji | Perangkat Uji | Target | Langkah Pengujian | Kriteria Lolos (Ambil Screenshot!) |
| :--- | :--- | :--- | :--- | :--- |
| **U-01** | `PC-LAB-1` | DHCP Lab | Buka IP Config ➔ Pilih DHCP | Status `DHCP request successful`, IP `192.168.20.X` |
| **U-02** | `PC-ADMIN-1` | DHCP Admin | Buka IP Config ➔ Pilih DHCP | Status `DHCP request successful`, IP `192.168.30.X` |
| **U-03** | `SmartphoneTamu-1` | DHCP Umum | Connect WiFi ➔ Cek IP | Menerima IP `192.168.40.X`, DNS `192.168.10.2` |
| **U-04** | `PC-LAB-3` | Web Portal | Web Browser ➔ `kampuskita.ac.id` | Halaman web KampusKita University terbuka |
| **U-05** | `PC-ADMIN-2` | FTP Server | CMD ➔ `ftp ftp.kampuskita.ac.id` | Berhasil login dengan user `admin` |
| **U-06** | `PC-ADMIN-1` | Email Send | Email ➔ Compose ke `dosen@...` | Muncul status `Send Success` |
| **U-07** | `LaptopDosen-1` | Email Receive | Email ➔ Klik tombol Receive | Email admin masuk dan tombol Reply bekerja |
| **U-08** | `LaptopMahasiswa-1` | `PC-LAB-5` | CMD ➔ `ping 192.168.20.14` | Ping Reply `bytes=32 time<1ms TTL=127` |
| **U-09** | `SmartphoneTamu-1` | Web Portal | Web Browser ➔ `kampuskita.ac.id` | **BERHASIL TERBUKA** (Web tamu diizinkan) |
| **U-10** | `SmartphoneTamu-1` | `MAIL-SERVER` | CMD ➔ `ping 192.168.10.5` | **GAGAL / Destination host unreachable** *(ACL Aktif!)* |

---

## 📸 Panduan Screenshot Kunci untuk Laporan PDF

Pastikan 5 screenshot wajib ini dimasukkan ke dalam laporan:
1. **Screenshot 1:** Kanvas keseluruhan topologi Packet Tracer (menunjukkan 4 zona lingkaran warna aktif dengan indikator link hijau).
2. **Screenshot 2:** Konfigurasi DNS Server (tabel 4 A-Record).
3. **Screenshot 3:** Halaman Web Browser yang berhasil membuka `kampuskita.ac.id` dari PC Lab.
4. **Screenshot 4:** Kotak masuk Email pada `LaptopDosen-1` yang menerima pesan dari `PC-ADMIN-1`.
5. **Screenshot 5:** Layar Command Prompt `SmartphoneTamu-1` yang menampilkan `Destination host unreachable` saat ping ke `192.168.10.5`, disandingkan dengan browser yang tetap bisa membuka web.

---

## 📦 Prosedur Pengemasan & Pengunggahan EdLink

1. Simpan file Packet Tracer Anda:
   - Nama file: `Topologi_KampusKita_KelompokX.pkt`
2. Ekspor Laporan Resmi ke format PDF:
   - Nama file: `Laporan_Mini_Project_KampusKita_KelompokX.pdf`
3. Kompresi kedua file tersebut menjadi satu file `.zip`:
   - Nama file zip: `MiniProject_KeterampilanKomputer_KelompokX.zip`
4. Unggah berkas zip ke portal **EdLink** mata kuliah Keterampilan Komputer sebelum **Pertemuan 15 (Maksimal 31 Desember 2026)**.
