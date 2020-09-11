## Building an Image

### Dockerfile

> Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.

```Dockerfile
FROM ubuntu
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt‐get update
RUN apt‐get install ‐y nginx
```

* FROM -> the base image to create the container;

* MAINTAINER -> specifies the name of who will keep the image; (Deprecated - use LABEL instead)

* RUN -> allows the execution of a command in the container.

* sudo docker build .

* sudo docker image

* sudo docker run ‐d ‐p 8080:80 (...) -> use '$ curl ‐IL http://localhost:8080' to test

###  Mapping ports

```Dockerfile
FROM ubuntu
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt‐get update
RUN apt‐get install ‐y nginx
EXPOSE 80
```

* EXOPSE -> informs Docker that the container listens on the specified network ports at runtime.

### Copying files

```Dockerfile
FROM ubuntu
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt‐get update
RUN apt‐get install ‐y nginx
ADD exemplo /etc/nginx/sites‐enabled/default
EXPOSE 8080
```

* ADD -> copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.

Exemplo
```
server {
listen 8080 default_server;
server_name localhost;
root /usr/share/nginx/html;
index index.html index.htm;
}
```

* sudo docker inspect aeaeaeaeaeae

* sudo docker inspect aeaeaeaeaeae | grep IPAddress "IPAddress": "192.180.0.1",

* sudo docker exec ‐it aeaeaeaeaeae ifconfig eth0

* curl ‐IL http://192.180.0.1:8080

### Defining the working directory

```Dockerfile
FROM ubuntu
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt‐get update
RUN apt‐get install ‐y nginx
ADD exemplo /etc/nginx/sites‐enabled/default
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
ADD ./ /rails
WORKDIR /rails
EXPOSE 8080
CMD service nginx start
```

* WORKDIR -> sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.

* CMD -> There can only be one CMD instruction in a Dockerfile. Provide default arguments for the ENTRYPOINT instruction, both the CMD and ENTRYPOINT instructions should be specified with the JSON array format.

### Initializing services

```Dockerfile
FROM ubuntu
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt‐get update
RUN apt‐get install ‐y nginx
ADD exemplo /etc/nginx/sites‐enabled/default
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
ADD ./ /rails
WORKDIR /rails
EXPOSE 8080
ENTRYPOINT ["/etc/init.d/nginx"]
CMD ["start"]
```

* ENTRYPOINT -> allows to configure a container that will run as an executable.

### Handling logs

```Dockerfile
FROM ubuntu
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt‐get update
RUN apt‐get install ‐y nginx
ADD exemplo /etc/nginx/sites‐enabled/default
RUN ln ‐sf /dev/stdout /var/log/nginx/access.log
RUN ln ‐sf /dev/stderr /var/log/nginx/error.log
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
ADD ./ /rails
WORKDIR /rails
EXPOSE 8080
ENTRYPOINT ["/etc/init.d/nginx"]
CMD ["start"]
```

* stdout & stderr

* docker build .

* docker run ‐d ‐p 80:80 nginx

* for ((i=1; i<=10; i++)); do curl ‐IL http://127.0.0.1;

* docker logs edeeeeeee222

### Export and import of containers

* sudo docker ps ‐q

* sudo docker commit bf3123213123 nova_imagem

* sudo docker save nova_imagem > /tmp/nova_imagem.tar

* sudo docker load < /tmp/nova_imagem.tar