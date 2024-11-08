pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/BasavarajSamrat/react-jenkins-docker.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("basavarajsamrat/react-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Bring down any existing containers, ignoring errors if no containers are running
                    sh 'docker-compose down || true'
                    
                    // Start new containers in detached mode
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
