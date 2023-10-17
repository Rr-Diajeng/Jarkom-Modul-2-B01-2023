# Jarkom-Modul-2-B01-2023-

| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| Rr. Diajeng Alfisyahrinnisa Anandha | 5025211147 | Jaringan Komputer (B) |

## Soal No 1

### Soal:
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 

### Jawab:

- Kita buat dulu topologi nya di GNS3:
    <br>![a](./assets/topologi.png)</br>

- Lalu, kita lakukan network configuration di masing-masing node:  
    a. Node Router:  
        <br>![b](./assets/b.png)</br>
    b. Node YudhistiraDNSMaster:  
        <br>![c](./assets/c.png)</br>
    c. Node WerkudaraDNSSlave:  
        <br>![d](./assets/d.png)</br>
    d. Node NakulaClient:  
        <br>![e](./assets/e.png)</br>
    e. Node SadewaClient:  
        <br>![f](./assets/f.png)</br>
    f. Node ArjunaLoadBalancer:  
        <br>![g](./assets/g.png)</br>
    g. Node PrabukusumaWebServer:  
        <br>![h](./assets/h.png)</br>
    h. Node AbimanyuWebServer:  
        <br>![i](./assets/i.png)</br>
    i. Node WisanggeniWebServer:  
        <br>![j](./assets/j.png)</br>
- Setelah itu, di router, kita tetapkan
    ```iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.9.0.0/16```   
    yang ditaruh dalam .bashrc  pada node `router`  
    ![k](./assets/k.png)
- Setelah itu, untuk semua node, kita tambahkan   
    `nameserver 192.168.122.1`  
    Tujuan: agar semua node dapat terhubung dengan internet, disini saya contohkan untuk node `PrabukusumaWebServer`   
    ![l](./assets/l.png)  

### Kendala Pengerjaan:
Tidak ada

## Soal No 2

### Soal:
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

### Jawab:

- Kita ingin membuat website utama yang mengarah ke node arjuna. Maka, kita pakai node `YudhistiraDNSMaster` yang menjadi DNS Master  
- Konfigurasi di YudhistiraDNSMaster:  
    a. Kita tetapkan syntax pada .bashrc di node YudhistiraDNSMaster:  
        <br>![m](./assets/m.png)</br>
    b. Kita lakukan konfigurasi domain `arjuna.b01.com`:  
        <br>![n](./assets/n.png)</br>
        > Disini, kita setup zone `arjuna.b01.com` dengan tipe master di `/etc/bind/named.conf.local`  
        > Lalu, kita setup di `/etc/bind/jarkom/arjuna.b01.com` untuk membuat domain baru. 
- Kita lakukan pengujian di Client dengan syntax:  
    <br>![o](./assets/o.png)</br>
- Hasil:
    <br>![p](./assets/p.png)</br>
    > Dari pengujian `host -t CNAME www.arjuna.b01.com`, kita ketahui bahwa www.arjuna.b01.com merupakan alias dari arjuna.b01.com
    > Lalu dari hasil ping, kita ketahui bahwa pengambilan data dilakukan oleh `IP: 10.9.1.4` yang merupakan IP dari node `ArjunaLoadBalancer`

### Kendala Pengerjaan:
Tidak ada

## Soal No 3

### Soal:
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Jawab:
- Kita ingin membuat website utama dengan akses abimanyu.b01.com dan alias www.abimanyu.b01.com, sehingga kita melakukan konfigurasi di DNS Master yaitu di `YudhistiraDNSMaster`:  
    <br>![q](./assets/q.png)<br>
    > Dari kode diatas, kita buat zone untuk `abimanyu.b01.com` pada `/etc/bind/named.conf.local`
    > Kita juga tetapkan domain abimanyu.b01.com beserta aliasnya di `/etc/bind/jarkom/abimanyu.b01.com`
- Lalu, kita lakukan pengujian di client:  
    <br>![r](./assets/r.png)</br>
- Hasil dari pengujian:  
    <br>![s](./assets/s.png)</br>
    > Kita ketahui bahwa www.abimanyu.b01.com merupakan alias dari abimanyu.b01.com
    > Lalu, kita ketahui juga bahwa request dilakukan oleh `IP = 10.9.3.3` yang merupakan IP dari node `AbimanyuWebServer`

### Kendala Pengerjaan:
Tidak ada

## Soal No 4

### Soal:
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Jawab:
- Karena kita ingin buat subdomain parikesit.abimanyu.b01.com dan diatur DNS-nya di Yudhistira, maka kita lakukan konfigurasi di node `YudhistiraDNSMaster` dan mengarah ke IP `AbimanyuWebServer`  
    <br>![t](./assets/t.png)</br>
    > Dari kode diatas, karena parikesit merupakan sebuah  subdomain yang mengarah ke IP AbimanyuWebServer, maka kita gunakan `A` 
- Lalu, kita lakukan pengujian di client:  
    <br>![u](./assets/u.png)</br>
- Hasil yang kita dapatkan:  
    <br>![v](./assets/v.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 5

### Soal:
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

### Jawab:
- Kita gunakan node `Yudhistira` untuk membuat reverse domain dengan IP menuju abimanyu:  
    <br>![x](./assets/x.png)</br>
- Kita lakukan pengujian di client:  
    <br>![y](./assets/y.png)</br>
- Hasil yang kita dapatkan:  
    <br>![z](./assets/z.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 6

### Soal:
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Jawab:
- Maka, di node `YudhistiraDNSMaster` kita tetapkan type dari `abimanyu.b01.com` adalah `master`:  
    <br>![a1](./assets/a1.png)</br>
    > Disini, kita terapkan  
        ```
            also-notify { 10.9.2.3; } // IP Werkudara
            allow-transfer { 10.9.2.3; } // IP Werkudara
        ```  
- Di node `WerkudaraDNSSlave`, kita tetapkan type dari `abimanyu.b01.com` adalah `slave`:  
    <br>![b1](./assets/b1.png)</br>
- Kita lakukan uji client:  
    <br>![c1](./assets/c1.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 7

### Soal:
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

### Jawab:
- Karena kita ingin subdomain dari domain `abimanyu.b01.com`, maka kita buat subdomain di node `YudhistiraDNSMaster` yang akan didelegasikan:
    <br>![d1](./assets/d1.png)</br>
    > Disini, kita tetapkan subdomain bernama `baratayuda` yang akan menuju IP AbimanyuWebServer
    > Lalu, kita ubah konfigurasi di `/etc/bind/named.conf.options` untuk menghapus bagian `dssec-validation` dan menambahkan syntax `allow-query` yang digunakan untuk query khusus domain
- Karena kita akan melakukan delegasi dari yudhistira ke werkudara, maka kita lakukan konfigurasi juga di node `WerkudaraDNSSlave`:  
    <br>![e1](./assets/e1.png)</br>
    > Lalu, kita ubah konfigurasi di `/etc/bind/named.conf.options` untuk menghapus bagian `dssec-validation` dan menambahkan syntax `allow-query` yang digunakan untuk query khusus domain
- Setelah itu, kita lakukan pengujian client:
    <br>![f1](./assets/f1.png)</br>
- Hasil dari pengujian:
    <br>![g1](./assets/g1.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 8

### Soal:
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Jawab:
- Karena kita disuruh buat subdomain melalui werkudara, maka kita melakukan konfigurasi di node `WerkudaraDNSSlave`:  
    <br>![h1](./assets/h1.png)</br>
    > Disini, kita buat subdomain baru bernama `rjp.baratayuda.abimanyu.b01.com` 
    > Kita juga buat alias bernama `www.rjp.baratayuda.abimanyu.b01.com`
- Lalu, kita lakukan pengujian di client:
    <br>![i1](./assets/i1.png)</br>
- Hasil pengujian: 
    <br>![j1](./assets/j1.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 9-10

### Soal:
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh:  
    - Prabakusuma:8001  
    - Abimanyu:8002  
    - Wisanggeni:8003  

### Jawab:

- Untuk memainkan loadbalancing, kita pertama-tama setup konfigurasi di DNS Master terlebih dahulu yaitu di node `YudhistiraDNSMaster`
    <br>![k1](./assets/k1.png)</br>
- Lalu, kita setup nginx di `ArjunaLoadBalancer`:
    <br>![l1](./assets/l1.png)</br>
    > Karena menggunakan konsep Round Robin, maka kita buat urutan. Bahwa jika port 8001 mati, maka web akan up ke port 8002. Jika port 8001 dan 8002 mati, maka web akan up ke port 8003
- Kita setup di node `PrabukusumaWebServer` yang menjadi worker: 
    <br>![m1](./assets/m1.png)</br>
    > Karena untuk `PrabukusumaWebServer` kita mengarah ke port 8001, maka kita ubah di `/etc/nginx/sites-available/arjuna.b01.com` untuk listen ke port 8001
- Kita setup di node `AbimanyuWebServer` yang menjadi worker:
    <br>![n1](./assets/n1.png)</br>
    > Karena untuk `AbimanyuWebServer` kita mengarah ke port 8002, maka kita ubah di `/etc/nginx/sites-available/arjuna.b01.com` untuk listen ke port 8002
- Kita setup di node `WisanggeniWebServer` yang menjadi worker:
    <br>![o1](./assets/o1.png)</br>
    > Karena untuk `WisanggeniWebServer` kita mengarah ke port 8002, maka kita ubah di `/etc/nginx/sites-available/arjuna.b01.com` untuk listen ke port 8003
- Lalu, kita lakukan pengujian client: 
    <br>![p1](./assets/p1.png)</br>
- Hasil uji ketika nginx di semua node masih menyala:
    <br>![q1](./assets/q1.png)</br>
- Hasil uji ketika nginx di node `PrabukusumaWebServer` dimatikan:
    <br>![r1](./assets/r1.png)</br>
- Hasil uji ketika nginx di node `AbimanyuWebServer` dimatikan:
    <br>![s1](./assets/s1.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 11

### Soal:
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Jawab:
- Karena kita ingin up ke webserver Apache pada abimanyu, maka kita lakukan setup di node `AbimanyuWebServer`:
    <br>![t1](./assets/t1.png)</br>
    > Disini, kita stop dulu nginx nya agar apache2 dapat bekerja
    > Lalu, kita lakukan git clone untuk mengambil assets untuk `AbimanyuWebServer`
- Kita lakukan pengujian client:
    <br>![u1](./assets/u1.png)</br>
- Hasil pengujian:
    <br>![v1](./assets/v1.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 12

### Soal:
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### Jawab:
- Pada node `AbimanyuWebServer`, kita lakukan rewrite agar dari `www.abimanyu.yyy.com/index.php/home` dapat menjadi `www.abimanyu.yyy.com/home`
    <br>![w1](./assets/w1.png)</br>
    > Disini, kita buat file .htaccess agar dapat dijalankan oleh virtual host
    > Isi dari .htaccess:  
        a. RewriteEngine On:  
            Ini adalah perintah yang mengaktifkan mod_rewrite di server web Apache.  
            <br></br> 
        b. RewriteRule ^home$ index.php/home [NC, L]:  
            RewriteRule: Ini adalah perintah untuk menentukan aturan pemetaan ulang URL. 
            <br></br>  
        ^home$: ini mencocokkan URL yang hanya berisi kata "home". Pola tersebut merupakan ekspresi reguler yang mencocokkan kata "home" di URL.  
            <br></br> 
          index.php/home: Ini adalah bagian target atau hasil pemetaan ulang. Ini akan menggantikan URL yang cocok dengan "home". Dalam hal ini, itu mengarahkan permintaan ke "index.php/home".
          <br></br> 
        [NC] berarti aturan ini bersifat case-insensitive, artinya "home" dan "Home" akan cocok.
        <br></br> 
        [L] menunjukkan bahwa ini adalah aturan terakhir yang akan diterapkan jika ada kecocokan. Artinya, jika URL cocok dengan pola, maka pemetaan ulang akan diterapkan, dan tidak akan ada aturan pemetaan ulang tambahan yang diterapkan setelahnya.

- Lalu, kita lakukan pengujian client:  
    <br>![x1](./assets/x1.png)</br>
- Hasil uji:
    <br>![y1](./assets/y1.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 13

### Soal:
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Jawab:
- karena kita ingin membuat subdomain `www.parikesit.abimanyu.b01.com`, maka kita lakukan konfigurasi di node `AbimanyuWebServer`:  
    <br>![z1](./assets/z1.png)</br>
- Setelah membuat subdomain di `AbimanyuWebServer`, kita juga lakukan konfigurasi di DNS Master yaitu di `YudhistiraDNSMaster` karena domain `abimanyu.b01.com` di set up di Yudhistira:  
    <br>![a2](./assets/a2.png)</br>
- Lalu, kita lakukan pengujian client:
    <br>![b2](./assets/b2.png)</br>
- Hasil pengujian:
    <br>![c2](./assets/c2.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 14

### Soal:
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### Jawab:
- Di `AbimanyuWebServer`, kita lakukan konfigurasi untuk menerapkan listing dan mematikan listing:
    <br>![d2](./assets/d2.png)</br>
    > Disini, untuk menerapkan listing maka menggunakan  
     `Options + Indexes`  
    > Untuk menonaktfikan listing, maka menggunakan  
    `Options -Indexes`
- Lalu, kita lakukan pengujian client:
    <br>![e2](./assets/e2.png)</br>
- Hasil pengujian untuk /public:
    <br>![f2](./assets/f2.png)</br>
- Hasil pengujian untuk /secret:
    <br>![g2](./assets/g2.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 15

### Soal:
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### Jawab:
- Untuk melakukan kostumisasi khusus untuk folder /error, maka kita lakukan setup di `AbimanyuWebServer`: 
    <br>![h2](./assets/h2.png)</br>
    > Disini, kita terapkan  
    `ErrorDocument 403 /error/403.html`  
    yang berarti untuk eror 403 akan muncul tampilan sesuai di /error/403.html
    > Disini, kita terapkan  
    `ErrorDocument 404 /error/404.html`  
    yang berarti untuk eror 404 akan muncul tampilan sesuai di /error/404.html
- Lalu kita lakukan pengujian client:
    <br>![i2](./assets/i2.png)</br>
- Hasil pengujian untuk 403:
    <br>![j2](./assets/j2.png)</br>
- Hasil pengujian untuk 404:
    <br>![k2](./assets/k2.png)</br>

### Kendala Pengerjaan:
Saat diuji dengan kode yang sama itu blank page. Ternyata, saya ga scroll ke atas.. pas scroll ke atas, baru muncul tulisan.

## Soal No 16

### Soal:
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

### Jawab:
- karena kita ingin mengubah /public/js menjadi /js, maka kita terapkan prinsip rewrite di `AbimanyuWebServer`
    <br>![l2](./assets/l2.png)</br>
    > Karena kita ingin menggunakan alias, maka menggunakan syntax:  
    `Alias "/js" "/var/www/parikesit.abimanyu.b01.com/public/js"`
- Lalu kita lakukan pengujian client:
    <br>![m2](./assets/m2.png)</br>
- Hasil pengujian:
    <br>![n2](./assets/n2.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 17

### Soal:
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

### Jawab:
- Untuk mengubah port, maka kita lakukan pengubahan di node `AbimanyuWebServer`:
    <br>![o2](./assets/o2.png)</br>
    <br>![p2](./assets/p2.png)</br>
    > Disini, kita ubah listen port menjadi 14000 dan 14400
- Lalu, kita lakukan pengujian client:
    <br>![q2](./assets/q2.png)</br>
- Hasil pengujian:
    <br>![r2](./assets/r2.png)</br>
    <br>![s2](./assets/s2.png)</br>
    <br>![t2](./assets/t2.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 18

### Soal:
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

### Jawab:
- Kita terapkan authentikasi di node `AbimanyuWebServer`:
    <br>![u2](./assets/u2.png)</br>  
    > Disini kita terapkan aturan autentikasi bahwa hanya pengguna dengan nama wayang yang dapat masuk ke area yang diproteksi, lalu hanya user root yang dapat memiliki akses ke berkas password atau .htpasswd. Lalu, user lain dapat membaca berkas tersebut. Selain itu, untuk membaca autentikasi, kita butuh www-data:www-data agar apache2 dapat melakukan autentikasi  
    <br>![v2](./assets/v2.png)</br>
    <br>![w2](./assets/w2.png)</br>
    <br>![x2](./assets/x2.png)</br>
- Kita lakukan pengujian
    <br>![y2](./assets/y2.png)</br>
- Hasil pengujian:
    <br>![z2](./assets/z2.png)</br>
    <br>![a3](./assets/a3.png)</br>
    <br>![b3](./assets/b3.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 19

### Soal:
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

### Jawab:
- Untuk secara mutlak apabila akses IP dari abimanyu ke `www.abimanyu.b01.com`, maka dapat dilakukan:
    <br>![c3](./assets/c3.png)</br>
    > Disini, kita menggunakan syntax:  
    `Redirect Permanent / http://www.abimanyu.b01.com`
- Kita lakukan pengujian client:
    <br>![d3](./assets/d3.png)</br>
- Hasil uji:
    <br>![e3](./assets/e3.png)</br>

### Kendala Pengerjaan:
Tidak ada

## Soal No 20

### Soal:
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

### Jawab:
- Karena kita ingin semua url yang mengandung kata "abimanyu" akan menuju "abimanyu.png", maka kita lakukan konfigurasi di `AbimanyuWebServer`:
    <br>![f3](./assets/f3.png)</br>
    <br>![g3](./assets/g3.png)</br>
    > Terdapat `RewriteEngine On` yang berarti perintah yang mengaktifkan mod_rewrite di server web Apache
    <br></br>
    > 'RewriteCond %{Request_uri} ^.*abimanyu.*' yang berarti jika permintaan URI (Request URI) mengandung string "abimanyu"
    <br></br>
    > `RewriteCond %{Request_uri} !/public/images/abimanyu.png` yang berarti Ini mengecualikan URI yang cocok dengan "/public/images/abimanyu.png". Artinya, jika URI mengarah ke berkas "/public/images/abimanyu.png", maka aturan pemetaan ulang tidak akan diterapkan.
    <br></br>
    > RewriteRule .* http://parikesit.abimanyu.b01.com/public/images/abimanyu.png [L, R=31] yang berarti:
    <br></br>
    a. .* mencocokkan semua karakter dalam URI
    <br></br>
    b. [L] menandakan bahwa ini adalah aturan terakhir yang akan diterapkan jika ada kecocokan
    <br></br>
    c. [R=31] menandakan bahwa server akan memberikan respons redirect (kode status HTTP 301) ke URL yang dituju. Kode status 301 menunjukkan bahwa alamat permanen diubah, dan browser akan mengikuti alamat yang baru
    <br></br>
- Kita lakukan pengujian:
    <br>![h3](./assets/h3.png)</br>
- Hasil pengujian:
    <br></br>
    a. yang menuju /abimanyu-student.jpg
    <br>![i3](./assets/i3.png)</br>
    b. yang menuju /lalaabimanyu.png
    <br>![j3](./assets/j3.png)</br>
    c. yang menuju /abimanyu.png
    <br>![k3](./assets/k3.png)</br>
    d. yang menuju /buddies.jpg
    <br>![l3](./assets/l3.png)</br>

### Kendala Pengerjaan:
Saat praktikum, belum sepenuhnya mengarah ke abimanyu.png. Waktu revisi, semua unsur "abimanyu" telah mengarah ke abimanyu.png
