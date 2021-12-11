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

####Mengatur DHCP Relay

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

####Mengatur DHCP Server

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
