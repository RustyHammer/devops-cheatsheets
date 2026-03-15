# Docker - Commands and Best Practices

## Images

```bash
docker build -t my-app:1.0 .       # Build an image
docker images                        # List images
docker rmi image_name                # Remove an image
docker image prune -f                # Remove unused images
docker tag my-app:1.0 user/my-app:1.0  # Tag for a registry
docker push user/my-app:1.0         # Push to Docker Hub
```

## Containers

```bash
docker run -d -p 8080:8080 my-app   # Run in background with port mapping
docker ps                            # Running containers
docker ps -a                         # All containers (including stopped)
docker stop <container_id>           # Stop
docker rm <container_id>             # Remove
docker logs <container_id>           # View logs
docker logs -f <container_id>        # Real-time logs
docker exec -it <container_id> sh   # Open a shell in the container
```

## Docker Compose

```bash
docker-compose up -d                 # Start the stack in background
docker-compose down                  # Stop and remove containers
docker-compose ps                    # Service status
docker-compose logs -f               # Logs for all services
docker-compose build                 # Rebuild images
docker-compose up -d --build         # Rebuild and restart
```

## Debug

```bash
docker inspect <container_id>        # Full details of a container
docker stats                         # Real-time CPU/RAM usage
docker network ls                    # List networks
docker volume ls                     # List volumes
```

## Dockerfile Best Practices

```dockerfile
# 1. Use a lightweight base image
FROM node:20-alpine    # NOT node:20 (900MB vs 120MB)

# 2. Use a non-root user
RUN addgroup -S app && adduser -S app -G app
USER app

# 3. Copy dependencies before code (Docker cache)
COPY package.json .
RUN npm install
COPY . .                # Code changes often, deps don't

# 4. One process per container
CMD ["node", "server.js"]
```

### Multi-stage Build

```dockerfile
# Build stage: heavy (Maven, full JDK)
FROM maven:3.9-amazoncorretto-17 AS build
COPY . .
RUN mvn package

# Runtime stage: lightweight (JRE only)
FROM amazoncorretto:17-alpine
COPY --from=build /app/target/*.jar app.jar
CMD ["java", "-jar", "app.jar"]
```

Advantage: the final image contains only what's needed for execution (~120MB instead of ~400MB).
