# KUMPULAN TUTORIAL 

## Membuat Aplikasi Cronjob menggunakan NodeJS
1.  Create a NodeJs application
    ```bash
    npm init
    
    npm i express
    ```

2. Set up the Express server

   Create the `app.js` file (note that some people prefer to call this file server.js Configure the port and set up the listener

    ```js
	const express = require("express");
	const app = express();
	app.set("port", process.env.PORT || 3000);

	app.listen(app.get("port"), () => {
	  console.log("Express server listening on port " + app.get("port"));
	});
    ```


3. Add the node-cron npm package to the application
    ```bash
    npm i node-cron
    ```
4. Set up a folder for your scheduled jobs

   Create a folder called "scheduledFunctions" or similar


5. Define the logic to run your scheduled jobs

   In the folder you just set up, create a file to hold your scheduled function logic. You can also use different files for each task if you prefer.

   Configure scheduled logic:
    ```js
    const CronJob = require("node-cron");

	exports.initScheduledJobs = () => {
	  const scheduledJobFunction = CronJob.schedule("*/5 * * * *", () => {
	    console.log("I'm executed on a schedule!");
	    // Add your custom logic here
	  });

	  scheduledJobFunction.start();
	}
    ```

   The execution frequency of your custom function is defined as the first argument to the Cron.schedule function. Note that this argument is written as a Cron Schedule Expression or crontab. I find Crontab Guru a really useful resource to play around and get comfortable with schedule expressions so you can easily set up your own.


6. Configure the Express app to initialise and run the job schedules
   In the app.js file add a call to the init function(s) from your scheduled file(s).

    ```js
	const express = require("express");
	// DEFINE the path to your scheduled function(s)
	const scheduledFunctions = require('./scheduledFunctions');
	const app = express();
	app.set("port", process.env.PORT || 3000);

	// ADD CALL to execute your function(s)
	scheduledFunctions.initScheduledJobs();

	app.listen(app.get("port"), () => {
	  console.log("Express server listening on port " + app.get("port"));
	});

    ```

   Start the app to generate the functions and their schedules in the Cron scheduler. Your custom logic will be executed to schedule as long as the app is running.


### Reference:

- https://www.mickpatterson.com.au/blog/how-to-setup-scheduled-functions-with-nodejs-express-and-node-cron

- https://medium.com/javascript-indonesia-community/penggunaan-cron-jobs-di-node-js-c986bbaaf7f2



## Dasar CLI Docker
1. Melihat Images

    ```bash
    docker images
    ```

2. Download Images

    ```bash
    docker pull debian (latest)

    docker pull debian:9.8 (certain version)
    ```

3. Melihat Docker Container yang sedang running

    ```bash
    docker cotainer ls
    ```

4. Melihat semua list Docker Container
    
    ```bash
    docker container ls -a
    ```

5. Membuat Docker Container
    
    ```bash
    docker container create --name mongodb1 mongo:4.0
    ```

6. Menjalankan Container
    
    ```bash
    docker container start mongodb1
    ```

7. Mencari Images
    
    ```bash
    docker search mysql
    ```

8. Menghentikan Container
    
    ```bash
    docker container stop mongodb1
    ```

9. Menghapus Container

    ```bash
    docker container rm mongodb1
    ```

10. Membuat Container yang dapat diakses dari host. Dengan kata lain, membuka port pada Container sehingga service yang dijalankan dapat diakses dari luar
    
    ```bash
    docker container create --name mongodb2 -p 1234:27017 mongo
    ```

11. Menjalankan Images secara langsung. Sehingga, container akan otomatis terbentuk dari Image tersebut

    ```bash
    docker run -itd mongo
    ```

12. Menjalankan Image secara langsung dan otomatis membuat container. Dan juga membuka port untuk container tersebut. Contohnya pada Image mysql/mysql-server

    ```bash
    docker run -d --name mysqlserver1 -p 1234:3306 mysql/mysql-server
    ```

13. Masuk ke dalam Container

    ```bash
    docker exec -it mongodb2 bash
    ```

    Nanti mungkin anda akan menemui beberapa sedikit masalah, seperti Image yang tidak bisa di start dengan perintah:

    ```bash
    docker container start namacontainer
    ```

    walaupun Container tersebut sudah dibuat sebelumnya. Tetapi Image tersebut akan jalan ketika langsung dijalankan dari Image-nya, contohnya seperti mysql atau mysql/mysql-server. Jadi ada langkah tertentu untuk menjalankan Image tertentu. Seperti mysql/mysql-server, anda bisa melihat dokumentasi resminya. Container mysql dapat di jalankan dengan perintah:
    
    ```bash
    docker run -itd --name mysqlserver1 -p 1234:3306 mysql/mysql-server
    ```

    Ketika dilihat pada running Container, maka Container tersebut langsung berjalan.
    Jadi perintah tersebut akan membuat container otomatis dari image mysql/mysql-server. Jika image mysql/mysql-server tidak ada di repository local, maka dia akan download image dari registry.

    ```bash
    docker ps

    docker container ls
    ```

14. Membuat Volume
    
    ```bash
    docker volume create data1

    docker volume create data2
    ```

    Untuk melihat volume yang sudah dibuat pada docker, ketikkan perintah:
    
    ```bash
    docker volume ls
    ```

    Untuk membuat container dengan mencantumkan volume yang ada dapat menggunakan opsi `-v`.
    
    ```bash
    docker run -itd --name mysqlserver1 -v data1:/var/lib/mysql -p 1234:3306 mysql
    ```

    Perintah tersebut akan me-mount volume data1 ke direktori `/var/lib/mysql/` pada container mysqlserver1.

15. Melakukan Commit Container
    
    ```bash
    docker commit container1 image1
    ```
    Perintah tersebut akan membuat image dengan nama image1 dari container yang sedang running yaitu container1. Jadi kondisi container1 yang sekarang akan disimpan dalam image1.

    Jika terjadi perubahan pada container1, maka ketika nanti membuat container baru dari image1 maka perubahan tersebut akan tetap ada.


### Contoh 1 – Setup Container Mongo
1. Download image untuk mongo
    
    ```bash
    docker pull mongo
    ```

2. Periksa apakah image sudah ter-download.

    ```bash
    docker images
    ```

3. Buat kontainer untuk mongo

    ```bash
    docker container create --name mongoserver1 -p 1000:27017 mongo
    ```

4. Periksa apakah kontainer sudah terbuat

    ```bash
    docker container ls -a
    ```

5. Jalankan kontainer mongo

    ```bash
    docker container start mongoserver1
    ```

6. Periksa apakah kontainer mongo sudah jalan

    ```bash
    docker container ls
    ```

7. Untuk masuk ke kontainer mongo dapat menggunakan perintah

    ```bash
    docker exec -it mongoserver1 bash
    ```

8. Untuk mengakses mongo dari host

    ```bash
    mongo --port 2000
    ```

### Contoh 2 – Setup Container Mysql Server
1. Download image untuk mysql-server

    ```bash
    docker pull mysql/mysql-server
    ```

2. Periksa apakah image sudah ter-download

    ```bash
    docker images
    ```

3. Buat kontainer untuk mysql-server dan langsung jalankan

    ```bash
    docker run -d --name mysqlserver1 -p 5000:3306 mysql/mysql-server
    ```

4. Periksa apakah kontainer sudah jalan

    ```bash
    docker container ls
    ```

5. Periksa log pada container mysql-server untuk melihat password yang di-generate

    ```bash
    docker logs mysqlserver1 | grep GENERATED
    ```

6. Untuk masuk ke kontainer mysql dapat menggunakan perintah:

    ```bash
    docker exec -it mysqlserver1 bash
    ```

7. Masuk ke mysql

    ```bash
    mysql -u root -p (gunakan password yang di generate)
    ```

8. Ganti password mysql

    ```bash
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpass';
    ```

9. Buat user baru mysql

    ```bash
    CREATE USER 'newuser'@'%' IDENTIFIED BY 'newpass';
    ```

10. Berikan akses pada user agar dapat diakses dari luar atau host

    ```bash
    GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%' WITH GRANT OPTION;
    ```

11. Untuk mengakses mysql-server dari host

    ```bash
    mysql -h localhost -P 1234 --protocol=tcp -u root -p
    ```

### References:
- https://zakkymuhammad.com/blog/perintah-perintah-dasar-pada-docker-dan-contohnya/



## Menjalankan Maria DB dengan Docker

```bash
docker run --name htop_db -v /home/user/data/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=***** -p 3307:3306  -d docker.io/library/mariadb:10.5
```

### References:
-  https://hub.docker.com/_/mariadb
- https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/


### Membuat file docker-compose.yaml
```yaml
version: '2.2'

services:
  mariadb:
    image: mariadb:10.7
    ports:
      - 3306:3306
    volumes:
      - ./apps/mariadb:/var/lib/mysql
      - ./apps/data:/home/user/data
    environment:
      - MYSQL_ROOT_PASSWORD=S3cret
      - MYSQL_PASSWORD=An0thrS3crt
      - MYSQL_USER=citizix_user
      - MYSQL_DATABASE=citizix_db

```
### Start and Stop docker compose
#### Start docker compose

```bash
docker-compose up -d
```

```
`up` brings up the container
`-d` in a detached mode
```

Output:

```
Creating network "tmp_default" with the default driver
Creating tmp_mariadb_1 ... done
```


#### Cek docker-compose

```bash
docker-compose ps
```

Output:
```bash
       Name                    Command              State              Ports
----------------------------------------------------------------------------------------
kubotadb_mariadb_1   docker-entrypoint.sh           Up      0.0.0.0:3312->3306/tcp,:::33
                     mariadbd                               12->3306/tcp
```

#### Masuk ke system docker

```bash
docker-compose exec mariadb /bin/bash
```

#### Menjalankan docker dengan container

```bash
docker run -d \
    --name my-mariadb \
    -p 3306:3306 \
    -v ~/apps/mariadb/data:/var/lib/mysql \
    --user 1000:1000 \
    -e MYSQL_ROOT_PASSWORD=S3cret \
    -e MYSQL_PASSWORD=An0thrS3crt \
    -e MYSQL_USER=citizix_user \
    -e MYSQL_DATABASE=citizix_db \
    mariadb:10.7
```


**In the above command:**

- The `-d` instructs docker container to run as a detached process. It run container in background and print container ID
- `-p` is for port mapping. We are instructing the container to expose the container port externally. Container port `3306` is mapped to host port `3306`. That means the service can be accessed through `localhost:3306`
- The `-v` directive is used to mount volumes. In our case we are mounting the container volume `/var/lib/mysql` to host path `~/apps/mariadb/data`. Containers are ephemeral devices that will contain its data for the time it is running. Once a container is stopped, its data is lost. Mounting volumes ensures that the data is added to a host path that can be reused when the container is restarted.
- The `--user` argument is used to run the container with an arbitrary user (non root user). This happens if you need to run mysqld with a specific UID/GID.
- The `-e` argument is for the environment variables. The supplied environment variables will be used to set up a Mariadb user, password and a database.


## Create Reverse Proxy with Docker

1. Start with the official Nginx image
    
    ```bash
    docker run -d --name nginx-base -p 80:80 nginx:latest
    ```

2. Copy the Nginx config file from Docker

    ```bash
    docker cp nginx-base:/etc/nginx/conf.d/default.conf ./default.conf
    ```

3. Edit file default

    ```bash
    location /sample {
        proxy_pass http://192.168.246.131:8080/sample;
    }
    ```

4. Copy the Nginx config file back to Docker

    ```bash
    docker cp ./default.conf nginx-base:/etc/nginx/conf.d/
    ```

   Next, validate and reload the Docker Nginx reverse proxy configuration:

    ```bash
    docker exec nginx-base nginx -t

    docker exec nginx-base nginx -s reload
    ```

## Create a new Docker Nginx reverse proxy image
1. Now that the Docker Nginx reverse proxy container works, create a new Docker image based on the container’s configuration:

    ```bash
    docker commit nginx-base nginx-proxy

    # check images

    docker images
    ```

2. Nginx reverse proxy server Dockerfile
   The content of the Nginx reverse proxy Dockerfile is as follows:
    
   ```bash
    FROM nginx:latest

    COPY default.conf /etc/nginx/conf.d/default.conf
    ```

3. New Nginx Docker image creation
   When the file is saved, run the docker build command in the same folder as the Dockerfile and the default.conf file:
   ```bash
   docker build -t nginx-reverse-proxy .
   ```

### References:
- https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Docker-Nginx-reverse-proxy-setup-example


## Push dengan repository yang sudah ada

1. Cek url existing

    ```bash
    git remote -v
    ```
   
2. Tambahkan url remote

    ```bash
    git remote add origin https://github.com/triyasmkom/tutorial_repo.git

    ```
   
3. Update repository dari remote

    ```bash
    git fetch
    ```

4. Ambil repository dari remote

    ```bash
    git pull origin master
    ```
   
5. Masukkan semua file/folder yang ingin kita commit

    ```bash
    git add .
    ```
   
6. Commit semua perubahan

    ```bash
    git commit -m "tutoarial git"
    ```
   
7. Push ke repository remote

    ```bash
    git push origin master
    ```

## Dasar-dasar comment line git
1. membuat repository

    ```bash
    git init
    ```
   
2. melihat status perubahan file

    ```bash
    git status
    ```
3. menambahkan commit

    ```bash
    git add <nama file/folder>
    ```
   
4. melakukan commit

    ```bash
    git commit -m "message commit"
    ```
   
5. pindah/membuat branch

    ```bash
    git checkout <nama_branch>
    ```
   
6. melihat perbedaan perubahan

    ```bash
    git diff
    ```
   
7. menghubungkan repository local dengan server

    ```bash
    git remote add origin <alamat server>
    ```
   
8. membuat branch baru

    ```bash
    git checkout -b <nama_branch>
    ```
   
9. pindah branch

    ```bash
    git checkout <branch yg dituju>
    ```
   
10. menghapus branch

    ```bash
    git branch -d feature_a
    git branch --delete feature_a
    ```
    
11. menghapus branch secara paksa

    ```bash
    git branch -D feature_a

    git branch --delete --force feature_a

    *-D = --delete --force
    ```
    
12. hapus branch remote

    ```bash
    git push -d <remote_name> <branch_name>
    git push -d origin example
    ```
    
13. update dan merging

    ```bash
    git pull

    git merge <nama_branch>
    ```


## Menyimpan sementara di local sebelum melakukan pull dari remote
1. Cek status perubahan

    ```bash
    git status
    ```
   
2. mengabaikan perubahan

    ```bash
    git stash
    ```
   
3. pull dari remote

    ```bash
    git pull
    ```
   
4. mengembalikan perubahan

    ```bash
    git stash pop
    ```
   
5. melihat daftar stash

    ```bash
    git stash list
    ```
   
6. menghapus stash

    ```bash
    git stash clear
    ```

## Git Cheat Sheet
### Setup
Set the name and email that will be attached to your commits and tags.
```shell
git config --global user.name "Triyas"

git config --global user.email "email@example.com"
```

### Start a Project
Create a local repo to initialise the current directory as a git repo

```shell
git init <directory>
```

Download a remote repo

```shell
git clone <url>
```

### Make a Change
Add a file to staging

```shell
git add <file>
```

Stage all files

```shell
git add .
```

Commit all staged files to git

```shell
git commit -m "commit_message"
```

Add all changes made to tracked files & commit

```shell
git commit -am "commit_message"
```


### Basic Concepts

- **Main**: default development branch
- **Origin**: default upstream repo
- **HEAD**: current branch
- **HEAD^**: parent of Head
- **HEAD-4**: great-great grand parent of Head

### Branches

List all local branches. Add -r flag to show all remote branches. -a flag for all brances.

```shell
git branch 
```

Create a new branch

```shell
git branch <new branch>
```

Switch to a branch & update the working directory

```shell
git checkot <branch>
```

Create a new branch and switch to it

```shell
git checkout -b <new branch>
```

Delete a merged branch

```shell
git branch -d <branch>
```

Delete a branch, whether merged or not

```shell
git branch -D <branch>
```

Add a tag to current commit (often used for new version releases)

```shell
git tag <tag-name>
```


### Merging
Merge branch a into branch b. Add --no-ff option for no-fast-forward merge

![image](./image/merging.png)

```shell
git checkout b

git merge a
```

Merge & squash all commits into pne new commit

```shell
git merge --squash a
```


### Rebasing
Rebase feature branch onto main (to incorporate new change made to main). Prevents unnecessary merge commits into feature, keeping history clean.

![image](./image/rebasing.png)

```shell
git checkout feature

git rebase main
```

Interactively clean up a branches commits before rebasing onto main

```shell
git rebase -i main
```

Interactively rebase the last 3 commits on current branch

```shell
git rebase -i Head-3
```


### Undoing Things

Move(&/or rename) a file & stage move

```shell
git mv <existing_path> <new_path> 
```

Remove a file from working directory & staging area then stage the removal

```shell
git rm <file>
```

Remove from staging area only

```shell
git rm --cached <file>
```


View a previous commit (READ only)
```shell
git checkout <commit_ID>
```

Create new commit, reverting the changes from a spesified commit

```shell
git revert <commit_ID>
```

Go back to a previous commit & delete all commits ahead of it(revert is saver). Add --hard flag to also delete workspace changes (BE VERY CAREFUL)

```shell
git reset <commit_ID>
```


### Review your Repo

Use new or modified files not yet committed

```shell
git status
```

List commit history, with respective IDs

```shell
git log --oneline
```

Show changes to unstaged files. For changes to staged files, add --cached option

```shell
git diff
```

Show changes between two commits

```shell
git diff commit1_ID commit2_ID
```


### Stashing
Store modified & staged changes. To include untracked files. Add -u flag. For untracked & ignored files, add -a flag.

```shell
git stash
```

As above, but add a comment

```shell
git stash save "comment"
```


Partial stash. Stash just a single file, a collection of files, or individual changes from within files

```shell
git stash -p
```

List all stashes

```shell
git stash list
```

Re-apply the stash without deleting it

```shell
git stash apply
```

Re-apply the stash at index 2, then delete it from the stash list. Omit stash@{n} to pop the most recent stash.

```shell
git stash pop stash@{2}
```

Show the duff summary of stash 1. Pass the -p flag to see the full diff.

```shell
git stash show stash@{1}
```

Delete stash at index 1, Omit stash@{n} to delete last stash made

```shell
git stash drop stash@{1}
```

Delete all stashes

```shell
git stash clear
```


### Synchronizing

Add a remote repo

```shell
git remote add <alias> <url>
```

View all remote connections. Add -v flag to view url

```shell
git remote
```

Remove a connection

```shell
git remote remove <alias>
```

Rename a connection

```shell
git remote rename <old> <new>
```

Fetch all branches from remote repo (no merge)

```shell
git fetch <alias>
```

Fetch a spesific branch

```shell
git fetch <alias> <branch>
```

Fetch the remote repo's copy of the current branch the merge

```shell
git pull
```

Move (rebase) your local changes onto the top of new changes made to the remote repo (for clean, linear history)

```shell
git pull --rebase <alias>
```

upload local content to remote repo

```shell
git push <alias>
```

Upload to a branch (can then pull request)

```shell
git push <alias> <branch>
```

![alt](https://media.licdn.com/dms/image/C4D22AQGUc8T7sQh-Tw/feedshare-shrink_800/0/1678891709141?e=1681948800&v=beta&t=7LbYj8oBVWttDZn0JxBsqDQ2Z0neyq5I2PtEg2I46TA)


## Restore and Backup database mysql
### Backup database from command line with mysqldump

```bash
sudo mysqldump -u [username] -p [database_name] > [filename].sql

sudo mysqldump -u root -p example_db > example_db.sql
```


### Restore database mysql
1. Create new database
    ```bash
    # login to mysql with CLI

        mysql -u root -p
    ```
   
    ```mysql
    # create new database
    
    create database example_db
    ```

2. Restore from file .sql

    ```bash
    mysql -u root -p example_db < example_db.sql
    ```
### Reference:
https://phoenixnap.com/kb/how-to-backup-restore-a-mysql-database


**If your find this error:**
```bash
#1273 – Unknown collation: ‘utf8mb4_unicode_520_ci’
```

Please your remove collation `utf8mb4_unicode_520_ci` in your file `example_db.sql`
