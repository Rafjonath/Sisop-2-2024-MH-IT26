# Sisop-2-2024-MH-IT26

## ***KELOMPOK IT 26***
  | Nama      | NRP         |
  |-----------|-------------|
  | Rafael Jonathan Arnoldus   | 5027231006  |
  | Muhammad Abhinaya Al-Faruqi  | 5027231011  |  
  | Danendra Fidel Khansa  | 5027231063  |

## Bagian Pengerjaan Soal 
+ Soal 1 = Rafael Jonathan Arnoldus
+ Soal 2 = Muhammad Abhinaya Al-Faruqi
+ Soal 3 = Danendra Fidel Khansa
+ Soal 4 = Danendra Fidel Khansa

## ***MODUL 1***
  Berikut untuk struktur repository pengumpulan dari hasil praktikum sistem operasi modul 2 :
  ```
—soal_1:
        — virus.c
—soal_2:
	      — management.c
—soal_3:
	      — admin.c
—soal_4:
	      — setup.c
```
## ***SOAL 1 (Rafael)***
Gavriel adalah seorang cyber security enthusiast. Suatu hari, ia dikontrak oleh sebuah perusahaan ternama untuk membuat sebuah program yang cukup rumit dengan bayaran jutaan rupiah. Karena tergoda dengan nominal bayaran tersebut, Gavriel langsung menerima tawaran tersebut. Setelah mencoba membuat dan mengembangkan program tersebut selama seminggu, laptop yang digunakan Gavriel mengalami overheat dan mati total sehingga harus dilarikan ke tukang servis terdekat. Karena keterbatasan waktu dalam pembuatan program dan tidak ingin mengecewakan perusahaan, Gavriel meminta bantuan kalian untuk membuat program tersebut dengan ketentuan sebagai berikut:

a. Program dapat menerima input path berupa ‘argv’ untuk mengatur folder dimana file akan dieksekusi

b. Program tersebut berfungsi untuk mereplace string dengan ketentuan sebagai berikut:
```   
   i. String m4LwAr3 direplace dengan string [MALWARE]
   ii. String 5pYw4R3 direplace dengan string [SPYWARE]
   iii. String R4nS0mWaR3 direplace dengan string [RANSOMWARE]
```
c. Program harus berjalan secara daemon, dan **tidak diperbolehkan menggunakan command system()** dalam pembuatan program

d. Program akan secara terus menerus berjalan di background dengan jeda **15 detik**

e. Catat setiap penghapusan string yang dilakukan oleh program pada sebuah file bernama **virus.log** dengan format: [dd-mm-YYYY][HH:MM:SS] Suspicious string at <file_name> successfully replaced!

Contoh penggunaan: ./virus /home/user/virus

Contoh isi file **sebelum** program dijalankan:
```
pU=-JWQ$5$)D-[??%AVh]$cB6bm4LwAr3jEQC2p3R{HV]=-AUaxj:Qe+h
!aNX,i:!z3W=2;.tHc3&S+}6F)CFf%tfZLP1*w5m1PAzZJUux(
Pd&f8$F5=E?@#[6jd{TJKj]5pYw4R3{KK1?hz384$ge@iba5GAj$gqB41
#C&&a}M9C#f64Eb.?%c)dGbCvJXtU[?SE4h]BY4e1PR4nS0mWaR3{]S/{w?*
```
Contoh isi file **setelah** setelah program dijalankan:
```
pU=-JWQ$5$)D-[??%AVh]$cB6b[MALWARE]jEQC2p3R{HV]=-AUaxj:Qe+h
!aNX,i:!z3W=2;.tHc3&S+}6F)CFf%tfZLP1*w5m1PAzZJUux(
Pd&f8$F5=E?@#[6jd{TJKj][SPYWARE]{KK1?hz384$ge@iba5GAj$gqB41
#C&&a}M9C#f64Eb.?%c)dGbCvJXtU[?SE4h]BY4e1P[RANSOMWARE]{]S/{w?*
```
## ***PENGERJAAN***

## ***PENJELASAN PENGERJAAN***

## ***Dokumentasi***

## ***SOAL 2 (Abhinaya)***
Paul adalah seorang mahasiswa semester 4 yang diterima magang di perusahaan XYZ. Pada hari pertama magang, ia diberi tugas oleh atasannya untuk membuat program manajemen file sederhana. Karena kurang terbiasa dengan bahasa C dan environment Linux, ia meminta bantuan kalian untuk mengembangkan program tersebut.

a. Atasannya meminta agar program tersebut dapat berjalan secara daemon dan dapat mengunduh serta melakukan unzip terhadap file berikut. Atasannya juga meminta program ini dibuat **tanpa menggunakan command system()**

b. Setelah ditelusuri, ternyata hanya 6 file teratas yang nama filenya tidak dienkripsi. Oleh karena itu, bantulah Paul untuk melakukan dekripsi terhadap nama file ke-7 hingga terakhir menggunakan algoritma ROT19

c. Setelah dekripsi selesai, akan terlihat bahwa setiap file memuat salah satu dari kode berikut: **r3N4mE, d3Let3, dan m0V3**. Untuk setiap file dengan nama yang memuat kode **d3Let3**, hapus file tersebut. Sementara itu, untuk setiap file dengan nama yang memuat kode **r3N4mE**, lakukan hal berikut:
```
  i. Jika ekstensi file adalah “.ts”, rename filenya menjadi “helper.ts”
  ii. Jika ekstensi file adalah “.py”, rename filenya menjadi “calculator.py”
  iii. Jika ekstensi file adalah “.go”, rename filenya menjadi “server.go”
  iv. Jika file tidak memuat salah satu dari ekstensi diatas, rename filenya menjadi “renamed.file”
```
d. Atasan Paul juga meminta agar program ini dapat membackup dan merestore file. Oleh karena itu, bantulah Paul untuk membuat program ini menjadi 3 mode, yaitu:
```
  i. default: program berjalan seperti biasa untuk me-rename dan menghapus file. Mode ini dieksekusi ketika program dijalankan tanpa argumen tambahan, yaitu dengan command ./management saja
  ii. backup: program memindahkan file dengan kode m0V3 ke sebuah folder bernama “backup”
  iii. restore: program mengembalikan file dengan kode m0V3 ke folder sebelum file tersebut dipindahkan
  iv. Contoh penggunaan: ./management -m backup
```
e. Terkadang, Paul perlu mengganti mode dari program ini tanpa menghentikannya terlebih dahulu. Oleh karena itu, bantulan Paul untuk mengintegrasikan kemampuan untuk mengganti mode ini dengan mengirim sinyal ke daemon, dengan ketentuan:
```
  i. SIGRTMIN untuk mode default
  ii. SIGUSR1 untuk mode backup
  iii. SIGUSR2 untuk mode restore
  iv. Contoh penggunaan: kill -SIGUSR2 <pid_program>
```
f. Program yang telah dibuat ini tidak mungkin akan dijalankan secara terus-menerus karena akan membebani sistem. Maka dari itu, bantulah Paul untuk membuat program ini dapat dimatikan dengan aman dan efisien

g. Terakhir, program ini harus berjalan setiap detik dan mampu mencatat setiap peristiwa yang terjadi ke dalam file .log yang bernama “history.log” dengan ketentuan:
```
  i. Format: [nama_user][HH:MM:SS] - <nama_file> - <action>
  ii. nama_user adalah username yang melakukan action terhadap file
  iii. Format action untuk setiap kode:
      1. kode r3N4mE: Successfully renamed.
      2. kode d3Let3: Successfully deleted.
      3. mode backup: Successfully moved to backup.
      4. mode restore: Successfully restored from backup.
  iv. Contoh pesan log:
      1. [paul][00:00:00] - r3N4mE.ts - Successfully renamed.
      2. [paul][00:00:00] - m0V3.xk1 - Successfully restored from backup.
```
h. Berikut adalah struktur folder untuk pengerjaan nomor 2:
```
    soal_2/
    ├── history.log
    ├── management.c
    └── library/
        └── backup/
```

## ***PENGERJAAN***

## ***PENJELASAN PENGERJAAN***

## ***Dokumentasi***

## ***SOAL 3 (Fidel)***
Pak Heze adalah seorang admin yang baik. Beliau ingin membuat sebuah program admin yang dapat memantau para pengguna sistemnya. Bantulah Pak Heze untuk membuat program  tersebut!

a. Namai program tersebut dengan nama **admin.c**

b. Program tersebut memiliki fitur menampilkan seluruh proses yang dilakukan oleh seorang user dengan menggunakan command:
```
admin <user>
```
c. Program dapat memantau proses apa saja yang dilakukan oleh user. Fitur ini membuat program berjalan secara daemon dan berjalan terus menerus. Untuk menjalankan fitur ini menggunakan command: 
```
admin -m <user>
```
Dan untuk mematikan fitur tersebut menggunakan: 
```
admin -s <user>
```
Program akan mencatat seluruh proses yang dijalankan oleh user di file <user>.log dengan format:
```
[dd:mm:yyyy]-[hh:mm:ss]-pid_process-nama_process_GAGAL/JALAN
```
D. Program dapat menggagalkan proses yang dijalankan user setiap detik dan berjalan terus menerus dengan menjalankan: 
```
admin -c user
```
sehingga user tidak bisa menjalankan proses yang dia inginkan dengan baik. Fitur ini dapat dimatikan dengan command:
```
admin -a user
```
E. Ketika proses yang dijalankan user digagalkan, program juga akan melog dan menset log tersebut sebagai **GAGAL**. Dan jika di log menggunakan fitur poin c, log akan ditulis dengan **JALAN**

## ***PENGERJAAN***

## ***PENJELASAN PENGERJAAN***

## ***Dokumentasi***


## ***SOAL 4 (Fidel)***
Salomo memiliki passion yang sangat dalam di bidang sistem operasi. Saat ini, dia ingin mengotomasi kegiatan-kegiatan yang ia lakukan agar dapat bekerja secara efisien. Bantulah Salomo untuk membuat program yang dapat mengotomasi kegiatan dia!

a. Salomo ingin dapat membuka berbagai macam aplikasi dengan banyak jendela aplikasi dalam satu command. Namai file program tersebut **setup.c**

b. Program dapat membuka berbagai macam aplikasi dan banyak jendela aplikasi sekaligus (bisa membuka 1 atau lebih aplikasi dengan 1 atau lebih window (kecuali aplikasi yang tidak bisa dibuka dengan banyak window seperti discord)) dengan menjalankan:
```
setup -o <app1> <num1> <app2> <num2>.....<appN> <numN>
```
Contoh penggunaannya adalah sebagai berikut: 
```
setup -o firefox 2 wireshark 2
```
Program akan membuka 2 jendela aplikasi firefox dan 2 jendela aplikasi wireshark.

c. Program juga dapat membuka aplikasi dengan menggunakan file konfigurasi dengan menggunakan 
```
setup -f file.conf
``` 
Format file konfigurasi dibebaskan, namun pastikan dapat menjalankan fitur dari poin 2.
Contoh isi file.conf:
```
Firefox 2
Wireshark 3
```
Ketika menjalankan command contoh, program akan membuka 2 jendela aplikasi firefox dan 3 jendela aplikasi wireshark.

d. Program dapat mematikan semua aplikasi yg dijalankan oleh program tersebut dengan: 
```
setup -k
```
e. Program juga dapat mematikan aplikasi yang dijalankan sesuai dengan file konfigurasi. 
Contohnya: 
```
setup -k file.conf 
```
Command ini hanya mematikan aplikasi yang dijalankan dengan 
```
setup -f file.conf
```
## ***PENGERJAAN***

## ***PENJELASAN PENGERJAAN***

## ***Dokumentasi***

NB: Semua soal tidak boleh dikerjakan menggunakan fungsi **system()**. Di utamakan menggunakan fungsi **fork()** atau **exec()**.
