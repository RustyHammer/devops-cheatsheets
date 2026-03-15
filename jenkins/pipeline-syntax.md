# Jenkins - Pipeline and CI/CD

## Declarative Pipeline (basic structure)

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven'    // Configured in Jenkins > Global Tool Configuration
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
        success { echo 'Build succeeded!' }
        failure { echo 'Build failed!' }
    }
}
```

## Credentials

```groovy
// Use credentials stored in Jenkins (never hardcoded)
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

Structure of a shared library:
```
jenkins-shared-library/
├── vars/              # Functions callable from the Jenkinsfile
│   ├── buildJar.groovy
│   └── buildImage.groovy
└── src/com/example/   # Reusable Groovy classes
    └── Docker.groovy
```

Usage in the Jenkinsfile:
```groovy
// Load the library
library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource', remote: 'https://github.com/user/shared-lib.git']
)

// Call the functions
buildJar()
buildImage('my-app:1.0')
```

## Automatic Versioning with Maven

```groovy
// Increment the patch version (e.g., 1.0.2 -> 1.0.3)
sh 'mvn build-helper:parse-version versions:set \
    -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
    versions:commit'

// Read the new version
def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
def version = matcher[0][1]
```

## Jenkins Configuration (checklist)

1. **Install plugins**: Pipeline, Git, Docker Pipeline, SSH Agent, Credentials
2. **Configure tools**: Maven, Java (in Global Tool Configuration)
3. **Add credentials**: Docker Hub, Git, SSH keys (in Credentials)
4. **Create a Pipeline job**: point to the Git repo containing the Jenkinsfile
5. **Configure the webhook**: GitHub/GitLab webhook to trigger the build on push
