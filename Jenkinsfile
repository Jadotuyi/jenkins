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
                echo "Building the project..."
                sh 'npm install'      // or bat 'npm install' on Windows
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'npm test'         // or bat 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh 'echo Deployment successful!'
            }
        }
    }
}
