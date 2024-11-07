pipeline {
    agent any

    environment {
        DOCKER_USERNAME = 'basavarajsamrat' // Docker Hub username
        // Use Jenkins credentials instead of hardcoding the password
        // Create credentials in Jenkins and refer to them by their ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/BasavarajSamrat/react-jenkins-docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it with the build ID
                    docker.build("${DOCKER_USERNAME}/react-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    // Push the image to Docker Hub
                    docker.image("${DOCKER_USERNAME}/react-app:${env.BUILD_ID}").push()
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Take down any running containers and bring up the new ones
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            // Always take down the containers after the pipeline completes
            sh 'docker-compose down'
        }
    }
}
