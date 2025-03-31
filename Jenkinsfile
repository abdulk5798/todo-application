pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = "docker-hub-credentials"
        IMAGE_NAME = 'todo-application-image'
        DOCKER_REPO = 'abdul5798/todo-application'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    credentialsId: 'github-hub-credentials',
                    url: 'https://github.com/abdulk5798/todo-application.git'

            }
        }

        stage('Build with maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build and Push Docker image') {
            steps {
                    withDockerRegistry([credentialsId: 'docker-hub-credentials', url: 'https://index.docker.io/v1/']) {
                        sh "docker build -t ${IMAGE_NAME} ."
                        sh "docker tag ${IMAGE_NAME} ${DOCKER_REPO}:${DOCKER_TAG}"
                        sh "docker push ${DOCKER_REPO}:${DOCKER_TAG}"
                }
            }
        }

        stage('Deply with Docker compose') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
