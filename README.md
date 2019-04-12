# M300_Docker

![M300_Docker](pics/docker.png)  
## Inhaltsverzeichnis

* Wissensstand vor LB2
* Tools
* Wordpress Webserver per Docker aufsetzen 
  * Dockerfile
  * Test
* LB2 Wissenstand
* Reflexion

## Wissensstand vor LB1
**Linux** <br>
Erweiterte Linux kenntnisse durch ÜK-, TBZ- und Abteilungsprojekte. <br>
Bash Experte und Linux Fan :)

**Git** <br>
Zuvor noch nie mit Git gearbeitet.
Nun habe ich dank diesem Modul mehr Erfahrung mit Git und kann es einwandfrei nutzen.

**Docker** <br>


## Tools
**GitHUB** <br>
Nach Anleitung von Herr Berger:  
*Github*  <br>
  1. Konto erstellen auf www.github.com 
  2. Bestätigung der E-Mail durch Verifications Link

*Repository*<br>
  1. Neues Repository Erstellen
  2. Name meines Repositorys M300_Docker
  3. Status des Repositorys auf Öffentlich stellen
  4. README File erstellen, .md für die Richtige Markdown darstellung des Files

*Git Bash*<br>  
1. Git Herunterladen https://git-scm.com/downloads  
2. Installation abschliessen und Git Bash öffnen
3. Konfigurieren per Bash Befehle:  
 --> git config --global user.name "bmestry"  
 --> git config --global user.email "bryan.lo99la@gmail.com" 
 
*Klonen des Repositorys*<br>
1. Erstelltes Repository auf GitHub öffen. Auf Clone/Download drücken und den Link kopieren.
2. Bash öffnen und Ordner für dieses Repository oder allgemeiner Ordner für kommende Repositories erstellen.
3. Zum Directory wechseln und folgender Befehl ausführen
```ruby
  git clone https://github.com/bmestry/M300_Bryan
  git status
```

## Wordpress Webserver per Docker aufsetzen 
 
 
### Docker Compose File

```yaml
  version: '3.3'

services:
   datenbank:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: 1234
       MYSQL_DATABASE: wordpress
       MYSQL_USER: bryan
       MYSQL_PASSWORD: 1234

   wordpress:
     depends_on:
       - datenbank
     image: wordpress:latest
     ports:
       - "8080:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: datenbank:3306
       WORDPRESS_DB_USER: bryan
       WORDPRESS_DB_PASSWORD: 1234
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
```

### Dockerfile
```yaml
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
```


### Test



## LB1 Wissenstand

 
## Reflexion
 
 
 
