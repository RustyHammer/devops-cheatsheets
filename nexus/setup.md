# Nexus Repository Manager

## What is Nexus?

An artifact storage server (JARs, WARs, Docker images, npm packages). Like a "private Docker Hub" but for all types of packages.

## Repository Types

| Type | Role | Example |
|------|------|---------|
| **hosted** | Store YOUR artifacts | `maven-snapshots`, `maven-releases` |
| **proxy** | Cache for external repos | Proxy to Maven Central, npm registry |
| **group** | Aggregator of multiple repos under a single URL | Combines hosted + proxy |

Analogy: The **proxy** is like a CDN - it downloads once from the Internet and caches locally. The **group** is a single facade that searches across all repos.

## Quick Installation

```bash
# On a Linux server with Docker
docker run -d -p 8081:8081 --name nexus \
    -v nexus-data:/nexus-data \
    sonatype/nexus3
```

Initial admin password:
```bash
docker exec nexus cat /nexus-data/admin.password
```

## Maven Configuration

See the [nexus-artifact-management](https://github.com/RustyHammer/nexus-artifact-management) project for complete `pom.xml` and `settings.xml` examples.

## Gradle Configuration

See the [nexus-artifact-management](https://github.com/RustyHammer/nexus-artifact-management) project for complete `build.gradle` and `gradle.properties` examples.

## Publishing Commands

```bash
# Maven
mvn deploy

# Gradle
gradle publish
```

## Best Practices

- **Backups**: Back up `/nexus-data` regularly
- **Cleanup policies**: Configure automatic deletion of old snapshots
- **HTTPS**: Put a reverse proxy (Nginx) with an SSL certificate in front of Nexus
- **Roles**: Create specific roles for CI/CD users (principle of least privilege)
