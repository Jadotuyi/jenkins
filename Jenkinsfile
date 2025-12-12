pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building project..."
                bat 'dir'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."

                    def app = docker.build("jadotuyi/jenkins-demo")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        def app = docker.image("jadotuyi/jenkins-demo")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                script {
                    echo "Deploying container..."
                    bat 'docker run -d -p 3000:3000 jadotuyi/jenkins-demo'
                }
            }
        }
    }
}
