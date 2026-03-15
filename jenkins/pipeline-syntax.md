# Jenkins - Pipeline et CI/CD

## Pipeline declarative (structure de base)

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven'    // Configure dans Jenkins > Global Tool Configuration
    }

    environment {
        DOCKER_IMAGE = 'user/my-app'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        success { echo 'Build reussi!' }
        failure { echo 'Build echoue!' }
    }
}
```

## Credentials

```groovy
// Utiliser des credentials stockes dans Jenkins (jamais en dur)
withCredentials([usernamePassword(
    credentialsId: 'docker-hub-repo',
    passwordVariable: 'PASS',
    usernameVariable: 'USER'
)]) {
    sh "echo $PASS | docker login -u $USER --password-stdin"
}

// SSH
sshagent(['ec2-server-key']) {
    sh "scp file.txt ec2-user@host:/path"
    sh "ssh ec2-user@host 'bash deploy.sh'"
}
```

## Shared Library

Structure d'une shared library :
```
jenkins-shared-library/
├── vars/              # Fonctions appelables depuis le Jenkinsfile
│   ├── buildJar.groovy
│   └── buildImage.groovy
└── src/com/example/   # Classes Groovy reutilisables
    └── Docker.groovy
```

Utilisation dans le Jenkinsfile :
```groovy
// Charger la library
library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource', remote: 'https://github.com/user/shared-lib.git']
)

// Appeler les fonctions
buildJar()
buildImage('my-app:1.0')
```

## Versioning automatique avec Maven

```groovy
// Incrementer la version patch (ex: 1.0.2 -> 1.0.3)
sh 'mvn build-helper:parse-version versions:set \
    -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
    versions:commit'

// Lire la nouvelle version
def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
def version = matcher[0][1]
```

## Configuration Jenkins (checklist)

1. **Installer les plugins** : Pipeline, Git, Docker Pipeline, SSH Agent, Credentials
2. **Configurer les tools** : Maven, Java (dans Global Tool Configuration)
3. **Ajouter les credentials** : Docker Hub, Git, SSH keys (dans Credentials)
4. **Creer un Pipeline job** : pointer vers le repo Git contenant le Jenkinsfile
5. **Configurer le webhook** : GitHub/GitLab webhook pour trigger le build au push
