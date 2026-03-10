# M169-P1
---
https://gbssg.gitlab.io/m169

https://www.datacamp.com/de/tutorial/docker-tutorial

---

## 🟢 Container & Docker-Grundbegriffe

**Container:** Eine isolierte Umgebung, die eine Anwendung und alle ihre Abhängigkeiten (Bibliotheken, Konfigurationsdateien) verpackt, damit sie überall gleich läuft.

**Docker Image:** Eine schreibgeschützte Vorlage (wie ein Bauplan), die Anweisungen zum Erstellen eines Containers enthält.

**Dockerfile:** Eine Textdatei mit Befehlen, die Docker nacheinander ausführt, um automatisch ein Image zu erstellen.

**Docker-Container:** Eine laufende Instanz eines Images. Er ist das "lebende" Objekt.

**Docker-Registry:** Ein Lagerort (wie Docker Hub), auf dem Images gespeichert und geteilt werden können.

## 🟢 Virtualisierung vs. Cloud

**Virtualisierung:** Die Technologie, die es ermöglicht, mehrere Betriebssysteme (VMs) auf einer physischen Hardware laufen zu lassen. Sie simuliert Hardware.

**Cloud:** Ein Bereitstellungsmodell, bei dem IT-Ressourcen (wie Rechenleistung oder Speicher) über das Internet "on-demand" gemietet werden. Cloud nutzt Virtualisierung oft als Basis, ist aber das übergeordnete Service-Konzept.

## 🟢 Docker-Container ausführen & interaktiv verwenden

**Befehl:** 
```bash
docker run -it <image-name>
```

**Erklärung:** -i steht für interaktiv, -t weist ein Terminal zu. Damit landest du direkt in der Konsole des Containers.

## 🟠 Portweiterleitungen (Port Mapping)

**Wichtigkeit:** Container laufen in einem isolierten Netzwerk. Um z. B. einen Webserver im Container von deinem Browser aus zu erreichen, muss ein Port des Host-Rechners auf einen Port des Containers umgeleitet werden.

**Befehl:**
```bash
docker run -p 8080:80 nginx
```
(Leitet Host-Port 80 auf Container-Port 8080 weiter).

## 🟠 Docker Volumes verstehen & benennen

**Umgang:** Volumes dienen der Datenpersistenz. Da Daten in Containern beim Löschen verloren gehen, speichert man sie in Volumes auf dem Host.

### Benennung & Pfad:
**Named Volume:**
```bash
docker run -v mein_specher:/pfad/im/container ...
```
(Docker verwaltet den Speicherort).
```bash
Bind Mount: docker run -v /mein/lokaler/pfad:/pfad/im/container ...
```
(Du definierst den genauen Ordner auf deinem Rechner).
    
## 🔴 Netzwerke mit Docker

**Standardaufbau:** Docker erstellt automatisch ein bridge-Netzwerk. Container darin können über IP-Adressen kommunizieren, aber nicht standardmässig über Namen (ausser im benutzerdefinierten Netzwerk).

**Eigene Definition:** Mit...
```bash
docker network create mein-netz
```
...erstellst du ein isoliertes Netzwerk. Container im selben Netzwerk können sich gegenseitig über ihren Containernamen erreichen (integriertes DNS).

## 🟠 Docker Befehle

**Platzbedarf ermitteln:**
```bash
docker system df
```

### Löschen:

**Container:**
```bash
docker rm <ID>
```
(muss gestoppt sein)
    
**Images:**
```bash
docker rmi <ID>
```
    
**Volumes verwalten:**
```bash
docker volume ls
```
(anzeigen)
```bash
docker volume rm <name>
```
(löschen).

**Gesamtüberblick:**
```bash
docker ps -a
```
(alle Container)
```bash
docker images
```
(alle Images).
    
**Speicher freigeben:** 
```bash
docker system prune
```
(entfernt alle ungenutzten Daten wie gestoppte Container und ungenutzte Netzwerke).
