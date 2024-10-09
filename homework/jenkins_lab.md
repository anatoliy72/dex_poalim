
![01](https://github.com/user-attachments/assets/03a7fd0f-3d33-4de6-8870-0ca32d2f3e2e)
![02](https://github.com/user-attachments/assets/3c1b9a7c-d985-4acf-b951-2e24141b86d9)
![03](https://github.com/user-attachments/assets/3b7f6971-379e-4f8a-85b9-7ec78d28b2dc)
![04](https://github.com/user-attachments/assets/7964292a-cc91-45fd-ad01-5e747947ad00)
![05](https://github.com/user-attachments/assets/270b074b-f5dd-4498-8ddf-02ed37520a6e)
![06](https://github.com/user-attachments/assets/66c5b411-6662-4cea-884b-2c18248b90fa)
![07](https://github.com/user-attachments/assets/7b412d65-9a06-47a9-85e3-ce184d9b8587)
![08](https://github.com/user-attachments/assets/a418251d-5634-4a3c-89c4-8e66dacdd8ce)

dockerfile
```
FROM jenkins/jenkins:lts


RUN jenkins-plugin-cli --plugins \
    git \
    docker-workflow \
    docker-plugin

USER root


RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg2 lsb-release && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y docker-ce-cli


ARG DOCKER_GID=124


RUN groupadd -for -g ${DOCKER_GID} docker && \
    usermod -aG docker jenkins

USER jenkins


ENV JENKINS_USER=admin
ENV JENKINS_PASS=admin123


ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"


COPY default-user.groovy /usr/share/jenkins/ref/init.groovy.d/

VOLUME /var/jenkins_home



Jenkinsfile pipeline
pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        IMAGE_NAME = 'dijem72/jenkins-demo' 
        DOCKER_IMAGE_TAG = "${env.JOB_NAME}_${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/anatoliy72/dex_poalim.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                sh "docker rmi ${IMAGE_NAME}:${DOCKER_IMAGE_TAG} || true"
                sh "docker rmi ${IMAGE_NAME}:latest || true"
            }
        }
        success {
            echo "Docker image '${IMAGE_NAME}:${DOCKER_IMAGE_TAG}' pushed successfully."
        }
    }
}

*** results
ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
