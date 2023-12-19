# **Praktikum 5 Jaringan Komputer**
<div align=justify>

Berikut repository dari Kelompok E30 Praktikum Modul 5 Jaringan Komputer.

# **Anggota Kelompok**

| Nama                      | NRP        | Kelas                |
| ------------------------- | ---------- | ----------------     |
| Hana Maheswari            | 5025211182 | Jaringan Komputer E  |

# **Soal**
<div align=justify>

Berikut adalah topologi yang digunakan. 



(A) Tugas pertama, buatlah peta wilayah sesuai tersebut.<br>
- Richter adalah DNS Server
- Revolte adalah DHCP Server
- Sein dan Stark adalah Web Server
- Jumlah Host pada SchwerMountain adalah 64
- Jumlah Host pada LaubHills adalah 255
- Jumlah Host pada TurkRegion adalah 1022
- Jumlah Host pada GrobeForest adalah 512

(B) Untuk menghitung rute-rute yang diperlukan, gunakan perhitungan dengan metode VLSM. Buat juga pohonnya, dan lingkari subnet yang dilewati.<br>
(C) Kemudian buatlah rute sesuai dengan pembagian IP yang kalian lakukan. <br>
(D) Tugas berikutnya adalah memberikan ip pada subnet SchwerMountain, LaubHills, TurkRegion, dan GrobeForest menggunakan bantuan DHCP.<br>

## **VLSM**

### Pembagian Subnet
Pertama, dibuat plotting subnettingnya.



Kemudian dilakukan labelling netmask berdasarkan jumlah IP yang dibutuhkan.



### Tree
Selanjutnya dilakukan pembagian IP Address menggunakan tree sesuai dengan kebutuhan masing-masing subnet yang ada. Dimulai dari 192.221.0.0/20 kemudian bagi menjadi dua bagian, lakukan cara yang sama hingga 192.221.x.x/30.



### VLSM-IP



### Konfigurasi Node



- **Aura**
  ```
  auto eth0
  iface eth0 inet dhcp
  hwaddress ether d6:06:64:70:c3:e3
  
  #A7 Aura-Frieren
  auto eth1
  iface eth1 inet static
  	address 192.221.0.17
  	netmask 255.255.255.252
  
  #A8 Aura-Heiter
  auto eth2
  iface eth2 inet static
  	address 192.221.0.21
  	netmask 255.255.255.252
  ```
- **Frieren**
  ```
  #A7 Frieren-Aura
  auto eth0
  iface eth0 inet static
  	address 192.221.0.18
  	netmask 255.255.255.252
  	gateway 192.221.0.17
  
  #A5 Frieren-Himmel
  auto eth1
  iface eth1 inet static
  	address 192.221.0.9
  	netmask 255.255.255.252
  
  #A6 Frieren-ServerStark
  auto eth2
  iface eth2 inet static
  	address 192.221.0.13
  	netmask 255.255.255.252
  ```
- **Himmel**
  ```
  #A5 Himmel-Frieren
  auto eth0
  iface eth0 inet static
  	address 192.221.0.10
  	netmask 255.255.255.252
  	gateway 192.221.0.9
  
  #A4 Himmel-PCLaubHills
  auto eth1
  iface eth1 inet static
  	address 192.221.2.1
  	netmask 255.255.254.0
  
  #A3 Himmel-Switch1
  #Fern, PCSchwerMountains
  auto eth2
  iface eth2 inet static
  	address 192.221.0.129
  	netmask 255.255.255.128
  ```
- **Fern**
  ```
  #A3 Fern-Himmel
  auto eth0
  iface eth0 inet static
  	address 192.221.0.131
  	netmask 255.255.255.128
  	gateway 192.221.0.129
  
  #A2 Fern-ServerRitcher
  auto eth1
  iface eth1 inet static
  	address 192.221.0.5
  	netmask 255.255.255.252
  
  #A1 Fern-ServerRevolte
  auto eth2
  iface eth2 inet static
  	address 192.221.0.1
  	netmask 255.255.255.252
  ```
- **Heiter**
  ```
  #A8 Heiter-Aura
  auto eth0
  iface eth0 inet static
  	address 192.221.0.22
  	netmask 255.255.255.252
  	gateway 192.221.0.21
  
  #A9 Heiter-PCTurkRegion
  auto eth1
  iface eth1 inet static
  	address 192.221.8.1
  	netmask 255.255.248.0
  
  #A10 Heiter-Switch3
  #ServerSein, PCGrobeForest
  auto eth2
  iface eth2 inet static
  	address 192.221.4.1
  	netmask 255.255.252.0
  ```
- **Richter (DNS Server)**
  ```
  #A2 ServerRitcher-Fern
  auto eth0
  iface eth0 inet static
  	address 192.221.0.6
  	netmask 255.255.255.252
  	gateway 192.221.0.5
  ```
- **Revolte (DHCP Server)**
  ```
  #A1 ServerRitcher-Fern
  auto eth0
  iface eth0 inet static
  	address 192.221.0.2
  	netmask 255.255.255.252
  	gateway 192.221.0.1
  ```
- **Sein (Web Server)**
  ```
  #A10 ServerSein-Heiter
  auto eth0
  iface eth0 inet static
  	address 192.221.4.2
  	netmask 255.255.252.0
  	gateway 192.221.4.1
  ```
  - **Stark (Web Server)**
  ```
  #A6 Stark-Frieren
  auto eth0
  iface eth0 inet static
  	address 192.221.0.14
  	netmask 255.255.255.252
  	gateway 192.221.0.13
  ```
  - **Client**
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
  
### Konfigurasi Routing



- **Fern**
  ```
  route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.221.0.129			#default A1,A2,A3
  ```
- **Himmel**
  ```
  route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.221.0.9			#default A4,A3,A5
  route add -net 192.221.0.4 netmask 255.255.255.252 gw 192.221.0.131	#A2Richter
  route add -net 192.221.0.0 netmask 255.255.255.252 gw 192.221.0.131	#A1Revolte
  ```
- **Frieren**
  ```
  route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.221.0.17			#default A5,A6,A7
  route add -net 192.221.2.0 netmask 255.255.254.0 gw 192.221.0.10	#A4LaubHills
  route add -net 192.221.0.128 netmask 255.255.255.128 gw 192.221.0.10	#A3Fern,SchwerMountains
  route add -net 192.221.0.4 netmask 255.255.255.252 gw 192.221.0.10	#A2Richter
  route add -net 192.221.0.0 netmask 255.255.255.252 gw 192.221.0.10	#A1Revolte
  ```
- **Heiter**
  ```
  route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.221.0.21			#default A10,A9,A8
  ```
- **Aura**
  ```
  route add -net 192.221.0.0 netmask 255.255.255.252 gw 192.221.0.18	#A1Revolte
  route add -net 192.221.0.4 netmask 255.255.255.252 gw 192.221.0.18	#A2Richter
  route add -net 192.221.0.128 netmask 255.255.255.128 gw 192.221.0.18	#A3SchwerMountains
  route add -net 192.221.2.0 netmask 255.255.254.0 gw 192.221.0.18	#A4LaubHills
  route add -net 192.221.0.8 netmask 255.255.255.252 gw 192.221.0.18	#A5Frieren-Himmel
  route add -net 192.221.0.12 netmask 255.255.255.252 gw 192.221.0.18	#A6Stark
  
  route add -net 192.221.4.0 netmask 255.255.252.0 gw 192.221.0.22	#A10Sein,GrobeForest
  route add -net 192.221.8.0 netmask 255.255.248.0 gw 192.221.0.22	#A9TurkRegion
  ```
- **Client**
  ```
  echo "nameserver 192.168.122.1" > /etc/resolv.conf
  apt-get update
  apt-get install netcat -y
  ```

## **Soal Nomor 1**
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.
## **Penyelesaian Soal Nomor 1**

### Testing

## **Soal Nomor 2**
Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.
## **Penyelesaian Soal Nomor 2**

### Testing

## **Soal Nomor 3**
Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.
## **Penyelesaian Soal Nomor 3**

### Testing

## **Soal Nomor 4**
Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.
## **Penyelesaian Soal Nomor 4**

### Testing

## **Soal Nomor 5**
Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.
## **Penyelesaian Soal Nomor 5**

### Testing

## **Soal Nomor 6**
Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari WebServer tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).
## **Penyelesaian Soal Nomor 6**

### Testing

## **Soal Nomor 7**
Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.
## **Penyelesaian Soal Nomor 7**

### Testing

## **Soal Nomor 8**
Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.
## **Penyelesaian Soal Nomor 8**

### Testing

## **Soal Nomor 9**
Sadar akan adanya potensial saling serang antar kubu politik, maka WebServer harus dapat secara otomatis memblokir  alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit. 
(clue: test dengan nmap)
## **Penyelesaian Soal Nomor 9**

### Testing

## **Soal Nomor 10**
Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.
## **Penyelesaian Soal Nomor 10**

### Testing
