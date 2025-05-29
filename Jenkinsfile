pipeline {
    agent any

    environment {
        DOCKER_USERNAME = credentials('docker-username') 
        DOCKER_PASSWORD = credentials('docker-password') 
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies and Test') {
            steps {
                sh '''
                npm install
                npm test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $DOCKER_USERNAME/node-app:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                docker push $DOCKER_USERNAME/node-app:latest
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
