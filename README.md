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



(A) Tugas pertama, buatlah peta wilayah sesuai tersebut.
    Richter adalah DNS Server
		Revolte adalah DHCP Server
		Sein dan Stark adalah Web Server
		Jumlah Host pada SchwerMountain adalah 64
		Jumlah Host pada LaubHills adalah 255
		Jumlah Host pada TurkRegion adalah 1022
		Jumlah Host pada GrobeForest adalah 512
(B) Untuk menghitung rute-rute yang diperlukan, gunakan perhitungan dengan metode VLSM. Buat juga pohonnya, dan lingkari subnet yang dilewati.
(C) Kemudian buatlah rute sesuai dengan pembagian IP yang kalian lakukan. 
(D) Tugas berikutnya adalah memberikan ip pada subnet SchwerMountain, LaubHills, TurkRegion, dan GrobeForest menggunakan bantuan DHCP.

### List

1. [VLSM](#VLSM)
2. s
3. s
4. s
5. s
6. s
7. s
8. s
9. s
10. s

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

### Testing
