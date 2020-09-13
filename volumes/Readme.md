# Working with volumesS

> A volume can be a directory located outside the system's file system of a container. Docker allows you to specify directories in the container so that can be mapped to the host file system. With that, we have the ability to manipulate data in the container without having any relation with the image information. A volume can be shared between multiple containers.

## Managing the data

> Volumes are directories set up within a container that provide the data sharing and persistence feature.

> we can test the use of volumes with the -v option. With it, it is possible to inform a directory on the host location that can be accessed in the container, working in a similar way with a mapping. In addition, we will inform the type of permission that the container will have under the local directory, being: ro for read only, and rw for reading and writing.

* docker run ‐d ‐p 8080:8080 –v /tmp/nginx:/usr/share/nginx/html:ro nginx

## Using volumes in the Dockerfile

```Dockerfile
FROM ubuntu
MAINTAINER Arthur Anastopulos <exemplo@gmail.com>
ENV DEBIAN_FRONTEND noninteractive
RUN apt‐get update ‐qq && apt‐get install ‐y  
mysql‐server‐5.5
ADD my.cnf /etc/mysql/conf.d/my.cnf
RUN chmod 664 /etc/mysql/conf.d/my.cnf
ADD run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run
VOLUME ["/var/lib/mysql"]
EXPOSE 3001
CMD ["/usr/local/bin/run"]
```

* sudo docker build ‐t mysql .

* sudo docker run ‐d ‐p 3001:3001 ‐e MYSQL_ROOT_PASSWORD=xpto1234

* mysql ‐h 127.0.0.1 ‐u root ‐p

* sudo docker run ‐i ‐t ‐‐volumes‐from 198888888887 busybox

* Delete Volume: docker rm  ‐v volume_name

## Backup and restore volumes

* sudo docker run ‐‐volumes‐from 198888888887 ‐v \
* (pwd):/backup ubuntu tar cvf /backup/backup.tar/var/lib/mysql

* sudo docker run ‐‐volumes‐from 198888888887 ‐v \
* (pwd):/backup ubuntu tar cvf /backup/backup.tar /var/lib/mysql
* sudo docker rm ‐f 198888888887
* sudo docker run ‐d ‐p 3306:3306 ‐e MYSQL_ROOT_PASSWORD=xpto1234

* sudo docker run ‐‐volumes‐from eb523451265 ‐v \
* (pwd):/backup busybox tar xvf /backup/backup.tar