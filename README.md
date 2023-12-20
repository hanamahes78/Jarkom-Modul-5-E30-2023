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

## **Script**
- **Heiter, Himmel (DHCP Relay)**
  <br>Pada Heiter dan Himmel install `apt-get install isc-dhcp-relay -y`. Kemudian edit server dengan mengarahkan dhcp-relay menuju Revolte `192.221.0.2` lalu tambahkan interfaces. Enable IP forwarding dengan uncoment `net.ipv4.ip_forward=1` dan lakukan `service isc-dhcp-relay start`.
  ```
  echo "nameserver 192.168.122.1" > /etc/resolv.conf
  
  apt-get update
  echo "" | apt-get install isc-dhcp-relay -y
  
  echo -e '
  # Defaults for isc-dhcp-relay initscript
  # sourced by /etc/init.d/isc-dhcp-relay
  # installed at /etc/default/isc-dhcp-relay by the maintainer scripts
  
  #
  # This is a POSIX shell fragment
  #
  
  # What servers should the DHCP relay forward requests to?
  SERVERS="192.221.0.2"
  
  # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
  INTERFACES="eth0 eth1 eth2 eth3"
  
  # Additional options that are passed to the DHCP relay daemon?
  OPTIONS=""
  ' > /etc/default/isc-dhcp-relay
  
  echo 'net.ipv4.ip_forward=1
  ' > /etc/default/sysctl.conf
  
  service isc-dhcp-relay restart
  ```
- **Richter (DNS Server)**
  <br>Pada Richter, install bind9 dengan `apt-get install bind9 -y`, kemudian masukkan nameserver. Lakukan service bind9 start.
  ```
  echo "nameserver 192.168.122.1" > /etc/resolv.conf
  
  apt-get update
  apt-get install bind9 -y
  
  echo -e '
  options {
          directory "/var/cache/bind";
  
          // If there is a firewall between you and nameservers you want
          // to talk to, you may need to fix the firewall to allow multiple
          // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
             forwarders {
                  192.168.122.1;
             };
  
          //========================================================================
          // If BIND logs error messages about the root key being expired,
          // you will need to update your keys.  See https://www.isc.org/bind-keys
          //========================================================================
          //dnssec-validation auto;
          allow-query{any;};
          auth-nxdomain no;    # conform to RFC1035
          listen-on-v6 { any; };
  };
  ' > /etc/bind/named.conf.options
  
  service bind9 start
  ```
- **Revolte (DHCP Server)**
  <br>Pada Revolte, edit file `/etc/default/isc-dhcp-server`, dengan menambahkan `INTERFACES="eth0"`. Pada dhcp-server isikan data pada `/etc/dhcp/dhcpd.conf`, lalu lakukan `service isc-dhcp-server restart`.
  ```
  echo "nameserver 192.168.122.1" > /etc/resolv.conf
  
  apt-get update
  apt-get install isc-dhcp-server -y
  
  echo -e '
  INTERFACES="eth0"
  ' > /etc/default/isc-dhcp-server
  
  echo -e '
  option domain-name "example.org";
  option domain-name-servers ns1.example.org, ns2.example.org;
  default-lease-time 600;
  max-lease-time 7200;
  
  ddns-update-style none;
  
  #A4 LaubHills
  subnet 192.221.2.0 netmask 255.255.254.0 {
          range 192.221.2.2 192.221.3.254;
          option routers 192.221.2.1;
          option broadcast-address 192.221.3.255;
          option domain-name-servers 192.221.0.6;
          default-lease-time 600;
          max-lease-time 7200;
  }
  
  #A3 SchwerMountain
  subnet 192.221.0.128 netmask 255.255.255.128 {
          range 192.221.0.130 192.221.0.254;
          option routers 192.221.0.129;
          option broadcast-address 192.221.0.255;
          option domain-name-servers 192.221.0.6;
          default-lease-time 600;
          max-lease-time 7200;
  }
  
  #A9 TurkRegion
  subnet 192.221.8.0 netmask 255.255.248.0 {
          range 192.221.8.2 192.221.15.254;
          option routers 192.221.8.1;
          option broadcast-address 192.221.15.255;
          option domain-name-servers 192.221.0.6;
          default-lease-time 600;
          max-lease-time 7200;
  }
  
  #A10 GrobeForest
  subnet 192.221.4.0 netmask 255.255.252.0 {
          range 192.221.4.2 192.221.7.254;
          option routers 192.221.4.1;
          option broadcast-address 192.221.7.255;
          option domain-name-servers 192.221.0.6;
          default-lease-time 600;
          max-lease-time 7200;
  }
  
  #A1 Routing dari Revolte ke router
  subnet 192.221.0.0 netmask 255.255.255.252 {
          option routers 192.221.0.1;
  }
  
  #A1
  subnet 192.221.0.0 netmask 255.255.255.252 {}
  
  #A2
  subnet 192.221.0.4 netmask 255.255.255.252 {}
  
  #A5
  subnet 192.221.0.8 netmask 255.255.255.252 {}
  
  #A6
  subnet 192.221.0.12 netmask 255.255.255.252 {}
  
  #A7
  subnet 192.221.0.16 netmask 255.255.255.252 {}
  
  #A8
  subnet 192.221.0.20 netmask 255.255.255.252 {}
  ' > /etc/dhcp/dhcpd.conf
  
  service isc-dhcp-server restart
  ```
- **Sein, Stark (Web Server)**
  <br>Install `apt-get install apache2` kemudian lakukan `service apache2 start` untuk start server.
  ```
  echo 'nameserver 192.168.122.1 ' > /etc/resolv.conf

  apt update
  apt install netcat -y
  apt install apache2 -y
  service apache2 start
  echo "$HOSTNAME" > /var/www/html/index.html
  ```
- **Client**
  ```
  echo "nameserver 192.168.122.1" > /etc/resolv.conf
  apt-get update
  apt-get install netcat -y
  ```

## **Soal Nomor 1**
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.
## **Script Nomor 1**
Cek IP dari Aura yang berhubungan dengan NAT menggunakan command `ip -br a`. Karena diminta untuk tidak menggunakan MASQUERADE, maka digunakan SNAT. Source akan diubah dari yang awalnya 0.0 ke Aura dengan `--to-source IPETH0`.
- Aura
  ```
  IPETH0="$(ip -br a | grep eth0 | awk '{print $NF}' | cut -d'/' -f1)"
  iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$IPETH0" -s 192.221.0.0/20
  ```
  Keterangan:
  - `ip -br a`: Memberi list network interface dan konfigurasinya
  - `grep eth0`: Filter output
  - `awk '{print $NF}'`: Mengambil last column, yaitu IP address and subnet
  - `cut -d'/' -f1`: Mengambil IP address dengan memisah string dengan length
  - `iptables -t nat`: Menggunakan nat
  - `-A POSTROUTING`: Menambahkan chain POSTROUTING
  - `-o eth0`: Out interface network eth0
  - `-j SNAT`: SNAT untuk memodikasi source address
  - `--to-source "$IPETH0"`: Source SNAT eth0

### Testing
Cek apakah eth0 pada setiap client sudah berada dalam range yang ada di dalam tree. Lakukan ping google.com pada tiap client.



## **Soal Nomor 2**
Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.
## **Script Nomor 2**
Untuk melakukan filtering, perlu mendefinisikan protokol TCP dan destination port 8080 untuk paket diterima. Dilakukan juga definisi protokol lain yang digunakan untuk didrop.
> Script dijalankan pada **Revolte** dengan command `bash no2.sh`
- Revolte
  ```
  iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
  iptables -A INPUT -p tcp -j DROP # Drop semua TCP
  iptables -A INPUT -p udp -j DROP # Drop semua UDP
  ```
  Keterangan:
  - `iptables -A INPUT`: Menggunakan chain INPUT
  - `-p tcp`: Mendefinisikan protokol yang digunakan, yaitu tcp
  - `-p udp`: Mendefinisikan protokol yang digunakan, yaitu udp
  - `--dport 8080`: Mendefinisikan destination port paket, yaitu 8080
  - `-j ACCEPT`: Mengizinkan paket data, paket diterima
  - `-j DROP`: Menolak paket data, paket didrop

### Testing
Cek pada client menuju Revolte menggunakan netcat untuk TCP dan UDP.



## **Soal Nomor 3**
Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.
## **Script Nomor 3**
Untuk membatasi DHCP dan DNS Server digunakan `connlimit` untuk menolak koneksi ICMP yang masuk, jika jumlah koneksi melebihi limit 3.
> Script dijalankan pada **Revolte, Richter** dengan command `bash no3.sh`
- Revolte, Richter
  ```
  iptables -I INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
  ```
  Keterangan:
  - `iptables -I INPUT`: Menggunakan chain INPUT
  - `-p icmp`: Mendefinisikan protokol yang digunakan, yaitu ICMP (ping)
  - `-m connlimit`: Menggunakan match modul connection limit
  - `--connlimit-above 3`: Mencocokkan jumlah koneksi tcp yang ada tidak di atas 3
  - `--connlimit-mask 0`: Hanya memperbolehkan 3 koneksi setiap subnet dalam satu waktu
  - `-j DROP`: Paket didrop

### Testing
Cek pada lebih dari 3 client menuju Revolte dengan ping.



## **Soal Nomor 4**
Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.
## **Script Nomor 4**
Melakukan drop paket pada Web Server dengan koneksi SSH, kecuali pada client `GrobeForest`. Akan dicek IP dengan command `ip a` karena IP client selalu berganti.
> Script dijalankan pada **Sein, Stark** dengan command `bash no4.sh`
- Sein, Stark
  ```
  iptables -A INPUT -p tcp --dport 22 -s <IP GrobeForest> -j ACCEPT
  iptables -A INPUT -p tcp --dport 22 -j DROP
  ```
  Keterangan:
  - `iptables -A INPUT`: Menggunakan chain INPUT
  - `-p tcp`: Mendefinisikan protokol yang digunakan, yaitu tcp
  - `--dport 22`: Mendefinisikan destination port paket, yaitu 22 (port SSH)
  - `-s <IP GrobeForest>`: Menentukan sumber address yang digunakan, hanya IP dari GrobeForest yang diaccept
  - `-j ACCEPT`: Paket diterima
  - `-j DROP`: Paket didrop

### Testing
Cek pada client menuju Sein menggunakan netcat untuk port 22.



## **Soal Nomor 5**
Selain itu, akses menuju Web Server hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.
## **Script Nomor 5**
Melakukan pembatasan paket menuju Web Server, kecuali pada jam kerja Senin-Jumat pada pukul 08.00-16.00.
> Script dijalankan pada **Sein, Stark** dengan command `bash no5.sh`
- Sein, Stark
  ```
  iptables -A INPUT -m time --timestart 08:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
  iptables -A INPUT -j REJECT
  ```
  Keterangan:
  - `iptables -A INPUT`: Menggunakan chain INPUT
  - `-m time`: Menggunakan modul time
  - `--timestart 08:00`: Mendefinisikan waktu mulai yaitu 08:00
  - `--timestop 16:00`: Mendefinisikan waktu berhenti yaitu 16:00
  - `--weekdays Mon,Tue,Wed,Thu,Fri`: Membatasi rule berlaku hanya untuk weekdays (Senin-Jumat)
  - `-j ACCEPT`: Paket diterima
  - `-j REJECT`: Paket ditolak

### Testing
Cek pada client menuju Sein dengan ping pada waktu tertentu.



## **Soal Nomor 6**
Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari Web Server tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).
## **Script Nomor 6**
Melakukan pembatasan paket menuju Web Server, kecuali pada jam kerja pada waktu yang ditetapkan.
> Script dijalankan pada **Sein, Stark** dengan command `bash no6.sh`
- Sein, Stark
  ```
  iptables -I INPUT -m time --timestart 12:00 --timestop 13:00 --weekdays Mon,Tue,Wed,Thu -j REJECT
  iptables -I INPUT -m time --timestart 11:00 --timestop 13:00 --weekdays Fri -j REJECT
  ```
  Keterangan:
  - `iptables -I INPUT`: Menggunakan chain INPUT
  - `-m time`: Menggunakan modul time
  - `--timestart 12:00`, `--timestart 11:00`: Mendefinisikan waktu mulai yaitu 12:00 dan 11.00
  - `--timestop 13:00`: Mendefinisikan waktu berhenti yaitu 13:00
  - `--weekdays Mon,Tue,Wed,Thu`, `--weekdays Fri`: Membatasi rule berlaku hanya untuk weekdays (Senin-Kamis, Jumat)
  - `-j REJECT`: Paket ditolak
  
### Testing
Cek pada client menuju Sein dengan ping pada waktu tertentu.



## **Soal Nomor 7**
Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.
## **Script Nomor 7**
- Sein, Stark
  ```
  ```
### Testing

## **Soal Nomor 8**
Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses Web Server hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.
## **Script Nomor 8**
Melakukan pembatasan paket menuju Web Server, hingga masa pencoblosan pemilu yaitu 19 Oktober 2023 hingga 15 Februari 2024.
> Script dijalankan pada **Sein, Stark** dengan command `bash no8.sh`
- Sein, Stark
  ```
  iptables -A INPUT -p tcp --dport 80 -s 192.221.0.0/30 -m time --datestart 2023-10-19 --datestop 2024-02-15 -j DROP
  ```
  Keterangan:
  - `iptables -A INPUT`: Menggunakan chain INPUT
  - `-p tcp`: Mendefinisikan protokol yang digunakan, yaitu tcp
  - `--dport 80`: Mendefinisikan destination port paket, yaitu 80
  - `-s 192.221.0.0/30`: Menentukan sumber address yang digunakan, hanya IP subnet A1 yang diizinkan
  - `--datestart 2023-10-19`: Mendefinisikan waktu mulai yaitu 2023-10-19
  - `--datestop 2024-02-15`: Mendefinisikan waktu berhenti yaitu 2024-02-15
  - `-j DROP`: Paket didrop
  
### Testing
Cek pada Revolte menuju Sein dengan ping pada waktu tertentu.



## **Soal Nomor 9**
Sadar akan adanya potensial saling serang antar kubu politik, maka Web Server harus dapat secara otomatis memblokir alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit.
(clue: test dengan nmap)
## **Script Nomor 9**
Membatasi alamat IP pada Web Server dengan membuat list IP address dinamis, kemudian dicocokkan dengan list tersebut dengan rule yang digunakan. Paket akan di drop jika terjadi lebih dari 20 update dalam waktu 10 menit.
- Sein, Stark
  ```
  iptables -N scanport
  iptables -A INPUT -m recent --name scanport --update --seconds 600 --hitcount 20 -j DROP
  iptables -A FORWARD -m recent --name scanport --update --seconds 600 --hitcount 20 -j DROP
  iptables -A INPUT -m recent --name scanport --set -j ACCEPT
  iptables -A FORWARD -m recent --name scanport --set -j ACCEPT
  ```
  Keterangan:
  - `iptables -N scanport`: Membuat chain baru dengan nama scanport
  - `iptables -A INPUT`: Menggunakan chain INPUT
  - `iptables -A FORWARD`: Menggunakan chain FORWARD
  - `-m recent`: Menggunakan modul recent untuk membandingkan list
  - `--name scanport`: List nama yang akan digunakan untuk commands
  - `--update`: Mengupdate "last seen" timestamp jika sesuai
  - `--seconds 600`: Mencocokkan agar hanya terjadi saat alamat IP ada pada list dan dalam 600 detik (10 menit) terakhir
  - `--hitcount 20`: Mencocokkan agar hanya terjadi saat alamat IP ada pada list dan paket telah diterima lebih besar atau sama dengan nilai yang diberikan sehingga melanjutkan aksi
  - `--set`: Menambahkan source address paket ke list
  - `-j DROP`: Paket didrop
  - `-j ACCEPT`: Paket diterima
    
### Testing
Cek pada client menuju Sein dengan ping 20 kali.



## **Soal Nomor 10**
Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.
## **Script Nomor 10**

### Testing
