# Jarkom-Modul-3-E03-2022

## Kelompok E03
- Pedro T. Korwa              05111940007003
- Made Rianja Richo Dainino   5025201236
- Syaiful Bahri Dirgantara    5025201203

### No. 1 Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server.

- Membuat topologi seperti yang diminta pada soal, kemudian ubah konfigurasi pada masing-masing node seperti pada yang diminta di soal pula

![image](https://user-images.githubusercontent.com/33245436/201689588-3a6c7b1e-b84f-4482-aa60-c3ce5cb61141.png)

<ul>
  <li>Wise</li>
  
Node Wise sebagai DNS Server: Install bind9
  
![image](https://user-images.githubusercontent.com/33245436/201691078-8aa715bc-bfe5-4536-b8e1-2dfb9470b25b.png)

 <li>Westalis</li>
  
Node Westalis sebagai DHCP Server: Install isc-dhcp-server
  
![image](https://user-images.githubusercontent.com/33245436/201689960-d408d5ab-cb0a-4138-ac38-a31d71b38607.png)

 <li>Berlint</li> 
  
</ul>
  
Node Berlint sebagai Proxy Server: Install squid3

![image](https://user-images.githubusercontent.com/33245436/201690434-d5115aa2-8db8-44f3-88ca-4ed420ca0e2c.png)

### No. 2 Ostania sebagai DHCP Relay.
  
Ubah konfigurasi Ostania untuk menjadi DHCP Relay : Install isc-dhcp-relay

![image](https://user-images.githubusercontent.com/33245436/201690202-766e91e6-a8b1-4a9e-919d-ab7bccf4c611.png)
  
### No. 3 Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

Tambahkan konfigurasi di file /etc/dhcp/dhcpd.conf di Westalis sebagai berikut :
![image](https://user-images.githubusercontent.com/33245436/201690948-1376a1f0-626f-48b3-be68-b5c5ba9ed15e.png)

### No. 4 Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85
  
Tambahkan konfigurasi di file /etc/dhcp/dhcpd.conf di Westalis sebagai berikut :
![image](https://user-images.githubusercontent.com/33245436/201690869-bea796e7-9a08-4e60-97ca-e9e6e53896c7.png)

### 5. Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

Jawaban : Pada konfigurasi dhcp pada node Westalis (```/etc/dhcp/dhcpd.conf```) untuk switch 1 dan 3 ditambahkan ```option domain-name-servers 10.23.2.2;```

### 6. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

Jawaban : Pada konfigurasi dhcp pada node Westalis (```/etc/dhcp/dhcpd.conf```) untuk switch 1 ditambahkan 
```
default-lease-time 300;
max-lease-time 6900;
```

dan pada switch 3 ditambahkan
```
default-lease-time 600;
max-lease-time 6900;
```

![image](https://user-images.githubusercontent.com/33245436/201691246-6b84433f-e3ab-4147-a183-e5cce145af1a.png)

### 7. Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13.

Jawaban : Pada konfigurasi dhcp pada node Westalis (```/etc/dhcp/dhcpd.conf```) ditambahkan 
```
host Eden {
    hardware ethernet (hwaddress milik Eden);
    fixed-address 10.23.3.13;
}
```
![image](https://user-images.githubusercontent.com/33245436/201691300-226866a6-fa7c-4775-8904-23853a08ffe7.png)

Lalu pada node Eden kita mengedit ```/etc/network/interfaces``` menjadi sebagai berikut:

```
auto eth0
    iface eth0 inet dhcp
    hwaddress ether (hwaddress milik Eden)
```
![image](https://user-images.githubusercontent.com/33245436/201691509-4edc84fc-9370-477a-9ee9-e15280ea2e92.png)


## Soal Proxy
### SSS, Garden, dan Eden digunakan sebagai client Proxy agar pertukaran informasi dapat terjamin keamanannya, juga untuk mencegah kebocoran data. Pada Proxy Server di Berlint, Loid berencana untuk mengatur bagaimana Client dapat mengakses internet. Artinya setiap client harus menggunakan Berlint sebagai HTTP & HTTPS proxy. Adapun kriteria pengaturannya adalah sebagai berikut:
Setup proxy menggunakan port 8080 karena pada soal tidak ada spesifik portnya
```
http_port 8080
visible_hostname Berlint
http_access allow all
```
Setelah itu restart node pada switch 1 dan switch 3, kemudian gunakan script berikut pada Eden, Garden, dan SSS
```
export http_proxy="http://10.23.2.3:8080"
env | grep -i proxy
```

### 8. Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)

Jawaban : Karena pengaksesan internet dilarang pada hari dan jam kerja saja, maka bisa kita tambahkan script berikut pada Berlint
```
acl available_working time M T W H F 08:00-17:00
```
Setelah itu, set available_working agar tidak diberikan akses internet
```
http_access deny available_working
```
### 9. Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)

Jawaban : Domain yang dapat diakses diiniasi dan dimasukkan ke dalam `/etc/squid/available-sites.acl`
```
echo -e '.loid-work.com
.franky-work.com
' > /etc/squid/available-sites.acl

acl sites_available_working dstdomain "/etc/squid/available-sites.acl"
```
Kemudian berikan akses ketika client mengakses pada hari dan jam kerja, maka hanya bisa mengakses domain tersebut dan ketika di luar waktu itu maka domain lain dapat diakses
```
http_access allow sites_available_working available_working
http_access deny sites_available_working
```
