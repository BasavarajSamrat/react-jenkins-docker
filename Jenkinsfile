pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/BasavarajSamrat/react-jenkins-docker.git
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
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }
