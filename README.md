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
*Repository Tree*<br>
bmestry
|_ Modul300_Bryan
  |_ pics (Ausschliesschlich für Dokumentation)
  |_ Wordpress
  |  |_ docker-compose.yml
  |_ README.md (Dokumenatation)



## Wordpress Webserver per Docker aufsetzen 
 
 
### Dockerfile

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


### Test
 
 
 
 
