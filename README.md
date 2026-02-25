# M169-P1

## 🟢 **Container, Docker Image, Dockerfile, Docker-Container, Docker-Registry erklären**

### Container

Ein **Container** ist eine isolierte Laufzeitumgebung für eine Applikation.  
Er nutzt Kernel-Features wie **Namespaces** und **cgroups**, teilt sich aber den Host-Kernel.

Eigenschaften:

- leichtgewichtig
    
- schnell startbar
    
- reproduzierbar
    
- isoliert (Prozesse, Netzwerk, Filesystem)
    

---

### Docker Image

Ein **Image** ist eine **read-only Vorlage**, aus der Container erstellt werden.  
Es besteht aus mehreren Layern (UnionFS).

Beispiel:
```bash
docker pull nginx
```

---

### Dockerfile

Textdatei zur Beschreibung eines Images (Infrastructure as Code).

Beispiel:

FROM node:20  
WORKDIR /app  
COPY package.json .  
RUN npm install  
COPY . .  
CMD ["node", "server.js"]

Image bauen:

```bash
docker build -t myapp:1.0 .
```

---

### Docker-Container

Instanz eines Images (read-write Layer oben drauf).

Container starten:
```bash
docker run -it ubuntu bash
```

---

### Docker-Registry

Speicherort für Images.

Beispiele:

- Docker Hub
    
- Private Registry im Unternehmen
    

Push:

```bash
docker push username/image:tag
```

---

## 🟢 **Virtualisierung vs Cloud unterscheiden**

### Virtualisierung

Technologie zur Erstellung virtueller Maschinen.

Beispiel:

- VMware ESXi
    
- VirtualBox
    

Merkmale:

- vollständiges Gast-OS
    
- höherer Ressourcenverbrauch
    
- Hardwarevirtualisierung
    

---

### Cloud

Betriebsmodell / Service-Modell.

Beispiele:

- Amazon Web Services
    
- Microsoft Azure
    
- Google Cloud
    

Merkmale:

- On-Demand
    
- Pay-per-Use
    
- Skalierbarkeit
    

Kurz:

> Virtualisierung = Technik  
> Cloud = Betriebs- und Servicemodell

---

## 🟢 **Docker-Container ausführen und interaktiv verwenden**

Container starten:

```bash
docker run -it ubuntu bash
```

Parameter:

- `-i` → interactive
    
- `-t` → TTY
    
- `--name` → Container benennen
    
- `-d` → detached
    

Beispiel mit Namen:

```bash
docker run -it --name testcontainer ubuntu bash
```

In laufenden Container:

```bash
docker exec -it testcontainer bash
```

Logs anzeigen:
```bash
docker logs testcontainer
```

---

## 🟡 **Warum Portweiterleitungen wichtig sind + umsetzen**

Container haben eigenes Netzwerk.

Ohne Portmapping:

- Service ist nur intern erreichbar
    

Mit Portmapping:

```bash
docker run -p 8080:80 nginx
```

Schema:

HostPort:ContainerPort

Beispiel:  
Browser → `localhost:8080` → Container Port 80

Mehrere Ports:
```bash
-p 3000:3000 -p 5432:5432
```

---

## 🟡 **Mit Docker Volumes umgehen**

Container sind ephemer.  
Volumes speichern persistente Daten.

Volume erstellen:

```bash
docker volume create myvolume
```

Verwenden:

```bash
docker run -v myvolume:/data ubuntu
```

Bind Mount:
```bash
docker run -v /host/path:/container/path ubuntu
```

Unterschied:

- Volume → Docker verwaltet
    
- Bind Mount → Host-Dateisystem
    

---

## 🟡 **Volumes benennen und in definiertem Verzeichnis speichern**

Standardpfad:

/var/lib/docker/volumes/

Benanntes Volume:

```bash
docker run -v mydbdata:/var/lib/mysql mysql
```

Explizites Host-Verzeichnis:

```bash
bashdocker run -v /srv/mysql:/var/lib/mysql mysql
```

---

## 🔴 **Standard-Netzwerkaufbau in Docker verstehen**

Beim Start erstellt Docker:

- bridge (default)
    
- host
    
- none
    

### Bridge-Netzwerk

Default für Container.

Interne Kommunikation via Container-IP.

Anzeigen:
```bash
docker network ls  
```
```bash
docker network inspect bridge
```

IP anzeigen:
```bash
docker inspect containername
```
Architektur:

Container ↔ bridge ↔ Host ↔ Internet

---

## 🔴 **Eigene Netzwerkdefinitionen umsetzen**

Eigenes Netzwerk:
```bash
docker network create mynetwork
```

Container verbinden:
```bash
docker run -d --name app --network mynetwork nginx  
```
```bash
docker run -d --name db --network mynetwork mysql
```

Vorteil:

- DNS-basierte Namensauflösung
    
- bessere Isolation
    
- Microservice-Architektur
    

Netzwerktyp definieren:
```bash
docker network create --driver bridge mynetwork
```
---

## 🔴 **Docker administrieren (wichtige Befehle)**

### Überblick
```bash
docker ps
```
```bash  
docker ps -a  
```
```bash
docker images  
```
```bash
docker system df  
```
```bash
docker system info
```
---

### Platzbedarf ermitteln
```bash
docker system df  
```
```bash
docker image ls
```
---

### Container löschen
```bash
docker rm containername  
```
```bash
docker rm -f containername
```
---

### Images löschen
```bash
docker rmi imagename
```
---

### Volumes verwalten
```bash
docker volume ls  
```
```bash
docker volume inspect myvolume  
```
```bash
docker volume rm myvolume
```
---

### Ungenutzten Speicher freigeben
```bash
docker system prune  
```
```bash
docker system prune -a  
```
```bash
docker volume prune
```
Achtung:

- `-a` löscht alle ungenutzten Images
    
- nicht rückgängig machbar
