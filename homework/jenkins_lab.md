
setup jenkins  via dockerfile

```dockerfile
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
```
```bash
# Check the group information for the docker group inside the Jenkins container:
docker exec -it jenkins getent group docker
# Display the user ID and group memberships of the jenkins user inside the Jenkins container
docker exec -it jenkins id jenkins
# List the permissions of the Docker socket inside the Jenkins container
docker exec -it jenkins ls -l /var/run/docker.sock
# Run docker ps as the jenkins user inside the Jenkins container
docker exec -u jenkins -it jenkins docker ps
```

```pipeline
pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'dijem72'
        IMAGE_NAME = 'dijem1972/jenkins-demo'
        DOCKER_IMAGE_TAG = "${env.JOB_NAME}_${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/anatoliy72/dex_poalim.git', branch: 'main'
            }
        }
        stage('List files') {
            steps {
                sh 'ls -la homework/'
            }
        }
        stage('Build Docker Image') {
            steps {
                dir('homework') {
                    script {
                        dockerImage = docker.build("${IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                    }
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
```
![image](https://github.com/user-attachments/assets/09b34ed7-8016-474d-b09f-3638b43d6e48)
![image](https://github.com/user-attachments/assets/c622f550-6b2f-4926-b28a-eec98bd4ca64)



![01](https://github.com/user-attachments/assets/03a7fd0f-3d33-4de6-8870-0ca32d2f3e2e)
![02](https://github.com/user-attachments/assets/3c1b9a7c-d985-4acf-b951-2e24141b86d9)
![03](https://github.com/user-attachments/assets/3b7f6971-379e-4f8a-85b9-7ec78d28b2dc)
![04](https://github.com/user-attachments/assets/7964292a-cc91-45fd-ad01-5e747947ad00)
![05](https://github.com/user-attachments/assets/270b074b-f5dd-4498-8ddf-02ed37520a6e)
![06](https://github.com/user-attachments/assets/66c5b411-6662-4cea-884b-2c18248b90fa)
![07](https://github.com/user-attachments/assets/7b412d65-9a06-47a9-85e3-ce184d9b8587)
![08](https://github.com/user-attachments/assets/a418251d-5634-4a3c-89c4-8e66dacdd8ce)
