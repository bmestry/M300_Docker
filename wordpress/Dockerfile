FROM ubuntu:16.04

MAINTAINER Bryan

#Passwort  des Root Users ändern
RUN debconf-set-selections <<< 'mysql-server mysql-server/root_password password 1234'
RUN debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password 1234'

#Installation von Mysql per APT
RUN apt-get update
RUN apt-get install -y mysql-server

#Remote Connection auf Dienst zulassen
RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf


ENV MYSQL_ROOT_PASSWORD: 1234
ENV MYSQL_DATABASE: wordpress
ENV MYSQL_USER: bryan
ENV MYSQL_PASSWORD: 1234

EXPOSE 3306

VOLUME /var/lib/mysql

CMD ["mysqld"]




