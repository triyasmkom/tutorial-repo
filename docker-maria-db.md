# Menjalankan Maria DB dengan Docker
```
docker run --name htop_db -v /home/user/data/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=***** -p 3307:3306  -d docker.io/library/mariadb:10.5
```

References:
-  https://hub.docker.com/_/mariadb
- https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/


# Membuat file docker-compose.yaml
```
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
# Start and Stop docker compose
 ## Start docker compose
 ```
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


## Cek docker-compose
```
docker-compose ps
```

Output:
```
       Name                    Command              State              Ports
----------------------------------------------------------------------------------------
kubotadb_mariadb_1   docker-entrypoint.sh           Up      0.0.0.0:3312->3306/tcp,:::33
                     mariadbd                               12->3306/tcp
```

## Masuk ke system docker
```
docker-compose exec mariadb /bin/bash
```

# Menjalankan docker dengan container
```
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


In the above command:

- The `-d` instructs docker container to run as a detached process. It run container in background and print container ID
- `-p` is for port mapping. We are instructing the container to expose the container port externally. Container port `3306` is mapped to host port `3306`. That means the service can be accessed through `localhost:3306`
- The `-v` directive is used to mount volumes. In our case we are mounting the container volume `/var/lib/mysql` to host path `~/apps/mariadb/data`. Containers are ephemeral devices that will contain its data for the time it is running. Once a container is stopped, its data is lost. Mounting volumes ensures that the data is added to a host path that can be reused when the container is restarted.
- The `--user` argument is used to run the container with an arbitrary user (non root user). This happens if you need to run mysqld with a specific UID/GID.
- The `-e` argument is for the environment variables. The supplied environment variables will be used to set up a Mariadb user, password and a database.