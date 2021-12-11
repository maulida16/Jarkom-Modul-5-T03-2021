# Jarkom-Modul-5-T03-2021

Setelah kalian mempelajari semua modul yang telah diberikan, Luffy ingin meminta bantuan untuk terakhir kalinya kepada kalian. Dan kalian dengan senang hati mau membantu Luffy.

### A. Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:
Keterangan : 	

![image](https://user-images.githubusercontent.com/73152464/145668906-f99a023a-a4d3-4141-b8f5-e8fc508b12e0.png)

    
    Doriki adalah DNS Server
    Jipangu adalah DHCP Server
    Maingate dan Jorge adalah Web Server
    Jumlah Host pada Blueno adalah 100 host
    Jumlah Host pada Cipher adalah 700 host
    Jumlah Host pada Elena adalah 300 host
    Jumlah Host pada Fukurou adalah 200 host

Jawab: Berikut adalah topologi yang telah dibuat

![image](https://user-images.githubusercontent.com/73152464/145668979-0d3b703f-f19e-4250-9bd6-6709990a4782.png)

### B. Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting, 

Jawab:

Kami menggunakan teknik VLSM untuk subnetting. Kemudian kami menentukan subnet-subnet pada topologinya. Berikut adalah gambar dari penentuan subnet kami.

![image](https://user-images.githubusercontent.com/73152464/145669060-e047457e-ce47-4532-b652-5db7987fa45d.png)

Selanjutnya kami menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dan melakukan labelling netmask berdasarkan jumlah IP yang dibutuhkan.

![image](https://user-images.githubusercontent.com/73152464/145669252-89276b62-ebe5-4288-abe5-1ebd6e9d18f1.png)

Setelah itu kami mendapatkan subnet terbesar yaitu /21. Subnet terbesar yang dibentuk memiliki NID 10.43.0.0 dengan netmask /21. Kami menghitung pembagian IP berdasarkan NID dan netmask tersebut menggunakan pohon seperti gambar di bawah.

![image](https://user-images.githubusercontent.com/73152464/145669315-6a6b9519-efe2-4165-9158-c9b3ac23a497.png)

Dari pohon tersebut akan mendapat pembagian IP sebagai berikut.

![image](https://user-images.githubusercontent.com/73152464/145669336-9d2644fc-c9ca-4110-838d-c8fe2dcab9b4.png)

### C. Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

Jawab:

Konfigurasi di Foosha (DHCP Relay)

	auto eth0
	iface eth0 inet dhcp

	auto eth1
	iface eth1 inet static
		address 10.43.0.1
		netmask 255.255.255.252

	auto eth2
	iface eth2 inet static
		address 10.43.0.5
		netmask 255.255.255.252

Konfigurasi di Water7 (DHCP Relay)

	auto eth0
	iface eth0 inet static
		address 10.43.0.2
		netmask 255.255.255.252
	auto eth1
	iface eth1 inet static
		address 10.43.0.9
		netmask 255.255.255.248
	auto eth2
	iface eth2 inet static
		address 10.43.0.129
		netmask 255.255.255.128
	auto eth3
	iface eth3 inet static
		address 10.43.4.1
		netmask 255.255.252.0

Konfigurasi di Guanhao (DHCP Relay)

	auto eth0
	iface eth0 inet static
		address 10.43.0.6
		netmask 255.255.255.252
	auto eth1
	iface eth1 inet static
		address  10.43.0.17
		netmask 255.255.255.248
	auto eth2
	iface eth2 inet static
		address  10.43.2.1
		netmask 255.255.254.0
	auto eth3
	iface eth3 inet static
		address  10.43.1.1
		netmask 255.255.255.0

Konfigurasi di Jipangu (DHCP Server)

	auto eth0
	iface eth0 inet static
		address 10.43.0.11
		netmask 255.255.255.248
		gateway 10.43.0.9

Konfigurasi di Doriki (DNS Server)

	auto eth0
	iface eth0 inet static
		address 10.43.0.10
		netmask 255.255.255.248
		gateway 10.43.0.9

Konfigurasi di Maingate (Web Server)

	auto eth0
	iface eth0 inet static
		address 10.43.0.19
		netmask 255.255.255.248
		gateway 10.43.0.17

Konfigurasi di Jorge (Web Server)

	auto eth0
	iface eth0 inet static
		address 10.43.0.18
		netmask 255.255.255.248
		gateway 10.43.0.17

Konfigurasi di Blueno, Chiper, Elena, Fukurou (Client)

	auto eth0
	iface eth0 inet dhcp


Routing di Foosha agar semua server dan PC terhubung ke Foosha.

- Ke arah Blueno (A2 /25) dengan gateway IP Water7
```
route add -net 10.43.0.128 netmask 255.255.255.128 gw 10.43.0.2 
```
- Ke arah Chiper (A3 /22) dengan gateway IP Water7
```
route add -net 10.43.4.0 netmask 255.255.252.0 gw 10.43.0.2 
```
- Ke arah Doriki & Jipangu (A4 /29) dengan gateway IP Water7
```
route add -net 10.43.0.8 netmask 255.255.255.248 gw 10.43.0.2
```
- Ke arah Elena (A6 /23) dengan gateway IP Guanhao
```
route add -net 10.43.2.0 netmask 255.255.254.0 gw 10.43.0.6
```
- Ke arah Fukurou (A7 /24) dengan gateway IP Guanhao
```
route add -net 10.43.1.0 netmask 255.255.255.0 gw 10.43.0.6 
```
- Ke arah Jorge & maingate (A8 /29) dengan gateway IP Guanhao
```
route add -net 10.43.0.16 netmask 255.255.255.248 gw 10.43.0.6 
```

### D. Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

Jawab:

#### Mengatur DHCP Relay

Pada Foosha install DHCP Relay
```
apt-get update
apt-get install isc-dhcp-relay -y

echo '
SERVERS="10.43.0.11"
INTERFACES="eth2 eth1"
OPTIONS=""
' > /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
```

![image](https://user-images.githubusercontent.com/73152464/145670570-b7068b73-f7ce-4ede-9775-4486bbaf0810.png)

Pada Water7 install DHCP Relay
```
echo "nameserver 192.168.122.1" > /etc/resolv.conf
apt update
apt install isc-dhcp-relay -y
echo '
SERVERS="10.43.0.11"
INTERFACES="eth2 eth3 eth0 eth1"
OPTIONS=""
' > /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
```

![image](https://user-images.githubusercontent.com/73152464/145670610-6f410d18-cbcc-4dc4-b18a-818624447645.png)

Pada Guanhao install DHCP Relay
```
echo "nameserver 192.168.122.1" > /etc/resolv.conf
apt update
apt install isc-dhcp-relay -y
echo '
SERVERS="10.43.0.11"
INTERFACES="eth2 eth3 eth1 eth0"
OPTIONS=""
' > /etc/default/isc-dhcp-relay
service isc-dhcp-relay restart
```

![image](https://user-images.githubusercontent.com/73152464/145670651-4eda1900-719e-4812-9921-ae8a9754cc04.png)

#### Mengatur DHCP Server

Pada Jipangu install DHCP Server

```
echo "nameserver 192.168.122.1" > /etc/resolv.conf
apt update
apt install isc-dhcp-server -y
echo '
INTERFACES="eth0"
' > /etc/default/isc-dhcp-server
```

![image](https://user-images.githubusercontent.com/73152464/145670752-4d7029eb-8d9e-4b17-a5d7-67e414277ba0.png)

Kemudian edit pada file /etc/dhcp/dhcpd.conf untuk menambahkan subnet

```
echo '
ddns-update-style none;
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
default-lease-time 600;
max-lease-time 7200;
log-facility local7;
subnet 10.43.4.0 netmask 255.255.252.0 {
    range 10.43.4.2 10.43.6.254;
    option routers 10.43.4.1;
    option broadcast-address 10.43.6.255;
    option domain-name-servers 10.43.0.10;
    default-lease-time 360;
    max-lease-time 7200;
}
subnet 10.43.0.128 netmask 255.255.255.128 {
    range 10.43.0.130 10.43.0.254;
    option routers 10.43.0.129;
    option broadcast-address 10.43.0.255;
    option domain-name-servers 10.43.0.10;
    default-lease-time 720;
    max-lease-time 7200;
}
subnet 10.43.2.0 netmask 255.255.254.0 {
    range 10.43.2.2 10.43.3.254;
    option routers 10.43.2.1;
    option broadcast-address 10.43.3.255;
    option domain-name-servers 10.43.0.10;
    default-lease-time 720;
    max-lease-time 7200;
}
subnet 10.43.1.0 netmask 255.255.255.0 {
    range 10.43.1.2 10.43.1.254;
    option routers 10.43.1.1;
    option broadcast-address 10.43.1.255;
    option domain-name-servers 10.43.0.10;
    default-lease-time 720;
    max-lease-time 7200;
}
subnet 10.43.0.8 netmask 255.255.255.248 {}
subnet 10.43.0.4 netmask 255.255.255.252 {}
subnet 10.43.0.0 netmask 255.255.255.252 {}
subnet 10.43.0.16 netmask 255.255.255.248 {}
' > /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
```

![image](https://user-images.githubusercontent.com/73152464/145670768-fe1d8f8c-28ba-4f32-9bc9-9f51398b4799.png)

### 1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

digunakan `SNAT` untuk menjalankan `iptables` nya, command:

```bash
IPETH0="$(ip -br a | grep eth0 | awk '{print $NF}' | cut -d'/' -f1)"
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$IPETH0" -s 10.43.0.0/21
```

command tersebut akan mengambil ip dhcp pada foosha untuk dijadikan `--to-source` pada `iptables SNAT`. digunakannya `grep` dan `awk` untuk mengotomasi supaya saat project ter-close, ip dhcp yang berganti-ganti maka akan otomatis terganti pada `iptables` nya.

Percobaan `ping google.com` pada Fukurou dan Foosha

![1-1](https://user-images.githubusercontent.com/73921231/145673719-49b1d76a-a1e9-460e-a802-2ed392a2a84b.jpg)

![1-2](https://user-images.githubusercontent.com/73921231/145673741-127ce4f5-dd00-4251-bb72-ac1bb79c97ad.jpg)


### 2. Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

Karena akses dari luar pasti akan melalui Foosha, maka pada Foosha dijalankan command iptables sebagai berikut

```bash
iptables -A FORWARD -d 10.43.0.11 -i eth0 -p tcp --dport 80 -j DROP
iptables -A FORWARD -d 10.43.0.10 -i eth0 -p tcp --dport 80 -j DROP
```

apabila terdapat request akses menuju ke Doriki (DNS Server) dan Jipangu (DHCP Server) pada port 80 (HTTP) maka akan di drop

### 3. Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

Pada Doriki (DNS Server) dan Jipangu (DHCP Server), dijalankan command sebagai berikut

```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

command tersebut akan menambahkan rule ke `iptables` untuk membatasi `ICMP` sebanyak 3 client menggunakan `--connlimit`

Percobaan `ping` ke Jipangu

1. Elena

![2-1](https://user-images.githubusercontent.com/73921231/145673921-9603ee0f-0dda-46d9-948f-4580bea7c4fa.jpeg)

2. Blueno

![2-2](https://user-images.githubusercontent.com/73921231/145673924-53b1aba9-5bdd-4c36-afe6-3197a7eebe03.jpeg)

3. Fukurou

![2-3](https://user-images.githubusercontent.com/73921231/145673943-932e6545-63aa-4043-b239-3f1cecac87f0.jpeg)

4. Chiper

![2-4](https://user-images.githubusercontent.com/73921231/145673951-71f295dd-b91a-463f-8e88-8c041df06e28.jpeg)

Dari percobaan tersebut dapat terlihat bahwa pada `ping` ke-empat (lebih dari 3) di Chiper akan terhenti karena adanya rule `iptables` tadi

### 4. Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.
Pada node DORIKI, inputkan command sebagai berikut
```
#Blueno
iptables -A INPUT -s 10.45.7.0/25 -m time --weekdays Fri,Sat,Sun -j REJECT
iptables -A INPUT -s 10.45.7.0/25 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu -j REJECT
iptables -A INPUT -s 10.45.7.0/25 -m time --timestart 15:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu -j REJECT
#Cipher
iptables -A INPUT -s 10.45.0.0/22 -m time --weekdays Fri,Sat,Sun -j REJECT
iptables -A INPUT -s 10.45.0.0/22 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu -j REJECT
iptables -A INPUT -s 10.45.0.0/22 -m time --timestart 15:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu -j REJECT
```

![image](https://user-images.githubusercontent.com/61416036/145673281-b0d28274-c126-458d-ae4b-3f3d191bd891.png)

![image](https://user-images.githubusercontent.com/61416036/145673283-eb577c42-31a0-4503-b186-7a382cb98270.png)

![image](https://user-images.githubusercontent.com/61416036/145673287-e9c8cc50-3db8-41e2-b343-a5f9cf7e6a26.png)

![image](https://user-images.githubusercontent.com/61416036/145673290-ca808406-c3aa-40dd-b2be-d0d14023bcf7.png)


### 5. Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
Pada node DORIKI, inputkan command sebagai berikut
```
#Elena
iptables -A INPUT -s 10.45.4.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT 
#Fukuro
iptables -A INPUT -s 10.45.6.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT 
```

![image](https://user-images.githubusercontent.com/61416036/145673265-23a0ddba-c3b1-4b82-8fe9-6b60a789d8e1.png)

![image](https://user-images.githubusercontent.com/61416036/145673266-ddea6d60-3466-425d-8e1e-1fbfd945d658.png)

![image](https://user-images.githubusercontent.com/61416036/145673268-ffd3ca77-61e7-442c-b2d3-ccf37bb45266.png)

![image](https://user-images.githubusercontent.com/61416036/145673270-12a3dd4f-78e1-43ee-9c47-81a1c39f1590.png)


### 6. Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate
Pada node GUANHAO, inputkan command sebagai berikut
```
iptables -A PREROUTING -t nat -p tcp -d 10.45.7.130 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.45.7.138:80
iptables -A PREROUTING -t nat -p tcp -d 10.45.7.130 -j DNAT --to-destination 10.45.7.139:80
```
Pada server JORGE lakuka listening di port 80 dengan `nc -l -p 80`. Setelah itu ELENA melakukan koneksi menggunakan netcat dengan `nc 10.43.0.10 80`

![image](https://user-images.githubusercontent.com/61416036/145673326-bbb36dcb-af91-48c8-b813-e121deb1706e.png)

![image](https://user-images.githubusercontent.com/61416036/145673328-f5123099-c6ae-4bd5-a93c-20bee75029dd.png)


Pada server MAINGATE lakuka listening di port 80 dengan `nc -l -p 80`. Setelah itu ELENA melakukan koneksi menggunakan netcat dengan `nc 10.43.0.10 80`

![image](https://user-images.githubusercontent.com/61416036/145673330-3ace2b97-81e6-41ba-85f6-ee88e9c0e1f4.png)

![image](https://user-images.githubusercontent.com/61416036/145673333-2b3e9b87-c201-4ef0-ae78-0a80e1ca22ec.png)


