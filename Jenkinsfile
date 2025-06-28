pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nodejs-app'
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE = "${DOCKER_REGISTRY}/sagar592/${IMAGE_NAME}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/sagar592/pipeli.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'npm install'
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").run('-p 8080:8080')
                }
            }
        }
    }
}
