
# docker_exam

## Docker – Commandes de base

### Affichage de la version de Docker
```bash
docker --version
```

### Infos système Docker
```bash
docker info
```

### Aide générale
```bash
docker help
```

## Conteneur Docker

### Lister les conteneurs en cours d’exécution
```bash
docker ps
```

### Lister tous les conteneurs (même arrêtés)
```bash
docker ps -a
```

### Lancer un conteneur interactif basé sur Ubuntu
```bash
docker run -it ubuntu bash
```

### Lancer un conteneur nginx en arrière-plan
```bash
docker run -d --name mon_nginx nginx
```

### Arrêter le conteneur mon_nginx
```bash
docker stop mon_nginx
```

### Redémarrer le conteneur mon_nginx
```bash
docker restart mon_nginx
```

### Afficher les logs du conteneur mon_nginx
```bash
docker logs mon_nginx
```

### Exécuter une commande dans le conteneur mon_nginx
```bash
docker exec -it mon_nginx bash
```

### Supprimer le conteneur mon_nginx
```bash
docker rm mon_nginx
```

## Docker Image

### Lister les images locales
```bash
docker images
```

### Télécharger l’image nginx
```bash
docker pull nginx
```

### Construire une image avec le nom monimage
```bash
docker build -t monimage .
```

### Taguer l’image pour un repository fictif Docker Hub
```bash
docker tag monimage monutilisateur/monimage:latest
```

### Pousser l’image vers un registry
```bash
docker push monutilisateur/monimage:latest
```

### Supprimer une image
```bash
docker rmi 123456789abc
```
> `123456789abc` est l'`image_id`

## Dockerfile

### Créer le fichier index.js
```bash
nano index.js
```
```js
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello Docker!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### Créer le Dockerfile
```bash
nano Dockerfile
```
```Dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

### Construire l’image Docker
```bash
docker build -t hello-docker .
```

### Exécuter le conteneur
```bash
docker run -p 3000:3000 hello-docker
```

### Tester
```bash
curl http://localhost:3000
```

## Docker Compose

### Démarrer les services
```bash
docker-compose up
```

### Démarrer en arrière-plan
```bash
docker-compose up -d
```

### Arrêter et supprimer les conteneurs
```bash
docker-compose down
```

### Construire les images
```bash
docker-compose build
```

### Afficher les logs
```bash
docker-compose logs
```

### Affiche l'état des services
```bash
docker-compose ps
```

### Créer un fichier PHP
```bash
nano php_app/index.php
```
```php
<?php
echo "<h1>Bienvenue sur mon application PHP via Docker Compose !</h1>";
echo "<p>Le serveur PHP fonctionne sur le port 3000.</p>";
?>
```

### Fichier docker-compose.yml
```bash
nano docker-compose.yml
```
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
```

### Configuration nginx.conf
```bash
nano nginx.conf
```
```nginx
server {
    listen 80;

    location / {
        proxy_pass http://app:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## Docker Volume

### Créer un volume
```bash
docker volume create monvolume
```

### Lister les volumes
```bash
docker volume ls
```

### Détails d’un volume
```bash
docker volume inspect monvolume
```

### Supprimer un volume
```bash
docker volume rm monvolume
```

### Utilisation dans `docker run`
```bash
docker run -v monvolume:/data nginx
```

## Docker Swarm

### Initier un Swarm
```bash
docker swarm init
```

### Token pour ajouter un worker
```bash
docker swarm join-token worker
```

### Lister les noeuds du Swarm
```bash
docker node ls
```

### Créer un service
```bash
docker service create --replicas 3 --name web nginx
```

### Lister les services
```bash
docker service ls
```

### Tâches du service
```bash
docker service ps web
```

### Déployer une stack
```bash
docker stack deploy -c docker-compose.yml mystack
```

### Supprimer une stack
```bash
docker stack rm mystack
```

## Docker Network

### Lister les réseaux
```bash
docker network ls
```

### Créer un réseau bridge personnalisé
```bash
docker network create --driver bridge mon_reseau_bridge
```

### Inspecter le réseau
```bash
docker network inspect mon_reseau_bridge
```

### Créer deux conteneurs connectés au réseau
```bash
docker run -dit --name conteneur1 --network mon_reseau_bridge alpine sh
docker run -dit --name conteneur2 --network mon_reseau_bridge alpine sh
```

### Tester la communication
```bash
docker exec -it conteneur1 ping conteneur2
```

### Créer un réseau overlay (Swarm)
```bash
docker swarm init
docker network create --driver overlay --attachable mon_reseau_overlay
```

### Connecter un conteneur
```bash
docker network connect mon_reseau_bridge conteneur1
```

### Déconnecter un conteneur
```bash
docker network disconnect mon_reseau_bridge conteneur1
```

### Nettoyage
```bash
docker container rm -f conteneur1 conteneur2
docker network rm mon_reseau_bridge mon_reseau_overlay
```

## Nettoyage Docker

### Supprimer les objets inutilisés
```bash
docker system prune
```

### Supprimer les volumes inutilisés
```bash
docker volume prune
```

### Supprimer les images non utilisées
```bash
docker image prune
```
