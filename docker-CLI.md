# Dasar CLI Docker
1. Melihat Images
    ```
    docker images
    ```

2. Download Images
    ```
    docker pull debian (latest)

    docker pull debian:9.8 (certain version)
    ```

3. Melihat Docker Container yang sedang running
    ```
    docker cotainer ls
    ```

4. Melihat semua list Docker Container
    ```
    docker container ls -a
    ```

5. Membuat Docker Container
    ```
    docker container create --name mongodb1 mongo:4.0
    ```

6. Menjalankan Container
    ```
    docker container start mongodb1
    ```

7. Mencari Images
    ```
    docker search mysql
    ```

8. Menghentikan Container
    ```
    docker container stop mongodb1
    ```

9. Menghapus Container
    ```
    docker container rm mongodb1
    ```

10. Membuat Container yang dapat diakses dari host. Dengan kata lain, membuka port pada Container sehingga service yang dijalankan dapat diakses dari luar
    ```
    docker container create --name mongodb2 -p 1234:27017 mongo
    ```

11. Menjalankan Images secara langsung. Sehingga, container akan otomatis terbentuk dari Image tersebut
    ```
    docker run -itd mongo
    ```

12. Menjalankan Image secara langsung dan otomatis membuat container. Dan juga membuka port untuk container tersebut. Contohnya pada Image mysql/mysql-server

    ```
    docker run -d --name mysqlserver1 -p 1234:3306 mysql/mysql-server
    ```

13. Masuk ke dalam Container

    ```
    docker exec -it mongodb2 bash
    ```

    Nanti mungkin anda akan menemui beberapa sedikit masalah, seperti Image yang tidak bisa di start dengan perintah:
    
    ```
    docker container start namacontainer
    ```

    walaupun Container tersebut sudah dibuat sebelumnya. Tetapi Image tersebut akan jalan ketika langsung dijalankan dari Image-nya, contohnya seperti mysql atau mysql/mysql-server. Jadi ada langkah tertentu untuk menjalankan Image tertentu. Seperti mysql/mysql-server, anda bisa melihat dokumentasi resminya. Container mysql dapat di jalankan dengan perintah:
    ```
    docker run -itd --name mysqlserver1 -p 1234:3306 mysql/mysql-server
    ```

    Ketika dilihat pada running Container, maka Container tersebut langsung berjalan.
    Jadi perintah tersebut akan membuat container otomatis dari image mysql/mysql-server. Jika image mysql/mysql-server tidak ada di repository local, maka dia akan download image dari registry.

    ```
    docker ps

    docker container ls
    ```

14. Membuat Volume
    ```
    docker volume create data1

    docker volume create data2
    ```

    Untuk melihat volume yang sudah dibuat pada docker, ketikkan perintah:
    ```
    docker volume ls
    ```

    Untuk membuat container dengan mencantumkan volume yang ada dapat menggunakan opsi `-v`.
    ```
    docker run -itd --name mysqlserver1 -v data1:/var/lib/mysql -p 1234:3306 mysql
    ```

    Perintah tersebut akan me-mount volume data1 ke direktori `/var/lib/mysql/` pada container mysqlserver1.

15. Melakukan Commit Container
    ```
    docker commit container1 image1
    ```
    Perintah tersebut akan membuat image dengan nama image1 dari container yang sedang running yaitu container1. Jadi kondisi container1 yang sekarang akan disimpan dalam image1.

    Jika terjadi perubahan pada container1, maka ketika nanti membuat container baru dari image1 maka perubahan tersebut akan tetap ada.


# Contoh 1 – Setup Container Mongo
1. Download image untuk mongo
    ```
    docker pull mongo
    ```

2. Periksa apakah image sudah ter-download.
    ```
    docker images
    ```

3. Buat kontainer untuk mongo
    ```
    docker container create --name mongoserver1 -p 1000:27017 mongo
    ```

4. Periksa apakah kontainer sudah terbuat
    ```
    docker container ls -a
    ```

5. Jalankan kontainer mongo
    ```
    docker container start mongoserver1
    ```

6. Periksa apakah kontainer mongo sudah jalan
    ```
    docker container ls
    ```

7. Untuk masuk ke kontainer mongo dapat menggunakan perintah
    ```
    docker exec -it mongoserver1 bash
    mongo
    ```

8. Untuk mengakses mongo dari host
    ```
    mongo --port 2000
    ```

# Contoh 2 – Setup Container Mysql Server
1. Download image untuk mysql-server
    ```
    docker pull mysql/mysql-server
    ```

2. Periksa apakah image sudah ter-download
    ```
    docker images
    ```

3. Buat kontainer untuk mysql-server dan langsung jalankan
    ```
    docker run -d --name mysqlserver1 -p 5000:3306 mysql/mysql-server
    ```

4. Periksa apakah kontainer sudah jalan
    ```
    docker container ls
    ```

5. Periksa log pada container mysql-server untuk melihat password yang di-generate
    ```
    docker logs mysqlserver1 | grep GENERATED
    ```

6. Untuk masuk ke kontainer mysql dapat menggunakan perintah:
    ```
    docker exec -it mysqlserver1 bash
    ```

7. Masuk ke mysql
    ```
    mysql -u root -p (gunakan password yang di generate)
    ```

8. Ganti password mysql
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpass';
    ```

9. Buat user baru mysql
    ```
    CREATE USER 'newuser'@'%' IDENTIFIED BY 'newpass';
    ```

10. Berikan akses pada user agar dapat diakses dari luar atau host
    ```
    GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%' WITH GRANT OPTION;
    ```

11. Untuk mengakses mysql-server dari host
    ```
    mysql -h localhost -P 1234 --protocol=tcp -u root -p
    ```

References:
- https://zakkymuhammad.com/blog/perintah-perintah-dasar-pada-docker-dan-contohnya/
