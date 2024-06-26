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

## ***MODUL 2***
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
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <time.h>

void mengubah_karakter(char *filename) {
    FILE *file;
    char line[256]; // Array untuk menyimpan setiap baris
    char temp_file[256]; // File sementara untuk menyimpan hasil perubahan
    int masuk_log = 0; // Tanda untuk menandakan apakah ada string yang berhasil diganti di file tersebut

    // Membuat file sementara
    sprintf(temp_file, "%s.tmp", filename);
    file = fopen(temp_file, "w");
    if (file == NULL) {
        return;
    }

    // Membuka file yang akan diubah
    FILE *original_file = fopen(filename, "r");
    if (original_file == NULL) {
        fclose(file);
        return;
    }

    while (fgets(line, sizeof(line), original_file) != NULL) {
        char *kata = line;
        char *new_kata;

        while ((new_kata = strstr(kata, "m4LwAr3")) != NULL) {
            strncpy(new_kata, "[MALWARE]", strlen("[MALWARE]"));
            kata = new_kata + strlen("[MALWARE]");
            masuk_log = 1; // Set string ke 1 karena string berhasil diganti
        }

        kata = line;
        while ((new_kata = strstr(kata, "5pYw4R3")) != NULL) {
            strncpy(new_kata, "[SPYWARE]", strlen("[SPYWARE]"));
            kata = new_kata + strlen("[SPYWARE]");
            masuk_log = 1;
        }

        kata = line;
        while ((new_kata = strstr(kata, "R4nS0mWaR3")) != NULL) {
            strncpy(new_kata, "[RANSOMWARE]", strlen("[RANSOMWARE]"));
            kata = new_kata + strlen("[RANSOMWARE]");
            masuk_log = 1;
        }

        fputs(line, file); // Menulis baris yang telah diubah ke file sementara
    }

    fclose(original_file);
    fclose(file);

    // Mengganti file asli dengan file sementara
    remove(filename);
    rename(temp_file, filename);

    if (masuk_log) {
        FILE *log_file = fopen("home/ubuntu/SISOP/Modul2/soal_1/virus.log", "a");
        if (log_file != NULL) {
            time_t current_time = time(NULL);
            struct tm tm = *localtime(&current_time);
            fprintf(log_file, "[%02d-%02d-%04d][%02d:%02d:%02d] Suspicious string at %s successfully replaced!\n", tm.tm_mday, tm.tm_mon + 1, tm.tm_year + 1900, tm.tm_hour, tm.tm_min, tm.tm_sec, filename);
            fclose(log_file);
        }
    }

    // Reset nilai string ke 0 untuk penggunaan berikutnya
    masuk_log = 0;
}
void pencarian_direktori(char *directory_path) {
    DIR *dir;
    struct dirent *entry;

    dir = opendir(directory_path);
    if (dir == NULL) {
        return;
    }

    while ((entry = readdir(dir)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) {
            continue; // Skip directory 
        }

        // Skip file selain .txt
        size_t len = strlen(entry->d_name);
        if (len < 4 || strcmp(entry->d_name + len - 4, ".txt") != 0) {
            continue;
        }

        char full_path[strlen(directory_path) + strlen(entry->d_name) + 2];
        snprintf(full_path, sizeof(full_path), "%s/%s", directory_path, entry->d_name);

        FILE *file = fopen(full_path, "r");
        if (file != NULL) {
            mengubah_karakter(full_path); // memanggil funngsi ubah karakter 
            fclose(file);
        }
    }

    closedir(dir);
}

int main(int argc, char *argv[]) {
    pid_t pid, sid; // Variabel untuk menyimpan PID

    pid = fork(); // Menyimpan PID dari child Psoses 

    /* Keluar saat fork gagal
    * (nilai variabel pid < 0) */
    if (pid < 0) {
        exit(EXIT_FAILURE);
    }

    /* Keluar saat fork berhasil
    * (nilai variabel pid adalah PID dari child process) */
    if (pid > 0) {
        exit(EXIT_SUCCESS);
    }
   
    umask(0);

    sid = setsid();
    if (sid < 0) {
        exit(EXIT_FAILURE);
    }

    if ((chdir("/")) < 0) {
        exit(EXIT_FAILURE);
    }

    close(STDIN_FILENO);
    close(STDOUT_FILENO);
    close(STDERR_FILENO);

    while (1) {
        
        if (argc < 2) {
            exit(EXIT_FAILURE);
        }

        pencarian_direktori(argv[1]);

        sleep(15); // Jeda setiap 15 detik
    }

    exit(EXIT_SUCCESS);
}
```
## ***PENJELASAN PENGERJAAN***
#### Pertama-tama saya membuat sebuah fungsi **mengubah_karakter** yang berfungsi untuk membuka file, mencari string yang sama dengan yang diminta lalu mengubah string tersebut bila ada pada teks tersebut dan terakhir memasukkan ke dalam log setiap ada perubahan string. Pada fungsi inipun saya membuat indikator **masuk_log** dimana apabila terjadi perubahan string maka indikator masuk_log akan menjadi true dan sehingga nantinya akan dimasukkan ke log pada satu file maksimal satu kali saja masuk log dalam satu waktu.
```
void mengubah_karakter(char *filename) {
    FILE *file;
    char line[256]; // Array untuk menyimpan setiap baris
    char temp_file[256]; // File sementara untuk menyimpan hasil perubahan
    int masuk_log = 0; // Tanda untuk menandakan apakah ada string yang berhasil diganti di file tersebut

    // Membuat file sementara
    sprintf(temp_file, "%s.tmp", filename);
    file = fopen(temp_file, "w");
    if (file == NULL) {
        return;
    }

    // Membuka file yang akan diubah
    FILE *original_file = fopen(filename, "r");
    if (original_file == NULL) {
        fclose(file);
        return;
    }

    while (fgets(line, sizeof(line), original_file) != NULL) {
        char *kata = line;
        char *new_kata;

        while ((new_kata = strstr(kata, "m4LwAr3")) != NULL) {
            strncpy(new_kata, "[MALWARE]", strlen("[MALWARE]"));
            kata = new_kata + strlen("[MALWARE]");
            masuk_log = 1; // Set string ke 1 karena string berhasil diganti
        }

        kata = line;
        while ((new_kata = strstr(kata, "5pYw4R3")) != NULL) {
            strncpy(new_kata, "[SPYWARE]", strlen("[SPYWARE]"));
            kata = new_kata + strlen("[SPYWARE]");
            masuk_log = 1;
        }

        kata = line;
        while ((new_kata = strstr(kata, "R4nS0mWaR3")) != NULL) {
            strncpy(new_kata, "[RANSOMWARE]", strlen("[RANSOMWARE]"));
            kata = new_kata + strlen("[RANSOMWARE]");
            masuk_log = 1;
        }

        fputs(line, file); // Menulis baris yang telah diubah ke file sementara
    }

    fclose(original_file);
    fclose(file);

    // Mengganti file asli dengan file sementara
    remove(filename);
    rename(temp_file, filename);

    if (masuk_log) {
        FILE *log_file = fopen("home/ubuntu/SISOP/Modul2/soal_1/virus.log", "a");
        if (log_file != NULL) {
            time_t current_time = time(NULL);
            struct tm tm = *localtime(&current_time);
            fprintf(log_file, "[%02d-%02d-%04d][%02d:%02d:%02d] Suspicious string at %s successfully replaced!\n", tm.tm_mday, tm.tm_mon + 1, tm.tm_year + 1900, tm.tm_hour, tm.tm_min, tm.tm_sec, filename);
            fclose(log_file);
        }
    }

    // Reset nilai string ke 0 untuk penggunaan berikutnya
    masuk_log = 0;
}
```
Pada kode diatas dapat kita lihat alurnya dimana pertama membuat file sementara lalu membuka file dan kemudian mengecek apaakah aada kata-kata spesifik di dalam file tersebut bila ada maka akan menulis baris yang diubah ke file sementara lalu mengubah file sementara dengan file asli dengan tetap mempertahankan nama file yang sama lalu indikator masuk_log akan menjadi satu sehingga saat dbawah terdapat kode yang akan membuat laopran ke log apabila masuk_log adalah trus atau 1. dan setelah selesai menulis ke log makan indikator masuk_log akan diubah menjadi 0 lagi untuk digunakan saat mengecek file lainnya. 

#### Pada fungsi yang kedua adalah pencarian_direktori dimana Fungsi ini bertugas untuk mencari file dengan ekstensi **.txt** pada suatu direktori dan memanggil fungsi mengubah_karakter() untuk setiap file.txt yang ditemukan. 
```
void pencarian_direktori(char *directory_path) {
    DIR *dir;
    struct dirent *entry;

    dir = opendir(directory_path);
    if (dir == NULL) {
        return;
    }

    while ((entry = readdir(dir)) != NULL) {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) {
            continue; // Skip directory 
        }

        // Skip file selain .txt
        size_t len = strlen(entry->d_name);
        if (len < 4 || strcmp(entry->d_name + len - 4, ".txt") != 0) {
            continue;
        }

        char full_path[strlen(directory_path) + strlen(entry->d_name) + 2];
        snprintf(full_path, sizeof(full_path), "%s/%s", directory_path, entry->d_name);

        FILE *file = fopen(full_path, "r");
        if (file != NULL) {
            mengubah_karakter(full_path); // memanggil funngsi ubah karakter 
            fclose(file);
        }
    }

    closedir(dir);
}
```
Pada fungsi ini pertama-tama menerima path direktori dari user, setelah itu membuka path direktori yang telah ditentukan sebelumnya dan saat masuk direktori akan melewati entri . dan .. dan juga akan menskip file yang selain .txt, dan saat menemukan file .txt akan memanggil fungsi yang sebelumnya yaitu fungsi mengubah_karakter dan ini akan diulangi secara terus menerus hingga semua file yang ada di diectory tersebut sudah diperiksa dan yang terakhir adalah menutup directorynya. 

#### Terakhir adalah memuat mainnya, main merukan fungsi utama yang akan dijalankan oleh program, di main akan menerika path directory dan dijalankan secara daemon. 
```
int main(int argc, char *argv[]) {
    pid_t pid, sid; // Variabel untuk menyimpan PID

    pid = fork(); // Menyimpan PID dari child Psoses 

    /* Keluar saat fork gagal
    * (nilai variabel pid < 0) */
    if (pid < 0) {
        exit(EXIT_FAILURE);
    }

    /* Keluar saat fork berhasil
    * (nilai variabel pid adalah PID dari child process) */
    if (pid > 0) {
        exit(EXIT_SUCCESS);
    }
   
    umask(0);

    sid = setsid();
    if (sid < 0) {
        exit(EXIT_FAILURE);
    }

    if ((chdir("/")) < 0) {
        exit(EXIT_FAILURE);
    }

    close(STDIN_FILENO);
    close(STDOUT_FILENO);
    close(STDERR_FILENO);

    while (1) {
        
        if (argc < 2) {
            exit(EXIT_FAILURE);
        }

        pencarian_direktori(argv[1]);

        sleep(15); // Jeda setiap 15 detik
    }

    exit(EXIT_SUCCESS);
}
```
Pada fungsi main diatas dapat kita lihat pertama-tama kita akan menggunakan argv untuk mendapatkan inputan sebuah path kemudian membuat program berjalan secara daemon dengan mengfork lalu membunuh parentnya lalu saat path yang diberikan ada, maka akan memanggil fungsi pencarian_direktori dan kemudian akan keluar dari program. 

#### Kesimpulan dari program ini adalah pertama kita menjadi program sebagai daemon lalu mendapatkan inputan dari user terkait dengan path lalu bila path tersebut ada maka menjalankan fungsi pencarian_direktori dan saat pencarian_direktori terdapat file .txt maka akan memanggil fungsi mengubah_karakter dan saat ada sebuah string yang sesuai dengan diperintahkan maka string tersebut akan diganti dan aktifitas menggantinya akan di catat pada log. 


Command yang digunakan dalam pengerjaan ini adalah : 
```
1. sprintf(temp_file, "%s.tmp", filename): Menggabungkan string filename dengan ekstensi .tmp dan menyimpan hasilnya di temp_file.
2. fopen(filename, "w"): Membuka file filename untuk ditulis.
3. fgets(line, sizeof(line), original_file): Membaca satu baris dari file original_file dan menyimpannya di dalam buffer line.
4. strstr(kata, "m4LwAr3"), strstr(kata, "5pYw4R3"), strstr(kata, "R4nS0mWaR3"): Mencari substring dalam string kata yang sesuai dengan pola yang ditentukan.
5. strncpy(new_kata, "[MALWARE]", strlen("[MALWARE]")), strncpy(new_kata, "[SPYWARE]", strlen("[SPYWARE]")), strncpy(new_kata, "[RANSOMWARE]", strlen("[RANSOMWARE]")): Mengganti substring yang ditemukan dengan label yang sesuai.
6. fputs(line, file): Menulis satu baris ke dalam file file.
7. remove(filename): Menghapus file filename.
8. rename(temp_file, filename): Mengubah nama file sementara temp_file menjadi filename.
9. time(NULL): Mendapatkan waktu saat ini dalam bentuk timestamp.
10. localtime(&current_time): Mengonversi timestamp menjadi struktur tm yang berisi informasi waktu lokal.
11. fprintf(log_file, "[%02d-%02d-%04d][%02d:%02d:%02d] Suspicious string at %s successfully replaced!\n", tm.tm_mday, tm.tm_mon + 1, tm.tm_year + 1900, tm.tm_hour, tm.tm_min, tm.tm_sec, filename): Mencetak pesan ke dalam file log dengan format yang telah ditentukan.
12. opendir(directory_path): Membuka direktori yang ditentukan.
13. readdir(dir): Membaca entri dari direktori yang telah dibuka.
14. closedir(dir): Menutup direktori yang telah dibuka.
15. pid = fork(): Membuat proses baru.
16. setsid(): Mengatur session ID.
17. umask(0): Mengatur file creation mask.
18. chdir("/"): Mengubah working directory.
19. close(STDIN_FILENO), close(STDOUT_FILENO), berfungsi untuk menutup file-file. 
20. sleep(15): Menunda eksekusi program selama 15 detik.
```
#### Error yang pernah terjadi adalah bila terdapat lebih dari 1 string yang diminta diubah dalam satu file maka akan mengubah salah satunya saja, dan hal ini dapat fiks dengan membuat file sementara terlebih dahulu dan tidak langsung mengubah di file aslinya. 
#### Error kedua yang pernah dialami adalah saat memasukkan ke log dengan cara satu-satu dimana setiap kali string ada yang diganti akan dimasukkan ke log sehingga bila ada beberapa string yang harus diganti lognya akan beberapa kali juga bukan hanya satu kali dan masalah ini di fiks dengan membuat indikator masuk_log

## ***Dokumentasi***
![Modul2(6)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150375098/d4c8db9e-1eee-4d8c-9b57-9e6e69a60dc1)
![Dokum Modul2(4)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150375098/c416cb55-54ce-426c-86dd-d5e031544c50)
![Dokum modul 2(1)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150375098/cbfd84d6-3bdb-4a51-ba01-eca350e1dbf9)
![Dokum modul2(3)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150375098/895db0c0-8b23-4ee7-954d-e56741ce4a31)
![Dokum Modul2(2)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150375098/c00c5861-ecb7-4494-b2b6-8a5b06b11365)
![Dokum modul2](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150375098/71b3894c-e0c3-47d5-93db-7de0139c8b0e)

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
```
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <errno.h>
#include <syslog.h>
#include <fcntl.h>

void rename_decrypt(){

  return;
};

void rename_code(){

  return;
}

void default_mode(){

  return;
}

void restore_mode(){

  return;
}

void backup_ mode(){

  return;
}

void signal_handling(){

  return;
} 

void log_history(){

  return;
}


int main(int argc, char *argv[]){

  pid_t pid, sid;        // Variabel untuk menyimpan PID

  pid = fork();     // Menyimpan PID dari Child Process

  /* Keluar saat fork gagal
  * (nilai variabel pid < 0) */
  if (pid < 0) {
    exit(EXIT_FAILURE);
  }

  /* Keluar saat fork berhasil
  * (nilai variabel pid adalah PID dari child process) */
  if (pid > 0) {
    exit(EXIT_SUCCESS);
  }

  umask(0);

  sid = setsid();
  if (sid < 0) {
    exit(EXIT_FAILURE);
  }

  close(STDIN_FILENO);
  //close(STDOUT_FILENO);
  //close(STDERR_FILENO);

  // program disini

    chdir("/home/abhi/sisop_proc");

    int id_download = fork();

    if (id_download == 0){
        execlp("wget", "wget", "-q", "--no-check-certificate", "https://docs.google.com/uc?export=download&id=1rUIZmp10lXLtCIH3LAZJzRPeRks3Crup", "-O", "library.zip", NULL);
    } else {
        wait(NULL);
        int id_unzip = fork();
        if (id_unzip == 0){
            execlp("unzip", "unzip", "-q", "library.zip", NULL);    
        } else {
            wait(NULL);
        }
    }


  /*
  program tersebut belum menyelesaikan tahap dekripsi menggunakan ROT19, program ini juga belum menyelesaikan tahap merename sesuai kode nama filenya
  dan menghapus sesuai kode nama filenya, tahap backup dan restore file belum selesai, tahap mengganti mode sesuai signal belum selesai,
  belum menyelesaikan code untuk dimatikan secara aman, dan pencatatan log file belum selesai
  */


    return 0;
}
```

## ***PENJELASAN PENGERJAAN***
```
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <errno.h>
#include <syslog.h>
#include <fcntl.h>

int main(int argc, char *argv[]){

  pid_t pid, sid;        // Variabel untuk menyimpan PID

  pid = fork();     // Menyimpan PID dari Child Process

  /* Keluar saat fork gagal
  * (nilai variabel pid < 0) */
  if (pid < 0) {
    exit(EXIT_FAILURE);
  }

  /* Keluar saat fork berhasil
  * (nilai variabel pid adalah PID dari child process) */
  if (pid > 0) {
    exit(EXIT_SUCCESS);
  }

  umask(0);

  sid = setsid();
  if (sid < 0) {
    exit(EXIT_FAILURE);
  }

  close(STDIN_FILENO);
  //close(STDOUT_FILENO);
  //close(STDERR_FILENO);
```
Pada bagian ini adalah membuat program dapat berjalan di latar belakang dan membuat program tidak dapat berinteraksi dengan user
hal ini bisa disebut bagian ini membuat program tersebut menjadi program daemon
```
    chdir("/home/abhi/sisop_proc");

    int id_download = fork();

    if (id_download == 0){
        execlp("wget", "wget", "-q", "--no-check-certificate", "https://docs.google.com/uc?export=download&id=1rUIZmp10lXLtCIH3LAZJzRPeRks3Crup", "-O", "library.zip", NULL);
    } else {
        wait(NULL);
        int id_unzip = fork();
        if (id_unzip == 0){
            execlp("unzip", "unzip", "-q", "library.zip", NULL);    
        } else {
            wait(NULL);
        }
    }


    return 0;
}
```
Pada bagian tersebut merupakan pengerjaain untuk nomor bagian yang a, yaitu mendownload sebuah file yang ada di google drive lalu
mengunzip file tersebut. Hal ini dapat saya lakukan dengan menggunakan fork dan execlop, saya membuat dua process (child dan parent process) agar execlp dapat berjalan dengan baik, jika tidak execlp akan menjalankan script bash lalu akan langsung keluar,
cara membuat execlp dapat berjalan dengan baik adalah dengan membuat child process dan parent process dan jangan lupa kita beri fungsi wait() di parent process agar tidak terjadi overlapping.

Untuk soal-soal setelahnya saya tidak dapat menjawab soal-soal tersebut dikarenakan saya mengalami sakit di saat praktikum berlangsung, dan saya hanya dapat mengerjakan soal bagaian yang a.

## ***Dokumentasi***
![Screenshot from 2024-04-27 19-59-44](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/123524655/a2d534ab-aa04-4a1b-b36e-706aaf0f44c4)
![Screenshot from 2024-04-27 20-00-03](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/123524655/8753e85d-b5ba-4d24-bc7b-bb688a729f1b)

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
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <errno.h>
#include <time.h>
#include <signal.h>
#include <sys/inotify.h>

// Variabel flag global 
int running_flag = 1;
// Fungsi untuk menulis ke log
void write_to_log(const char *username, const char *activity, int status) {
    char log_filename[100];
    sprintf(log_filename, "%s.log", username);
    FILE *log_file = fopen(log_filename, "a");
    if (log_file == NULL) {
        perror("Error opening log file");
        return;
    }
    time_t now;
    struct tm *local_time;
    time(&now);
    local_time = localtime(&now);

    fprintf(log_file, "[%02d:%02d:%04d]-[%02d:%02d:%02d]-%d-%s %s\n",
            local_time->tm_mday, local_time->tm_mon + 1, local_time->tm_year + 1900,
            local_time->tm_hour, local_time->tm_min, local_time->tm_sec,
            getpid(), activity, status ? "JALAN" : "GAGAL");

    fclose(log_file);
}
// Fungsi untuk mengirim sinyal SIGTERM ke proses 
void stop_monitoring(const char *username) {
    char log_message[100];
    sprintf(log_message, "Menghentikan pemantauan untuk pengguna %s", username);
    write_to_log(username, log_message, 1);
    // Baca PID dari file log
    char log_filename[100];
    sprintf(log_filename, "%s.log", username);
    FILE *log_file = fopen(log_filename, "r");
    if (log_file == NULL) {
        perror("Error opening log file");
        return;
    }
    int pid;
    if (fscanf(log_file, "[%*d:%*d:%*d]-[%*d:%*d:%*d]-%d", &pid) != 1) {
        perror("Error reading PID from log file");
        fclose(log_file);
        return;
    }
    fclose(log_file);
    // Kirim sinyal SIGTERM ke proses 
    if (kill(pid, SIGTERM) == -1) {
        perror("Error sending SIGTERM signal");
        return;
    }
}
// Fungsi untuk memonitor segala aktivitas yang dilakukan oleh pengguna
void monitor_activities(const char *username) {
    // Membuat handle untuk memantau direktori home pengguna
    int inotify_fd = inotify_init();
    if (inotify_fd == -1) {
        perror("Failed to initialize inotify");
        return;
    }
    const char *user_home = getenv("HOME");
    int wd = inotify_add_watch(inotify_fd, user_home, IN_ALL_EVENTS);
    if (wd == -1) {
        perror("Failed to add watch to user's home directory");
        close(inotify_fd);
        return;
    }
    // Membuat buffer untuk menampung event inotify
    char event_buf[4096];
    ssize_t num_bytes;
    printf("Memulai pemantauan untuk pengguna %s\n", username);
    while (1) {
        // Membaca event inotify
        num_bytes = read(inotify_fd, event_buf, sizeof(event_buf));
        if (num_bytes <= 0) {
            perror("Error reading inotify events");
            break;
        }
        // Mengolah setiap event
        int i = 0;
        while (i < num_bytes) {
            struct inotify_event *event = (struct inotify_event *) &event_buf[i];
            if (event->len > 0) {
                // Mencatat aktivitas ke file log
                write_to_log(username, event->name, 1); // 1 untuk Jalan
                printf("Aktivitas: %s. PID: %d\n", event->name, getpid());
            }
            i += sizeof(struct inotify_event) + event->len;
        }
    }
    // Menghapus watch dan menutup handle inotify
    inotify_rm_watch(inotify_fd, wd);
    close(inotify_fd);
}
// Fungsi menggagalkan kegiatan pengguna
void fail_activities(const char *username) {
    running_flag = 0;
    write_to_log(username, "Gagalkan aktivitas", 0); // 0 Untuk Gagal
}
// Fungsi mengaktifkan kembali kegiatan pengguna
void activate_activities(const char *username) {
    running_flag = 1;
    write_to_log(username, "Aktifkan kembali kegiatan", 1); // 1 Untuk Jalan
}
int main(int argc, char *argv[]) {
    if (argc < 3) {
        fprintf(stderr, "Usage: %s [-m | -s | -c | -a] <username>\n", argv[0]);
        return 1;
    }
    const char *username = argv[2];
    if (strcmp(argv[1], "-m") == 0) {
        printf("Memulai pemantauan untuk pengguna %s\n", username);
        // Jalankan fitur daemon
        pid_t pid = fork();
        if (pid < 0) {
            perror("Error forking");
            exit(EXIT_FAILURE);
        }
        if (pid > 0) {
            printf("Pemantauan dimulai. PID proses pemantauan: %d\n", pid);
            exit(EXIT_SUCCESS); // Keluar parent process
        }
        // Child process jadi leader
        if (setsid() < 0) {
            perror("Error creating new session");
            exit(EXIT_FAILURE);
        }
        close(STDIN_FILENO);
        close(STDOUT_FILENO);
        close(STDERR_FILENO);
        // Pemilihan proses pengguna
        monitor_activities(username);
    } else if (strcmp(argv[1], "-s") == 0) {
        printf("Menghentikan pemantauan untuk pengguna %s\n", username);
        // Mematikan fitur daemon
        stop_monitoring(username);
    } else if (strcmp(argv[1], "-c") == 0) {
        printf("Menggagalkan kegiatan pengguna %s\n", username);
        // Menggagalkan kegiatan pengguna
        fail_activities(username);
    } else if (strcmp(argv[1], "-a") == 0) {
        printf("Mengaktifkan kembali kegiatan pengguna %s\n", username);
        // Mengaktifkan kembali kegiatan pengguna
        activate_activities(username);
    } else {
        fprintf(stderr, "Invalid option\n");
        return 1;
    }
return 0;
}
```
## ***PENJELASAN PENGERJAAN***
Library yang Digunakan:

- stdio.h: untuk fungsi input dan output bahasa c
- stdlib.h: untuk alokasi memori dinamis, pengelolaan proses, dan fungsi-fungsi umum lainnya, seperti malloc() dan exit().
- unistd.h: untuk fungsi-fungsi sistem POSIX, seperti fork(), setsid(), dan close().
- string.h: untuk fungsi-fungsi pengolahan string, seperti strcmp() dan sprintf().
- sys/types.h: berisi definisi-definisi tipe data sistem, seperti pid_t.
- sys/stat.h: berisi fungsi-fungsi untuk pengolahan informasi status file.
- fcntl.h: berisi definisi-definisi untuk pengelolaan file, seperti open() dan fcntl().
- errno.h: berisi definisi kode error untuk menyimpan kode error yang dihasilkan oleh fungsi-fungsi sistem.
- time.h: berisi fungsi-fungsi untuk pengolahan waktu, seperti time() dan localtime().
- signal.h: berisi definisi-definisi dan konstanta-konstanta yang digunakan dalam pemrograman dengan sinyal.
- sys/inotify.h: berisi deklarasi-deklarasi untuk fungsi-fungsi yang digunakan untuk memantau perubahan dalam sistem file menggunakan inotify.
```
int running_flag = 1;
```
- Variabel ini digunakan untuk mengontrol status pemantauan. Jika bernilai 1, pemantauan akan berjalan. Jika bernilai 0, pemantauan akan dihentikan.
```
void write_to_log(const char *username, const char *activity, int status) {
    char log_filename[100];
    sprintf(log_filename, "%s.log", username);
    FILE *log_file = fopen(log_filename, "a");
    if (log_file == NULL) {
        perror("Error opening log file");
        return;
    }
    time_t now;
    struct tm *local_time;
    time(&now);
    local_time = localtime(&now);

    fprintf(log_file, "[%02d:%02d:%04d]-[%02d:%02d:%02d]-%d-%s %s\n",
            local_time->tm_mday, local_time->tm_mon + 1, local_time->tm_year + 1900,
            local_time->tm_hour, local_time->tm_min, local_time->tm_sec,
            getpid(), activity, status ? "JALAN" : "GAGAL");

    fclose(log_file);
}
```
- Fungsi ini digunakan untuk menulis kegiatan pengguna ke file log. Parameter yang diterima adalah username pengguna, aktivitas yang dilakukan, dan status aktivitas (jalan atau gagal).
```
void stop_monitoring(const char *username) {
    char log_message[100];
    sprintf(log_message, "Menghentikan pemantauan untuk pengguna %s", username);
    write_to_log(username, log_message, 1);
    // Baca PID dari file log
    char log_filename[100];
    sprintf(log_filename, "%s.log", username);
    FILE *log_file = fopen(log_filename, "r");
    if (log_file == NULL) {
        perror("Error opening log file");
        return;
    }
    int pid;
    if (fscanf(log_file, "[%*d:%*d:%*d]-[%*d:%*d:%*d]-%d", &pid) != 1) {
        perror("Error reading PID from log file");
        fclose(log_file);
        return;
    }
    fclose(log_file);
    // Kirim sinyal SIGTERM ke proses 
    if (kill(pid, SIGTERM) == -1) {
        perror("Error sending SIGTERM signal");
        return;
    }
}
```
- Fungsi ini bertanggung jawab untuk menghentikan pemantauan yang sedang berjalan untuk pengguna tertentu. Pertama, fungsi ini membaca PID proses pemantauan dari file log pengguna. Kemudian, fungsi ini mengirim sinyal SIGTERM ke proses pemantauan untuk menghentikannya.
```
void monitor_activities(const char *username) {
    // Membuat handle untuk memantau direktori home pengguna
    int inotify_fd = inotify_init();
    if (inotify_fd == -1) {
        perror("Failed to initialize inotify");
        return;
    }
    const char *user_home = getenv("HOME");
    int wd = inotify_add_watch(inotify_fd, user_home, IN_ALL_EVENTS);
    if (wd == -1) {
        perror("Failed to add watch to user's home directory");
        close(inotify_fd);
        return;
    }
    // Membuat buffer untuk menampung event inotify
    char event_buf[4096];
    ssize_t num_bytes;
    printf("Memulai pemantauan untuk pengguna %s\n", username);
    while (1) {
        // Membaca event inotify
        num_bytes = read(inotify_fd, event_buf, sizeof(event_buf));
        if (num_bytes <= 0) {
            perror("Error reading inotify events");
            break;
        }
        // Mengolah setiap event
        int i = 0;
        while (i < num_bytes) {
            struct inotify_event *event = (struct inotify_event *) &event_buf[i];
            if (event->len > 0) {
                // Mencatat aktivitas ke file log
                write_to_log(username, event->name, 1); // 1 untuk Jalan
                printf("Aktivitas: %s. PID: %d\n", event->name, getpid());
            }
            i += sizeof(struct inotify_event) + event->len;
        }
    }
    // Menghapus watch dan menutup handle inotify
    inotify_rm_watch(inotify_fd, wd);
    close(inotify_fd);
}
```
- Fungsi ini melakukan pemantauan aktivitas pengguna dalam direktori home mereka menggunakan inotify. Setiap kali ada perubahan dalam direktori home pengguna, seperti pembuatan atau penghapusan file, aktivitas tersebut dicatat ke file log.
```
// Fungsi menggagalkan kegiatan pengguna
void fail_activities(const char *username) {
    running_flag = 0;
    write_to_log(username, "Gagalkan aktivitas", 0); // 0 Untuk Gagal
}
// Fungsi mengaktifkan kembali kegiatan pengguna
void activate_activities(const char *username) {
    running_flag = 1;
    write_to_log(username, "Aktifkan kembali kegiatan", 1); // 1 Untuk Jalan
}
```
- Fungsi ini digunakan untuk mengaktifkan kembali kegiatan pengguna yang sebelumnya telah digagalkan. Saat dipanggil, variabel running_flag diatur kembali menjadi 1, sehingga pemantauan dapat dilanjutkan.
- Fungsi ini digunakan untuk menggagalkan kegiatan pengguna. Saat dipanggil, variabel running_flag diatur menjadi 0, menandakan bahwa pemantauan akan dihentikan.
```
int main(int argc, char *argv[]) {
    if (argc < 3) {
        fprintf(stderr, "Usage: %s [-m | -s | -c | -a] <username>\n", argv[0]);
        return 1;
    }
    const char *username = argv[2];
    if (strcmp(argv[1], "-m") == 0) {
        printf("Memulai pemantauan untuk pengguna %s\n", username);
        // Jalankan fitur daemon
        pid_t pid = fork();
        if (pid < 0) {
            perror("Error forking");
            exit(EXIT_FAILURE);
        }
        if (pid > 0) {
            printf("Pemantauan dimulai. PID proses pemantauan: %d\n", pid);
            exit(EXIT_SUCCESS); // Keluar parent process
        }
        // Child process jadi leader
        if (setsid() < 0) {
            perror("Error creating new session");
            exit(EXIT_FAILURE);
        }
        close(STDIN_FILENO);
        close(STDOUT_FILENO);
        close(STDERR_FILENO);
        // Pemilihan proses pengguna
        monitor_activities(username);
    } else if (strcmp(argv[1], "-s") == 0) {
        printf("Menghentikan pemantauan untuk pengguna %s\n", username);
        // Mematikan fitur daemon
        stop_monitoring(username);
    } else if (strcmp(argv[1], "-c") == 0) {
        printf("Menggagalkan kegiatan pengguna %s\n", username);
        // Menggagalkan kegiatan pengguna
        fail_activities(username);
    } else if (strcmp(argv[1], "-a") == 0) {
        printf("Mengaktifkan kembali kegiatan pengguna %s\n", username);
        // Mengaktifkan kembali kegiatan pengguna
        activate_activities(username);
    } else {
        fprintf(stderr, "Invalid option\n");
        return 1;
    }
return 0;
}
```
- Fungsi main adalah titik masuk utama program. Program ini menerima argumen dari baris perintah yang menentukan tindakan apa yang harus dilakukan (memulai, menghentikan, menggagalkan, atau mengaktifkan kembali pemantauan) dan pengguna mana yang akan dipantau. Berdasarkan argumen yang diberikan, program akan menjalankan fungsi yang sesuai.

## ***REVISI***
Penambahan fungsi baru untuk membuka dan memblokir ping saat membuka aplikasi 
```
// Fungsi memblokir ping
void block_ping() {
    system("sudo iptables -A OUTPUT -p icmp --icmp-type echo-request -j DROP");
    printf("Ping telah diblokir\n");
}

// Fungsi membuka blokir ping
void unblock_ping() {
    system("sudo iptables -D OUTPUT -p icmp --icmp-type echo-request -j DROP");
    printf("Ping telah dibuka\n");
}
```
- Kedua fungsi yang ditambahkan diatas akan membuka dan meblokir ping, kita dapat mengeceknya menggunakan ping (aplikasi)
```
    } else if (strcmp(argv[1], "-c") == 0) {
        printf("Menggagalkan kegiatan pengguna %s\n", username);
        // Menggagalkan kegiatan pengguna
        fail_activities(username);
        // Blokir ping
        block_ping();
    } else if (strcmp(argv[1], "-a") == 0) {
        printf("Mengaktifkan kembali kegiatan pengguna %s\n", username);
        // Mengaktifkan kembali kegiatan pengguna
        activate_activities(username);
        // Buka blokir ping
        unblock_ping();
    } else {
        fprintf(stderr, "Invalid option\n");
        return 1;
    }
    return 0;
}
```
- memanggil fungsi void yang untuk membuka dan memblokir ping dengan memasukkan ke else if untuk -c dan -a

## ***Dokumentasi***
![Screenshot (345)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/4b69ccaa-8159-4ee3-98cc-3591eef73b3f)
![Screenshot (344)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/b006bb04-4386-4516-b43e-e488c476372b)
![Screenshot (346)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/ab9ffe1f-b423-4c29-87d9-df531fe79964)
![Screenshot (347)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/273edf27-6851-4ce3-8014-c696066d1df5)
![Screenshot (348)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/85aa434b-594b-4839-b72d-75664087b7e1)
![Screenshot (349)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/baa7ddb5-6de7-46b5-ad39-9510df01948a)
![Screenshot (350)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/8ee24d49-ba46-46c7-9c0e-2516a035a712)

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
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <string.h>
#include <signal.h>

// Prototipe fungsi
void save_running_app(pid_t pid);
// Fungsi membuka aplikasi
void open_app(char *app_name, int num_windows, pid_t *pids, int *pid_count) {
    for (int i = 0; i < num_windows; i++) {
        pid_t pid = fork();
        if (pid == 0) {
            // Child proses, ngejalanin aplikasi
            execlp(app_name, app_name, NULL);
            perror("execlp");
            exit(1);
        } else if (pid < 0) {
            perror("fork");
            exit(1);
        } else {
            // Simpan PID kedalam file log
            pids[*pid_count] = pid;
            (*pid_count)++;
            usleep(10000); 
            save_running_app(pid);
        }
    }
}
// Fungsi membaca konfigurasi dari file
void read_config_file(char *file_name, pid_t *pids, int *pid_count) {
    FILE *file = fopen(file_name, "r");
    if (file == NULL) {
        perror("fopen");
        exit(1);
    }
    char app_name[256];
    int num_windows;
    while (fscanf(file, "%s %d", app_name, &num_windows) != EOF) {
        open_app(app_name, num_windows, pids, pid_count);
    }
    fclose(file);
}
// Fungsi mematikan aplikasi berdasarkan PID
void kill_apps(pid_t *pids, int pid_count) {
    if (pid_count == 0) {
        printf("Tidak ada aplikasi.\n");
        return;
    }    
    for (int i = 0; i < pid_count; i++) {
        kill(pids[i], SIGTERM);
    }   
    // Menunggu child proses selesai
    while (wait(NULL) > 0);
}
// Fungsi menyimpan PID aplikasi yang di run kedalam file log
void save_running_app(pid_t pid) {
    FILE *log_file = fopen("Berjalan", "a");
    if (log_file == NULL) {
        perror("Error opening log file");
        exit(1);
    }
    fprintf(log_file, "%d\n", pid);
    fclose(log_file);
}
// Fungsi membaca PID aplikasi dari file log dan melakukan kill
void kill_apps_from_log() {
    FILE *log_file = fopen("Berjalan", "r");
    if (log_file == NULL) {
        printf("Tidak ada aplikasi.\n");
        return;
    }
    pid_t pid;
    while (fscanf(log_file, "%d", &pid) != EOF) {
        if (kill(pid, SIGTERM) == -1) {
            perror("Error killing process");
        } else {
            printf("Killed process with PID: %d\n", pid);
        }
    }
    fclose(log_file);
    remove("berjalan"); // Hapus file log setelah selesai melakukan kill
}
// Fungsi Main untuk memilih opsi program yang dijalankan
int main(int argc, char *argv[]) {
    pid_t pids[100]; 
    int pid_count = 0;

    // Menghilangkan peringatan Gtk
    putenv("GTK_MODULES=");

    if (argc < 2) {
        printf("Usage: setup -o <app1> <num1> <app2> <num2>... or setup -f <file.conf> or setup -k\n");
        return 1;
    }
    if (strcmp(argv[1], "-o") == 0) {
        // Membuka aplikasi dengan perintah
        for (int i = 2; i < argc; i += 2) {
            int num_windows = atoi(argv[i + 1]);
            open_app(argv[i], num_windows, pids, &pid_count);
        }
    } else if (strcmp(argv[1], "-f") == 0) {
        // Membaca konfigurasi dari file
        if (argc != 3) {
            printf("Usage: setup -f <file.conf>\n");
            return 1;
        }
        read_config_file(argv[2], pids, &pid_count);
    } else if (strcmp(argv[1], "-k") == 0) {
        // Mematikan aplikasi
        kill_apps(pids, pid_count);
        // Menjalankan fungsi untuk melakukan kill berdasarkan file log
        kill_apps_from_log();
    } else {
        printf("Invalid option\n");
        return 1;
    }
return 0;
}
```
## ***PENJELASAN PENGERJAAN***

*Salomo ingin dapat membuka berbagai macam aplikasi dengan banyak jendela aplikasi dalam satu command. Namai file program tersebut setup.c*

Pertama mkdir untuk membuat directory, disini saya menamai direktori dengan soal4, kemudian nano untuk membuat text book yang dinamai dengan setup.c sesuai dengan keterangan soal `mkdir soal4` `nano setup.c`

*Program C sesuai yang diminta di soal beserta penjelasan fungsi fungsi pada programnya*

Buat untuk `Library` program C

- #include <stdio.h> = untuk input-output program c

- #include <stdlib.h> = untuk fungsi-fungsi umum seperti alokasi memori dan konversi

- #include <unistd.h> = untuk system call POSIX 

- #include <sys/types.h> = untuk tipe data khusus sistem

- #include <sys/wait.h> =  untuk fungsi-fungsi yang berhubungan dengan penungguan proses

- #include <string.h> =  untuk manipulasi string

- #include <signal.h> = untuk menangani sinyal dan pengontrolan proses
```
void save_running_app(pid_t pid)
```
- Fungsi save_running_app(pid_t pid) berfungsi untuk menyimpan PID aplikasi yang sedang berjalan ke dalam file log
```
void open_app(char *app_name, int num_windows, pid_t *pids, int *pid_count) {
    for (int i = 0; i < num_windows; i++) {
        pid_t pid = fork();
        if (pid == 0) {
            // Child proses, ngejalanin aplikasi
            execlp(app_name, app_name, NULL);
            perror("execlp");
            exit(1);
        } else if (pid < 0) {
            perror("fork");
            exit(1);
        } else {
            // Simpan PID kedalam file log
            pids[*pid_count] = pid;
            (*pid_count)++;
            usleep(10000); 
            save_running_app(pid);
        }
    }
}
```
- Fungsi ini digunakan untuk membuka aplikasi. Menerima nama aplikasi, jumlah jendela, array PID, dan jumlah PID yang digunakan sebagai argumen.
- Menggunakan fork() untuk membuat proses baru dan execlp() untuk menjalankan aplikasi.
- Setelah menjalankan aplikasi, PID disimpan dan ditambahkan ke file log.
```
void read_config_file(char *file_name, pid_t *pids, int *pid_count) {
    FILE *file = fopen(file_name, "r");
    if (file == NULL) {
        perror("fopen");
        exit(1);
    }
    char app_name[256];
    int num_windows;
    while (fscanf(file, "%s %d", app_name, &num_windows) != EOF) {
        open_app(app_name, num_windows, pids, pid_count);
    }
    fclose(file);
}
```
- Fungsi ini membaca konfigurasi dari file.
- Menerima nama file, array PID, dan jumlah PID sebagai argumen.
- Setiap baris file membaca nama aplikasi dan jumlah jendela, dan kemudian memanggil open_app().
```
void kill_apps(pid_t *pids, int pid_count) {
    if (pid_count == 0) {
        printf("Tidak ada aplikasi.\n");
        return;
    }    
    for (int i = 0; i < pid_count; i++) {
        kill(pids[i], SIGTERM);
    }   
    // Menunggu child proses selesai
    while (wait(NULL) > 0);
}
```
- Fungsi ini digunakan untuk mematikan aplikasi berdasarkan PID.
- Menerima array PID dan jumlah PID sebagai argumen.
- Mengirimkan sinyal SIGTERM ke setiap PID yang disimpan.
```
void save_running_app(pid_t pid) {
    FILE *log_file = fopen("Berjalan", "a");
    if (log_file == NULL) {
        perror("Error opening log file");
        exit(1);
    }
    fprintf(log_file, "%d\n", pid);
    fclose(log_file);
}
```
- Fungsi ini menyimpan PID aplikasi yang dijalankan ke dalam file log.
- Menerima PID aplikasi sebagai argumen.
- Membuka file log, menulis PID, dan menutup file log.
```
void kill_apps_from_log() {
    FILE *log_file = fopen("Berjalan", "r");
    if (log_file == NULL) {
        printf("Tidak ada aplikasi.\n");
        return;
    }
    pid_t pid;
    while (fscanf(log_file, "%d", &pid) != EOF) {
        if (kill(pid, SIGTERM) == -1) {
            perror("Error killing process");
        } else {
            printf("Killed process with PID: %d\n", pid);
        }
    }
    fclose(log_file);
    remove("berjalan"); // Hapus file log setelah selesai melakukan kill
}
```
- Fungsi ini membaca PID aplikasi dari file log dan mematikan aplikasi.
- Membuka file log, membaca PID, dan mengirimkan sinyal SIGTERM ke setiap PID.
- Setelah selesai, file log dihapus.
```
int main(int argc, char *argv[]) {
    pid_t pids[100]; 
    int pid_count = 0;

    // Menghilangkan peringatan Gtk
    putenv("GTK_MODULES=");

    if (argc < 2) {
        printf("Usage: setup -o <app1> <num1> <app2> <num2>... or setup -f <file.conf> or setup -k\n");
        return 1;
    }
    if (strcmp(argv[1], "-o") == 0) {
        // Membuka aplikasi dengan perintah
        for (int i = 2; i < argc; i += 2) {
            int num_windows = atoi(argv[i + 1]);
            open_app(argv[i], num_windows, pids, &pid_count);
        }
    } else if (strcmp(argv[1], "-f") == 0) {
        // Membaca konfigurasi dari file
        if (argc != 3) {
            printf("Usage: setup -f <file.conf>\n");
            return 1;
        }
        read_config_file(argv[2], pids, &pid_count);
    } else if (strcmp(argv[1], "-k") == 0) {
        // Mematikan aplikasi
        kill_apps(pids, pid_count);
        // Menjalankan fungsi untuk melakukan kill berdasarkan file log
        kill_apps_from_log();
    } else {
        printf("Invalid option\n");
        return 1;
    }
return 0;
}
```
Fungsi utama program.

- Memeriksa argumen baris perintah untuk menentukan tindakan apa yang diambil.
- Jika argumen adalah -o, program membuka aplikasi dengan argumen baris perintah.
- Jika argumen adalah -f, program membaca konfigurasi dari file.
- Jika argumen adalah -k, program mematikan aplikasi.
- Jika argumen tidak valid, program mencetak pesan kesalahan.
  
## ***Dokumentasi***
![Screenshot (340)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/3345e8f9-812e-4603-bfff-44125c249ddc)
![Screenshot (341)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/0eee9cb1-9b07-4bd8-b7c1-21fd9b187b30)
![Screenshot (342)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/c394fd92-4e57-46be-a086-a39f0bd7215e)
![Screenshot (343)](https://github.com/Rafjonath/Sisop-2-2024-MH-IT26/assets/150430084/c56fad9a-d850-4b57-9594-4fd802c749de)


NB: Semua soal tidak boleh dikerjakan menggunakan fungsi **system()**. Di utamakan menggunakan fungsi **fork()** atau **exec()**.
