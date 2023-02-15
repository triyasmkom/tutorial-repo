## Create Reverse Proxy with Docker

1. Start with the official Nginx image
    ```
    docker run -d --name nginx-base -p 80:80 nginx:latest
    ```

2. Copy the Nginx config file from Docker
    ```
    docker cp nginx-base:/etc/nginx/conf.d/default.conf ./default.conf
    ```

3. Edit file default
    ```
    location /sample {
        proxy_pass http://192.168.246.131:8080/sample;
    }
    ```

4. Copy the Nginx config file back to Docker
    ```
    docker cp ./default.conf nginx-base:/etc/nginx/conf.d/
    ```

    Next, validate and reload the Docker Nginx reverse proxy configuration:
    ```
    docker exec nginx-base nginx -t

    docker exec nginx-base nginx -s reload
    ```

## Create a new Docker Nginx reverse proxy image
1. Now that the Docker Nginx reverse proxy container works, create a new Docker image based on the container’s configuration:
    ```
    docker commit nginx-base nginx-proxy

    check images

    docker images
    ```

2. Nginx reverse proxy server Dockerfile
    The content of the Nginx reverse proxy Dockerfile is as follows:
    ```
    FROM nginx:latest

    COPY default.conf /etc/nginx/conf.d/default.conf
    ```

3. New Nginx Docker image creation
   When the file is saved, run the docker build command in the same folder as the Dockerfile and the default.conf file:
   ```
   docker build -t nginx-reverse-proxy .
   ```

References: 
- https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/Docker-Nginx-reverse-proxy-setup-example