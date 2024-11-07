pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "basavarajsamrat/react-app:${env.BUILD_ID}"
    }
    stages {
        stage('Checkout') {
            steps {
                 git branch: "main", url: 'https://github.com/BasavarajSamrat/react-jenkins-docker.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Stop any running containers and start the services with Docker Compose
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                    // Optionally, check if containers are running
                    sh 'docker-compose ps'
                }
            }
        }
    }
    post {
        always {
            // Clean up by bringing down Docker Compose services
            sh 'docker-compose down'
        }
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed. Please check the logs for errors."
        }
    }
}
