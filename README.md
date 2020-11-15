# Jarkom_Modul2_Lapres_E12
## Anggota Kelompok :
* Restu Agung Parama (05111840000123)
### Soal 1. Membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw <br>
Sebelumnya lakukan konfigurasi seperti pada modul pengenalan uml. <br>
Langkah pertama adalah konfigurasi pada file /etc/bind/named.conf.local sesuai dengan gambar dibawah. 
![image](https://user-images.githubusercontent.com/58405725/99186518-57abad80-2783-11eb-8a72-1f3e46dc7855.png)
<br>
Konfigurasi di atas sekaligus konfig untuk soal nomor 4 dan 6.
<br>
Lalu buat sebuah direktori dengan nama jarkom. <br>
```diff
nano /etc/bind/jarkom
```
Setelahnya salin db.local ke semerue12.pw .
```diff
cp /etc/bind/db.local /etc/bind/jarkom/semerue12.pw
```
Lalu konfigurasi pada file /etc/bind/jarkom/semerue12.pw .<br>
![image](https://user-images.githubusercontent.com/58405725/99186647-5464f180-2784-11eb-9e2b-e6f84d93faab.png)
<br>
Konfig di atas sekaligus konfig untuk soal no 2, 3, dan 6.
<br>
Lalu kita konfigurasi beberapa file di uml GRESIK dengan menambah `nameserver 10.151.71.108 dan 10.151.71.107` agar bisa mengakses ip-nya. <br>
### Soal 2. Buat Alias www.semerue12.pw
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semerue12.pw sesuai gambar di no 1. 
<br>
### Soal 3. Membuat subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semerue12.pw seperti gambar di nomor 1. <br>

### Soal 4. Membuat reverse domain untuk domain utama.
Konfigurasi pada UML MALANG di file /etc/bind/named.conf.local. sesuai dengan gambar di nomor 1.<br>
Salin file /etc/bind/db.local ke /etc/bind/jarkom/77.151.10.in-addr.arpa dan konfigurasi file tersebut sesuai gambar di bawah. <br>
![image](https://user-images.githubusercontent.com/58405725/99186893-fd601c00-2785-11eb-9d5d-b2f37d981166.png)
<br>
Lalu pindah ke UML Client dan konfigurasi file `/etc/resolv.conf`. Pada file tersebut uncomment nameserver dua atas dan comment yg IP Malang. <br>
Setelah melakukan konfigurasi update dan install dnsutils. <br>
```diff
apt-get update 
apt-get install dnsutils
```
Lalu konfigurasi file /etc/resolv.conf, comment nameserver dua atas, uncomment yg IP Malang. <br>
Setelah itu jalankan `host -t PTR 10.151.71.106` untuk mengeceknya. <br>

### Soal 5. Membuat DNS Server Slave pada MOJOKERTO.
Konfigurasi pada UML MALANG di file /etc/bind/named.conf.local seperti gambar pada nomor 1. <br>

Lalu pindah ke UML MOJOKERTO, update UML dan install aplikasi bind9. Konfigurasi file /etc/bind/named.conf.local seperti gambar dibawah. <br>
![image](https://user-images.githubusercontent.com/58405725/99186970-7d868180-2786-11eb-9c46-c3d52bc44b47.png)
<br>

### Soal 6. Membuat subdomain gunung.semerue12.pw delegasi pada MOJOKERTO dan mengarah ke IP PROBOLINGGO.
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semerue12.pw seperti gambar dibawah. <br>
```diff
@	IN	SOA	semerue12.pw. root.semerue12.pw. (
			2020111101	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		semerue12.pw.
@		IN	A		10.151.71.106	; IP MALANG
www		IN	CNAME	semerue12.pw.
penanjakan	IN	A		10.151.71.108	; IP PROBOLINGGO
ns1		IN	A		10.151.71.107	; IP MOJOKERTO
gunung	IN	NS		ns1
```
Konfigurasi file `/etc/bind/named.conf.options` dengan melakukan comment `dnssec-validation` auto menambahkan `allow-query{any;};` <br>
![image](https://user-images.githubusercontent.com/58405725/99187104-2fbe4900-2787-11eb-9773-ae030000a8a5.png)
<br>
Lalu tambahkan konfigurasi file `/etc/bind/named.conf.local` seperti dibawah ini. <br>
```diff
zone "semerue12.pw" {
    type master;
    file "/etc/bind/jarkom/semerue12.pw";
    allow-transfer { 10.151.71.107; }; // IP MOJOKERTO
};
```
Pindah ke UML MOJOKERTO. <br>
Konfigurasi file `/etc/bind/named.conf.options` dengan melakukan comment dnssec-validation auto dan menambahkan `allow-query{any;};` <br>
Setelah itu menambahkan konfigurasi file `/etc/bind/named.conf.local` <br>
```diff
zone "semerue12.pw" {
    type master;
    file "/etc/bind/delegasi/semerue12.pw";
    allow-transfer { any; };
};
```
Lalu buat folder delegasi dengan mkdir. <br>
```diff
mkdir /etc/bind/delegasi
```
Setelah itu salin file db.local ke `/delegasi/gunung.semerue12.pw.` <br>
```diff
cp /etc/bind/db.local /etc/bind/delegasi/gunung.semerue12.pw
```
Konfigurasi file gunung.semerue12.pw seperti dibawah ini. <br>
```diff
@	IN	SOA	gunung.semerue12.pw. root.gunung.semerue12.pw. (
			2020111102	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		gunung.semerue12.pw.
@		IN	A		10.151.71.108	; IP PROBO
```

### Soal 7. Membuat subdomain naik.gunung.semerue12.pw diarahkan ke Probo.
Konfigurasi pada UML MOJOKERTO di file /etc/bind/delegasi/gunung.semerue12.pw .<br>
```diff
@	IN	SOA	gunung.semerue12.pw. root.gunung.semerue12.pw. (
			2020111102	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		gunung.semerue12.pw.
@		IN	A		10.151.71.108	; IP PROBOLINGGO
naik		IN	A		10.151.71.108	; IP PROBOLINGGO
```


### Soal 8. Membuat domain semerue12.pw DocumentRoot /var/www/semerue12.pw dapat diakses pada semerue12.pw/index.php/home.
Konfigurasi pada UML PROBOLINGGO :
* Membuat file semerue12.pw.conf di sites-available, salin dari default
* Tambahkan ServerName semerue12.pw pada file tersebut
* Ubah DocumentRoot seperti yg diminta
* Download file semru.xip, unzip, copy ke /var/ww/semerue12.pw <br>
Berikut ini konfigurasinya : <br>
![image](https://user-images.githubusercontent.com/58405725/99187214-d86ca880-2787-11eb-9970-917b6646b51c.png)
 <br>
Dan ini merupakan hasil yang diminta : <br>
![image](https://user-images.githubusercontent.com/58405725/99187242-fa662b00-2787-11eb-9c98-2dcfddf0d387.png)
 <br>

### Soal 9. Mengaktifkan mod rewrite semerue12.pw/index.php/home -> semerue12.pw/home

Langkah menyelesaikan soal 9 :
* Nyalakan mod rewrite dengan a2enmod mod_rewrite
* Konfigurasi semerue12 agar bisa pake mod_rewrite kyk di modul
* Buat .htaccess di /var/www/semerue12.pw dengan rule berikut : <br>
```
RewriteEngine On
RewriteRule ^home$ index.php/home [C]
```
Dibawah ini merupakan hasilnya : <br>
![image](https://user-images.githubusercontent.com/58405725/99187242-fa662b00-2787-11eb-9c98-2dcfddf0d387.png) <br>


### Soal 10. Membuat penanjakan.semerue12.pw untuk nyimpen file asset, root pada /var/www/penanjakan.semerue12.pw

Langkah menyelesaikan soal 10 :
* Membuat config apache penanjakan.semerue12.pw.conf dengan cp
* Mengganti root, servername, dan alias <br>
![image](https://user-images.githubusercontent.com/58405725/99187308-81b39e80-2788-11eb-968c-6341013677ca.png)
 <br>
* Download dan unzip penanjakan.semeru.zip ke /var/ww/penanjakan/semerue12.pw <br>

* Jalankan a2ensite dan restart apache <br>

### Soal 11. Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.
* Tambah Options +Indexes sm -Indexes seperti di bawah. <br>
![image](https://user-images.githubusercontent.com/58405725/99187325-a1e35d80-2788-11eb-8929-32ea60d837a7.png)
 <br>
* Restart apache. <br>
* kemudian buka, maka hasilnya. <br>
![image](https://user-images.githubusercontent.com/58405725/99187357-cb9c8480-2788-11eb-9e6d-a702f441a358.png)
 <br>
 Hasil yang sama akan berlaku pada folder lainnya di dalam public.
### Soal 12. Mengatasi http error code 404, pake 404.html pada /error buat ganti defaultnya apache.

Berikut ini merupakan langkah penyelesaian :
* Setting di .htaccess seperti dibawah <br>
![image](https://user-images.githubusercontent.com/58405725/99187419-1a4a1e80-2789-11eb-8d01-0aef3a6bd341.png)
 <br>
* Untuk mengetes, buka alamat asal. <br>
![image](https://user-images.githubusercontent.com/58405725/99187434-3057df00-2789-11eb-991e-f6bb1059be04.png)
 <br>

### Soal 13. Alias penanjakan.semerue12.pw/public/javascripts jd penanjakan.semerue12.pw/js
Berikut ini merupakan langkah penyelesaian :
* Tambahkan alias di config penanjakan sesuai gambar nomor 11<br>

### Soal 14. Membuat web naik.gunung.semerue12.pw bisa diakses lewat port 8888
Berikut ini merupakan langkah penyelesaian : 
* Salin config seperti tadi, tapi portnya ganti 8888 <br>
![image](https://user-images.githubusercontent.com/58405725/99187492-862c8700-2789-11eb-9224-2415fc7c1142.png)
 <br>
* Setting port agar listen ke 8888 <br>
![image](https://user-images.githubusercontent.com/58405725/99187507-98a6c080-2789-11eb-98e0-9827f0030fc0.png)
) <br>
* Download, unzip ke /var/www <br>
* A2ensite, restart apache, buka <br>
![image](https://user-images.githubusercontent.com/58405725/99187519-aceabd80-2789-11eb-872a-3967d2d8ad1a.png)


### Soal 15. Membuat autentikasi naik gunung user “semeru” pass “kuynaikgunung”
Berikut ini merupakan langkah penyelesaian : 
* Membuat user sama pass seperti ini. Kami disini memakai pass “kuy” karena malas hehe <br>
![soal15_1](https://user-images.githubusercontent.com/49650266/98773133-d7034f00-241a-11eb-93ef-7ae06cc1bdb3.png) <br>
* Edit config seperti ini <br>
![image](https://user-images.githubusercontent.com/58405725/99187541-ce4ba980-2789-11eb-8d83-1094cb46aa9e.png)
<br>
* Hasilnya akan seperti ini <br>
![image](https://user-images.githubusercontent.com/58405725/99187563-e4f20080-2789-11eb-803d-635fb92ddb48.png)
<br>

### Soal 16. Ubah virtual host sesuai gambar no 8.
Hasilnya akan seperti ini : 
<br>
![image](https://user-images.githubusercontent.com/58405725/99187619-42864d00-278a-11eb-8428-c9cf75b9105d.png)

### Soal 17. Membuat request substring semeru redirect ke semeru.jpg
Edit .htaccess lalu tambhkan seperti ini : <br>
![image](https://user-images.githubusercontent.com/58405725/99187642-75c8dc00-278a-11eb-8cb3-d4011a3cc544.png)
<br>
Penjelasan: 
* `RewriteCond %{REQUEST_URI} !/public/images/` memastikan bahwa url request sekarang sesuai url-nya.
* `RewriteRule semeru` -> mengecek substring semeru pada url
* `public/images/semeru.jpg` url redirect nya <br> <br>
![image](https://user-images.githubusercontent.com/58405725/99187695-baed0e00-278a-11eb-9b7c-1730028849a1.png)
