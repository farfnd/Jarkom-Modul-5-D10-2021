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

> (C) Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

> (D) Tugas berikutnya adalah memberikan IP pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.


## Soal 1
> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

## Soal 2
> Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

## Soal 3
> Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

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
