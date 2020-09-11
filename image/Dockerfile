FROM ubuntu:16.04
LABEL maintainer="Arthur Anastopulos <exemple@gmail.com>"
RUN apt-get update
RUN apt-get install -y nginx
RUN rm -v /etc/nginx/nginx.conf
ADD nginx.conf /etc/nginx/
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

EXPOSE 80

COPY ./files/ /var/www/html/

CMD ["/usr/sbin/nginx"]