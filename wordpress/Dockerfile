FROM oraclelinux:7-slim

ARG MYSQL_SERVER_PACKAGE=mysql-community-server-minimal-5.7.25
ARG MYSQL_SHELL_PACKAGE=mysql-shell-8.0.15

# Install server
RUN yum install -y https://repo.mysql.com/mysql-community-minimal-release-el7.rpm \
      https://repo.mysql.com/mysql-community-release-el7.rpm \
  && yum-config-manager --enable mysql57-server-minimal \
  && yum install -y \
      $MYSQL_SERVER_PACKAGE \
      $MYSQL_SHELL_PACKAGE \
      libpwquality \
  && yum clean all \
  && mkdir /docker-entrypoint-initdb.d

VOLUME /var/lib/mysql


ENV MYSQL_ROOT_PASSWORD: 1234
ENV MYSQL_DATABASE: wordpress
ENV MYSQL_USER: bryan
ENV MYSQL_PASSWORD: 1234

EXPOSE 3306 3306
CMD ["mysqld"]