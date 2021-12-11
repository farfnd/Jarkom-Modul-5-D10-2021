# Laporan Resmi Soal Shift Modul 5 Jarkom 2021

## Anggota Kelompok D10
- Mohammad Faderik I H (05111940000023)
- Farhan Arifandi (05111940000061)
- Yusril Zubaydi (05111940000160)

## Pendahuluan
> (A) Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy di bawah ini:
> 
> ![image](https://user-images.githubusercontent.com/70105993/145669899-f12fb756-f9f5-43fa-95e4-ead742d04a13.png)
> 
> Keterangan:
>   - Doriki adalah DNS Server
>   - Jipangu adalah DHCP Server
>   - Maingate dan Jorge adalah Web Server
>   - Jumlah Host pada Blueno adalah 100 host
>   - Jumlah Host pada Cipher adalah 700 host
>   - Jumlah Host pada Elena adalah 300 host
>   - Jumlah Host pada Fukurou adalah 200 host

> (B) Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting,

## Subnetting
Subnetting yang kami lakukan di sini menggunakan teknk VLSM.
![VLSM](https://cdn.discordapp.com/attachments/848199470025801749/919170962757410826/145669899-f12fb756-f9f5-43fa-95e4-ead742d04a13.png)

Dari pembagian subnet tersebut didapatkan :
| Subnet | Jumlah IP | Netmask |
|--------|-----------|---------|
| A1     | 3         | /29     |
| A2     | 101       | /25     |
| A3     | 701       | /22     |
| A4     | 2         | /30     |
| A5     | 2         | /30     |
| A6     | 301       | /23     |
| A7     | 201       | /24     |
| A8     | 3         | /29     |
| Total  | 1314      | /21     |

Untuk treenya sebagai berikut :
![tree](https://cdn.discordapp.com/attachments/848199470025801749/919173668423221258/prak5.jpg)

Untuk tabel subnet lengkap sebagai berikut :

| Subnet | Node                   | Network ID  | Netmask         | Broadcast Address |
|--------|------------------------|-------------|-----------------|-------------------|
| A1     | Doriki-Jipangu-Water7  | 10.26.7.128 | 255.255.255.248 | 10.26.7.135       |
| A2     | Blueno-Water7          | 10.26.7.0   | 255.255.255.128 | 10.26.7.127       |
| A3     | Cipher-Water7          | 10.26.0.0   | 255.255.252.0   | 10.26.0.255       |
| A4     | Water7-Foosha          | 10.26.7.144 | 255.255.255.252 | 10.26.7.147       |
| A5     | Foosha-Guanhao         | 10.26.7.148 | 255.255.255.252 | 10.26.7.151       |
| A6     | Elena-Guanhao          | 10.26.4.0   | 255.255.254.0   | 10.26.4.255       |
| A7     | Guanhao-Fukuro         | 10.26.6.0   | 255.255.255.0   | 10.26.6.255       |
| A8     | Guanhao-Jorge-Maingate | 10.26.7.136 | 255.255.255.248 | 10.26.7.143       |

Untuk setting network configuration tiap node seperti ini :
* Foosha
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
hwaddress ether 6a:e6:46:00:b3:5b

auto eth1
iface eth1 inet static
address 10.26.7.149
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 10.26.7.146
netmask 255.255.255.252
```
* Water7
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.26.7.145
netmask 255.255.255.252
gateway 10.26.7.146

auto eth1
iface eth1 inet static
address 10.26.7.1
netmask 255.255.255.128

auto eth2
iface eth2 inet static
address 10.26.0.1
netmask 255.255.252.0

auto eth3
iface eth3 inet static
address 10.26.7.129
netmask 255.255.255.248
```

* Guanhao
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.26.7.150
netmask 255.255.255.252
gateway 10.26.7.149

auto eth1
iface eth1 inet static
address 10.26.4.1
netmask 255.255.254.0

auto eth2
iface eth2 inet static
address 10.26.6.1
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 10.26.7.137
netmask 255.255.255.248
```

* Doriki
```
auto eth0
iface eth0 inet static
address 10.26.7.130
netmask 255.255.255.248
gateway 10.26.7.129
```

* Jipangu
```
auto eth0
iface eth0 inet static
address 10.26.7.131
netmask 255.255.255.248
gateway 10.26.7.129
```

* Jorge
```
auto eth0
iface eth0 inet static
address 10.26.7.138
netmask 255.255.255.248
gateway 10.26.7.137
```

* MainGate
```
auto eth0
iface eth0 inet static
address 10.26.7.139
netmask 255.255.255.248
gateway 10.26.7.137
```

* Blueno
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

* Cipher
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

* Elena
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

* Fukurou
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

> (C) Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

Untuk Routing jalankan command ini pada router Foosha
```
# kiri
route add -net 10.26.7.128 netmask 255.255.255.248 gw 10.26.7.145
route add -net 10.26.7.0 netmask 255.255.255.128 gw 10.26.7.145
route add -net 10.26.0.0 netmask 255.255.252.0 gw 10.26.7.145

# kanan
route add -net 10.26.4.0 netmask 255.255.254.0 gw 10.26.7.150
route add -net 10.26.6.0 netmask 255.255.255.0 gw 10.26.7.150
route add -net 10.26.7.136  netmask 255.255.255.248 gw 10.26.7.150
```

Setelah itu, kita bisa melakukan cek dengan command `route -n` dan akan menghasilkan seperti ini
![routing](https://cdn.discordapp.com/attachments/848199470025801749/919178336285102080/unknown.png)

> (D) Tugas berikutnya adalah memberikan IP pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

Untuk melakukan hal tersebut pertama, kita perlu menjadikan Jipangu sebagai DHCP Server

Pada Jipangu
1. Lakukan update dan install `isc-dhcp-server dengan` command
```
apt-get update
apt-get install isc-dhcp-server -y
```

2. Lakukan edit pada `/etc/default/isc-dhcp-server` dengan menambahkan interface yang mengarah ke `Water7` sehingga menjadi seperti ini
![/etc/default/isc-dhcp-server](https://cdn.discordapp.com/attachments/848199470025801749/919181243004911646/unknown.png)

3. Edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan ip pada tiap subnet
```
# Blueno A2
subnet 10.26.7.0 netmask 255.255.255.128 {
        range 10.26.7.2 10.26.7.126;
        option routers 10.26.7.1;
        option broadcast-address 10.26.7.127;
        option domain-name-servers 10.26.7.130;
        default-lease-time 600;
        max-lease-time 7200;
}

# Cipher A3
subnet 10.26.0.0 netmask 255.255.252.0 {
        range 10.26.0.2 10.26.3.254;
        option routers 10.26.0.1;
        option broadcast-address 10.26.3.255;
        option domain-name-servers 10.26.7.130;
        default-lease-time 600;
        max-lease-time 7200;
}

# Elena A6
subnet 10.26.4.0 netmask 255.255.254.0 {
        range 10.26.4.2 10.26.5.254;
        option routers 10.26.4.1;
        option broadcast-address 10.26.5.255;
        option domain-name-servers 10.26.7.130;
        default-lease-time 600;
        max-lease-time 7200;
}


# Fukurou A7
subnet 10.26.6.0 netmask 255.255.255.0 {
        range 10.26.6.2 10.26.6.254;
        option routers 10.26.6.1;
        option broadcast-address 10.26.6.255;
        option domain-name-servers 10.26.7.130;
        default-lease-time 600;
        max-lease-time 7200;
}

# Routing dari Jipangu ke router
subnet 10.26.7.128 netmask 255.255.255.248 {
        option routers 10.26.7.129;
}
```

Setelah itu, kita juga perlu melakukan setting dhcp relay pada setiap router (Water7 dan Guanhao)

1. Lakukan update dan install `isc-dhcp-relay`
```
apt-get update
apt-get install isc-dhcp-relay -y
```

2. Edit file `/etc/default/isc-dhcp-relay` sehingga menjadi seperti di bawah ini
![/etc/default/isc-dhcp-relay](https://cdn.discordapp.com/attachments/848199470025801749/919183734077534218/unknown.png)

Terakhir, install DNS Server pada Doriki

1. Update dan Install bind9 dengan command
```
apt-get update
apt-get install bind9 -y
```

2. Lakukan edit pada file `/etc/bind/named.conf.options` sehingga menjadi seperti di bawah ini
![/etc/bind/named.conf.options](https://cdn.discordapp.com/attachments/848199470025801749/919185477112852511/unknown.png)

## Soal 1
> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

Pada Foosha, jalankan command berikut
```
iptables -t nat -A POSTROUTING -s 10.26.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.105
```

## Soal 2
> Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

Command yang dijalankan di Doriki (DNS server) dan Jipangu (DHCP server):

```bash
iptables -A FORWARD -d 10.26.7.128/29 -i eth0 -p tcp --dport 80 -j DROP
```

Command tersebut berfungsi menolak/drop seluruh paket yang menuju subnet A1 (Doriki-Jipangu-Water7) melalui port 80 (HTTP).

**Pengujian**

Pengujian dilakukan dengan melakukan ping ke situs web yang masih menggunakan protokol HTTP seperti monta.if.its.ac.id dari Doriki/Jipangu.

![recording(5)](https://user-images.githubusercontent.com/70105993/145674382-395cdb51-10cf-4435-96ca-7f7b20d055ad.gif)

## Soal 3
> Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

Command yang dijalankan di Doriki (DNS server) dan Jipangu (DHCP server):

```bash
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

Command tersebut menggunakan INPUT chain untuk membatasi koneksi masuk maksimal 3 (`--connlimit-above 3`) dari mana saja (`--connlimit-mask 0`) dan menolak/drop paket yang masuk dengan protokol (`-p`) ICMP jika koneksi masuk di atas 3.

**Pengujian**

Pengujian dilakukan dengan melakukan ping ke IP Doriki/Jipangu dari node lain secara bersamaan hingga ping dilakukan di lebih dari 3 node.

![recording(3)](https://user-images.githubusercontent.com/70105993/145673457-03a856d5-a74c-4c5c-846b-f5761ef70b3b.gif)

## Soal 4-5
> Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut:
> 
> 4. Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Command yang dijalankan di Doriki:

```bash
iptables -A INPUT -s 10.26.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.26.7.0/25 -j REJECT

iptables -A INPUT -s 10.26.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.26.0.0/22 -j REJECT
```

- IP yang dijadikan sebagai nilai parameter `-s` (source) adalah IP subnet Blueno (10.26.7.0/25) dan Cipher (10.26.0.0/22).
- Waktu pembatasan dimulai pada pukul 07.00 dan berakhir pada pukul 15.00 setiap Senin-Kamis sesuai soal, selain waktu tersebut akses akan ditolak.

**Pengujian**

Pengujian dilakukan dengan melakukan ping ke IP Doriki (10.26.7.130) dari client Blueno dan Cipher dengan tanggal dan waktu sistem disesuaikan dengan pembatasan waktu akses.

-  Tanggal dan waktu sistem saat pengujian adalah Sabtu, 11 Desember 2021 pukul 09.45 UTC (di luar waktu akses yang diperbolehkan)
    
    ![image](https://user-images.githubusercontent.com/70105993/145672129-ba3b3e08-940a-418a-93d1-c49936931a11.png)
    
    ![image](https://user-images.githubusercontent.com/70105993/145672200-dc50b8d2-d36d-4df3-8a32-795604fe2587.png)

- Tanggal dan waktu sistem diubah ke waktu akses yang diperbolehkan, contohnya Kamis, 9 Desember 2021 pukul 08.00 UTC dengan command `date -s "9 DEC 2021 08:00:00"`

    ![image](https://user-images.githubusercontent.com/70105993/145672151-1e17865f-daec-4541-a1d4-e530e2a7c0e5.png)
    
    ![image](https://user-images.githubusercontent.com/70105993/145672220-b9f6862b-277b-4223-9912-97254e627bf9.png)

> 5. Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
> 
> Selain itu (2 nomor di atas) di-reject.

Command yang dijalankan di Doriki:

```bash
iptables -A INPUT -s 10.26.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.26.4.0/23 -j REJECT

iptables -A INPUT -s 10.26.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.26.6.0/24 -j REJECT
```

- IP yang dijadikan sebagai nilai parameter `-s` (source) adalah IP subnet Elena (10.26.4.0/23) dan Fukurou (10.26.6.0/24).
- Waktu pembatasan dimulai pada pukul 15.01 dan berakhir pada pukul 06.59 setiap hari sesuai soal, selain waktu tersebut akses akan ditolak.

**Pengujian**

Pengujian dilakukan dengan melakukan ping ke IP Doriki (10.26.7.130) dari client Elena dan Fukurou dengan tanggal dan waktu sistem disesuaikan dengan pembatasan waktu akses.

-  Tanggal dan waktu sistem saat pengujian adalah Sabtu, 11 Desember 2021 pukul 09.52 UTC (di luar waktu akses yang diperbolehkan)
    
    ![image](https://user-images.githubusercontent.com/70105993/145672310-09c2c805-2887-4d1c-99ac-1f3f2f1efed6.png)
    
    ![image](https://user-images.githubusercontent.com/70105993/145672281-dd594348-7bfb-459c-9d90-2bd8f4bc0d3e.png)

- Tanggal dan waktu sistem diubah ke waktu akses yang diperbolehkan, contohnya Sabtu, 11 Desember 2021 pukul 18.00 UTC dengan command `date -s "11 DEC 2021 18:00:00"`

    ![image](https://user-images.githubusercontent.com/70105993/145672387-01ee9425-dcb6-402f-91a3-9b2bccd4e2c4.png)
    
    ![image](https://user-images.githubusercontent.com/70105993/145672406-7f568e05-e3b0-417f-b756-bb8e87ff5dc9.png)

## Soal 6
> Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

Command yang dijalankan di Guanhao:
```bash
iptables -t nat -A PREROUTING -p tcp -d 10.26.7.130 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.26.7.138
iptables -t nat -A PREROUTING -p tcp -d 10.26.7.130 -j DNAT --to-destination 10.26.7.139

iptables -t nat -A POSTROUTING -p tcp -d 10.26.7.138 -j SNAT --to-source 10.26.7.130
iptables -t nat -A POSTROUTING -p tcp -d 10.26.7.139 -j SNAT --to-source 10.26.7.130
```

Command tersebut menggunakan PREROUTING chain untuk mengarahkan paket yang awalnya menuju ke IP Doriki (10.26.7.130) sebagai DNS server ke Maingate (10.26.7.138) dan Jorge (10.26.7.139), serta POSTROUTING chain untuk mengubah alamat asal paket yang dikirim dari Maingate dan Jorge menjadi dari Doriki.

**Pengujian**

1. Install Netcat pada node Doriki, Guanhao, Elena, Fukurou, Maingate, dan Jorge dengan command

    ```bash
    apt-get update
    apt-get install netcat -y
    ```
2. Pada Maingate dan Jorge, gunakan Netcat untuk menangkap request pada port tertentu, contohnya port 80 dengan command `nc -l -p 80`
3. Pada Elena dan Fukurou, gunakan Netcat untuk mengirim request ke Doriki melalui port tertentu (sama dengan port yang di-listen pada Maingate dan Jorge), contohnya port 80 dengan command `nc 10.26.7.130 80`

    ![recording(4)](https://user-images.githubusercontent.com/70105993/145674188-02baeb68-0ff9-4302-9bd2-9f5fe0ca713b.gif)
