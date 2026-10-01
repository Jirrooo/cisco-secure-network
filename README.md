# Perancangan dan Pengerasan Keamanan Infrastruktur Jaringan Enterprise

## Ringkasan Proyek
Proyek ini berfokus pada perancangan serta pengalokasian keamanan pada infrastruktur jaringan enterprise skala kecil hingga menengah menggunakan Cisco Packet Tracer. Tujuan utama dari proyek ini adalah menerapkan segmentasi jaringan yang kuat, mengamankan akses pada Layer 2 dan Layer 3, serta membatasi lalu lintas data yang tidak diizinkan antar-VLAN berdasarkan prinsip pertahanan berlapis (defense-in-depth).

---

## Fitur Keamanan Utamanya

### 1. Keamanan Layer 2 (Access Layer)
* Port Security: Diterapkan pada port akses yang terhubung langsung ke perangkat pengguna (Fa0/1) dengan batas maksimal 1 alamat MAC menggunakan metode sticky learning. Mode pelanggaran diset ke shutdown untuk mematikan port secara otomatis apabila ada perangkat tidak dikenal yang terhubung.
* Pengubahan Native VLAN: Mengubah Native VLAN bawaan (VLAN 1) menjadi VLAN khusus yang tidak terpakai (VLAN 99) pada jalur trunk untuk mencegah potensi serangan VLAN Hopping.

### 2. Segmentasi dan Kontrol Akses Layer 3
* Inter-VLAN Routing: Mengkonfigurasi Router-on-a-Stick pada sub-interface Gi0/0.10 dan Gi0/0.20 untuk memisahkan jaringan antar-departemen (HR dan Guest).
* Extended Access Control List (ACL): Menerapkan aturan penyaringan lalu lintas data masuk pada interface Guest (BLOCK-GUEST-TO-FINANCE) untuk memblokir seluruh akses dari subnet Guest (192.168.20.0/24) menuju Server Keuangan (10.10.10.2/32), sembari tetap mempertahankan hak akses penuh bagi staf HR (192.168.10.0/24).

### 3. Keamanan Akses Manajemen
* Mematikan penggunaan protokol manajemen jarak jauh yang tidak terenkripsi seperti Telnet.
* Mengaktifkan protokol SSH v2 menggunakan kunci enkripsi RSA 1024-bit dan autentikasi pengguna lokal untuk mengamankan sesi manajemen perangkat CLI secara jarak jauh.

---

## Skema Pengalamatan Jaringan

| Perangkat | Interface / VLAN | Alamat IP | Subnet Mask | Deskripsi |
| :--- | :--- | :--- | :--- | :--- |
| RT-CORE-01 | Gi0/0.10 | 192.168.10.1 | 255.255.255.0 | Gateway VLAN 10 |
| RT-CORE-01 | Gi0/0.20 | 192.168.20.1 | 255.255.255.0 | Gateway VLAN 20 |
| RT-CORE-01 | Gi0/1 | 10.10.10.1 | 255.255.255.0 | Gateway Server Keuangan |
| PC-HR | VLAN 10 | 192.168.10.10 | 255.255.255.0 | Perangkat Staf HR |
| PC-Guest | VLAN 20 | 192.168.20.10 | 255.255.255.0 | Perangkat Tamu |
| Server-Finance | Subnet Khusus | 10.10.10.2 | 255.255.255.0 | Server Internal Keuangan |

---

## Hasil Pengujian dan Verifikasi

### 1. Pengujian Penyaringan Lalu Lintas ACL
* Akses Terizin (PC-HR ke Server): Pengujian konektivitas ICMP (ping) berhasil dilakukan dan memiliki respon yang stabil.
* Akses Terblokir (PC-Guest ke Server): Paket ICMP berhasil ditolak oleh aturan ACL pada router, menghasilkan pesan "Destination host unreachable".

### 2. Pengujian Port Security Terhadap Perangkat Asing
* Saat perangkat yang tidak terdaftar (PC-Attacker) dihubungkan ke port Fa0/1, switch secara otomatis mendeteksi adanya pelanggaran keamanan dan mengubah status port menjadi err-disable atau shutdown (ditandai dengan indikator merah pada port).

### 3. Pengujian Akses Jarak Jauh SSH
* Sesi remote administration terenkripsi dari PC-HR ke Gateway Router (192.168.10.1) menggunakan SSH v2 berhasil terhubung dan berjalan dengan normal.

---

## Berkas dalam Repositori
* topology_design.pkt: Berkas simulasi topologi jaringan Cisco Packet Tracer.
* docs/: Folder berisi dokumentasi tangkapan layar hasil verifikasi dan diagram topologi.
