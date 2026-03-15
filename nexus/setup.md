# Nexus Repository Manager

## Qu'est-ce que Nexus ?

Un serveur de stockage d'artefacts (JARs, WARs, images Docker, packages npm). Comme un "Docker Hub prive" mais pour tous les types de packages.

## Types de repositories

| Type | Role | Exemple |
|------|------|---------|
| **hosted** | Stocker VOS artefacts | `maven-snapshots`, `maven-releases` |
| **proxy** | Cache pour les repos externes | Proxy vers Maven Central, npm registry |
| **group** | Agregateur de plusieurs repos en un seul URL | Combine hosted + proxy |

Analogie : Le **proxy** c'est comme un CDN - il telecharge une fois depuis Internet et cache localement. Le **group** c'est une facade unique qui cherche dans tous les repos.

## Installation rapide

```bash
# Sur un serveur Linux avec Docker
docker run -d -p 8081:8081 --name nexus \
    -v nexus-data:/nexus-data \
    sonatype/nexus3
```

Mot de passe admin initial :
```bash
docker exec nexus cat /nexus-data/admin.password
```

## Configuration Maven

Voir le projet [nexus-artifact-management](https://github.com/RustyHammer/nexus-artifact-management) pour les exemples complets `pom.xml` et `settings.xml`.

## Configuration Gradle

Voir le projet [nexus-artifact-management](https://github.com/RustyHammer/nexus-artifact-management) pour les exemples complets `build.gradle` et `gradle.properties`.

## Commandes de publication

```bash
# Maven
mvn deploy

# Gradle
gradle publish
```

## Bonnes pratiques

- **Sauvegardes** : Sauvegarder `/nexus-data` regulierement
- **Cleanup policies** : Configurer la suppression automatique des vieux snapshots
- **HTTPS** : Mettre un reverse proxy (Nginx) avec un certificat SSL devant Nexus
- **Roles** : Creer des roles specifiques pour les users CI/CD (principe du moindre privilege)
