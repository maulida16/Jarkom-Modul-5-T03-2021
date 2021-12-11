# Jarkom-Modul-5-T03-2021

Setelah kalian mempelajari semua modul yang telah diberikan, Luffy ingin meminta bantuan untuk terakhir kalinya kepada kalian. Dan kalian dengan senang hati mau membantu Luffy.

### A. Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:
Keterangan : 	

![image](https://user-images.githubusercontent.com/73152464/145668906-f99a023a-a4d3-4141-b8f5-e8fc508b12e0.png)

    `
    Doriki adalah DNS Server
		Jipangu adalah DHCP Server
		Maingate dan Jorge adalah Web Server
		Jumlah Host pada Blueno adalah 100 host
		Jumlah Host pada Cipher adalah 700 host
		Jumlah Host pada Elena adalah 300 host
		Jumlah Host pada Fukurou adalah 200 host`

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



### D. Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.
