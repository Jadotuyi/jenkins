pipeline {
    agent any
    environment {
        // ⚠️ CHANGE THESE TWO LINES:
        DOCKER_IMAGE = 'jadotuyi/my-web-app'  // e.g., johndoe123/my-web-app
        DOCKER_CREDENTIALS_ID = 'jadotuyi'  // Simple name, not a UUID
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Verify Docker Setup') {
            steps {
                bat '''
                    echo "Checking Docker installation..."
                    docker --version || echo "ERROR: Docker not found!"
                    echo "Checking if Docker daemon is running..."
                    docker ps
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${DOCKER_IMAGE}"
                    // First, check if we have the necessary files
                    bat 'dir /b'
                    bat 'type Dockerfile || echo "ERROR: No Dockerfile found!"'
                    
                    // Then build
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing to Docker Hub with credentials ID: ${DOCKER_CREDENTIALS_ID}"
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy to Local Docker') {
            steps {
                bat '''
                    echo "Stopping any existing container..."
                    docker stop my-web-app 2>nul || echo "No container to stop"
                    docker rm my-web-app 2>nul || echo "No container to remove"
                    
                    echo "Running new container..."
                    docker run -d --name my-web-app -p 8080:80 YOUR_DOCKERHUB_USERNAME/my-web-app:latest
                    
                    echo "Waiting for container to start..."
                    timeout /t 5 /nobreak > nul
                    
                    echo "Checking container status..."
                    docker ps | findstr "my-web-app"
                    
                    echo "Testing application..."
                    curl http://localhost:8080 || echo "Application not responding yet"
                '''
            }
        }
    }
    post {
        always {
            bat '''
                echo "=== DEBUG INFO ==="
                docker ps -a
                docker images | findstr "my-web-app"
            '''
        }
    }
}