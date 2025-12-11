pipeline {
    agent any
    environment {
        // 🔴 REPLACE THIS WITH YOUR ACTUAL DOCKER HUB USERNAME
        DOCKER_IMAGE = 'jadotuyi/my-web-app'  // Format: your-docker-id/your-image-name
        DOCKER_CREDENTIALS_ID = 'd19314a5a-c615-404c-9fae-76aebeb9076a'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
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
        stage('Deploy to Local Docker') {
            steps {
                // ⚠️ Changed from 'sh' to 'bat' for Windows
                bat '''
                    docker rm -f my-web-app || echo "Container did not exist, continuing..."
                    docker run -d --name my-web-app -p 8080:80 jadotuyi/my-web-app:latest
                '''
            }
        }
    }
}