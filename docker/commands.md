# Docker - Commandes et bonnes pratiques

## Images

```bash
docker build -t my-app:1.0 .       # Construire une image
docker images                        # Lister les images
docker rmi image_name                # Supprimer une image
docker image prune -f                # Supprimer les images inutilisees
docker tag my-app:1.0 user/my-app:1.0  # Tagger pour un registry
docker push user/my-app:1.0         # Pousser vers Docker Hub
```

## Containers

```bash
docker run -d -p 8080:8080 my-app   # Lancer en arriere-plan avec port mapping
docker ps                            # Containers en cours
docker ps -a                         # Tous les containers (y compris arretes)
docker stop <container_id>           # Arreter
docker rm <container_id>             # Supprimer
docker logs <container_id>           # Voir les logs
docker logs -f <container_id>        # Logs en temps reel
docker exec -it <container_id> sh   # Ouvrir un shell dans le container
```

## Docker Compose

```bash
docker-compose up -d                 # Lancer la stack en arriere-plan
docker-compose down                  # Arreter et supprimer les containers
docker-compose ps                    # Statut des services
docker-compose logs -f               # Logs de tous les services
docker-compose build                 # Reconstruire les images
docker-compose up -d --build         # Reconstruire et relancer
```

## Debug

```bash
docker inspect <container_id>        # Details complets d'un container
docker stats                         # Utilisation CPU/RAM en temps reel
docker network ls                    # Lister les networks
docker volume ls                     # Lister les volumes
```

## Bonnes pratiques Dockerfile

```dockerfile
# 1. Utiliser une image de base legere
FROM node:20-alpine    # PAS node:20 (900MB vs 120MB)

# 2. Utiliser un utilisateur non-root
RUN addgroup -S app && adduser -S app -G app
USER app

# 3. Copier les dependances avant le code (cache Docker)
COPY package.json .
RUN npm install
COPY . .                # Le code change souvent, les deps non

# 4. Un seul processus par container
CMD ["node", "server.js"]
```

### Multi-stage build

```dockerfile
# Stage build : gros (Maven, JDK complet)
FROM maven:3.9-amazoncorretto-17 AS build
COPY . .
RUN mvn package

# Stage runtime : leger (JRE seulement)
FROM amazoncorretto:17-alpine
COPY --from=build /app/target/*.jar app.jar
CMD ["java", "-jar", "app.jar"]
```

Avantage : l'image finale ne contient que le necessaire pour l'execution (~120MB au lieu de ~400MB).
