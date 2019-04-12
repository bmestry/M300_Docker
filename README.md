# M300_Docker

![M300_Docker](pics/docker.png)  
## Inhaltsverzeichnis

* Wissensstand vor LB2
* Tools
* Wordpress Webserver per Docker aufsetzen 
  * Dockerfile
  * Docker Compose File
* Test
  * Dockerfile
  * Docker Compose File
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

### Dockerfile - Versuch

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
Ich habe versucht mit einem Dockerfile mir ein Image zu erstellen. In diesem Image hätte ein funktionfähiger Datenbank Server mit einer vorkonfigurierten Datenbank sein sollen. Datenbank, Datenbank User, Datenbank Passwort und Root Passwort.

Aber da mein Docker Image ausgeführt durch ein Docker Build nicht funktioniert hat, musste ich alle benötigten Konfigurationen im docker-compose.yml File vornehmen.
 
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

Als erstes musste meine LB in 2 container aufgeteilt werden. Auf der einen Seite haben wir den Datenbank Server und auf der anderen den Wordpress Server. Der Wordpress Webserver muss auf die Datenbank über einen Port verbinden können. Hier habe ich denn Standard Port 3306 belassen, da dieser ohni Forwarding sowieso nach aussen gezeigt wird. Die Umgebungsvariabeln beider Dienste kann Online gefunden werden. Somit wurden diese in mein Script eingebunden. Der Port 80 muss durch ein weiterleitenden Port nach aussen gezeigt wurde, da dieser Port von Host selbst schon benutzt wird. Hier habe ich mich für den Standard Weiterleitungs HTTP Port entschieden: 8080.


## Test

**Dockerfile** <br>
1. Dockerfile speicher
2. In das Verzeichnis des Dockerfiles wechseln.
3. Powershell, CMD oder bash starten
4. ```cmd docker build -t <IMAGENAME>``` um den Container zu bauen.
5. ```cmd docker ps -a``` überprüfen ob der Container lauft.
6. Auf den Dienst des neuerstellten Images zugreifen

**Docker-Compose** <br>
1. Das docker-compose.yml File speichern
2. In das Verzeichnis des docker-compose.yml wechseln.
3. Powershell, CMD oder bash starten
4. ```cmd docker-compose up -d``` um die Container detatched zu starten.
5. ```cmd docker ps -a``` überprüfen ob der Container lauft.
6. Auf den Dienst des neuerstellten Images zugreifen. In meinem Fall auf den weitergeleiteten Port. http://localhost:8080


## LB1 Wissenstand
Alles was in diesem Dokument steht oder in irgenndeiner Verbindung zu Docker, hbae ich durch ihre Theorie Blöcke, selberarbeitung oder Hilfe von dritten gelernt. D.h. ich wusste noch nichts über Docker, ausser das ich ```cmd docker run``` mit einem Imagename ausführen kann. Nun sieht man in der Refelxion und im Beschreib des Codes, dass ich einen grossen Fortschritt in Sachen Docker gemacht habe. Was ich auch nicht wusste und in meinem Vagrant_Wordpress Projekt nicht integriert habe, waren die Umgebungsvariabeln für die Wordpress Installation. Diese Variable bringen den User automatisiert von Beginn an einen Schritt weiter. Das kommende muss jedoch trotzdem vom User Manuell eingegeben werden. Ich finde die Idee das man im Imagenamen die Variable xxx:latest mit geben kann, eine sehr nützliche und gut skalierende Funktion. Diese gibt es Beipielweise nicht bei Vagrant. 

 
## Reflexion
Ich habe mich sehr auf dieses Projekt gefreut da es im Gegensatz zu Vagrant für mich etwas Total neues war. Ich bin sehr enttäuscht, dass ich nicht genug Zeit hatte um mein selbstgeschriebenes Dockerfile zu debuggen und dann für das Compose benutzen zu können. Aber im gross und ganzen hat mir das Projekt sehr spass gemacht. Ich konnte viel von anderen lernen die bereits mit Container Virtualisierung und spezifisch Docker zu tun hatten. Der Start fiel mir sehr schwer, denn ich startete mit einem Lokal installierten Docker und einem Compose File. Merkte dann aber das ich gerne eine Stufe höher Zielen möchte mit einem eigenen Image. Installierte mir dann eine VM mit sehr viel Geduld weil es zuerst nicht klappte. Und konnte dann schlussendlich kein ein funktiontüchtiges Dockerfile erstellen. Ich hoffe aber dann man sieht das ich mir Mühe gemacht habe und sogar um 21.00 Uhr nicht im Usgang sondern an der LB2 sitze. LG Bryan
 
 
