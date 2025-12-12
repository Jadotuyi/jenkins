pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'jadotuyi/jenkins'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps {
                echo "Building project..."
                bat 'dir'
            }
        }

        stage('Test') {
            steps { echo "Running tests..." }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                script {
                    bat """
                    docker rm -f jenkins || true
                    docker run -d --name jenkins -p 8081:80 ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }
    }
}