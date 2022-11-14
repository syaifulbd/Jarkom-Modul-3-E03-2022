# Jarkom-Modul-3-E03-2022

## Kelompok E03
- Pedro T. Korwa              05111940007003
- Made Rianja Richo Dainino   5025201236
- Syaiful Bahri Dirgantara    5025201203

### No. 1 Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server.

- Membuat topologi seperti yang diminta pada soal, kemudian ubah konfigurasi pada masing-masing node seperti pada yang diminta di soal pula

(foto topologi)

<ul>
  <li>Wise</li>
  
Node Wise sebagai DNS Server: Install bind9
  
(bukti terinstall bind9)

 <li>Westalis</li>
  
Node Westalis sebagai DHCP Server: Install isc-dhcp-server
  
(bukti terinstall dhcp server)

 <li>Berlint</li> 
  
</ul>
  
Node Berlint sebagai Proxy Server: Install squid9

(bukti terinstall squid9)

### No. 2 Ostania sebagai DHCP Relay.
  
Ubah konfigurasi Ostania untuk menjadi DHCP Relay : Install isc-dhcp-relay

(bukti terinstall dhcp relay)
  
### No. 3 Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

Tambahkan konfigurasi di file /etc/dhcp/dhcpd.conf di Westalis sebagai berikut :

### No. 4 Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85
  
Tambahkan konfigurasi di file /etc/dhcp/dhcpd.conf di Westalis sebagai berikut :
