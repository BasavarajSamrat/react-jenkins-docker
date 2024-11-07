pipeline {
    agent any

    environment {
        DOCKER_USERNAME = 'basavarajsamrat' // Your Docker Hub username
        DOCKER_PASSWORD = 'Bassu5@2002docker' // Your Docker Hub password (you may want to use credentials instead of hardcoding this)
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/BasavarajSamrat/react-jenkins-docker.git' // Your GitHub repository URL
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
                // Log in to Docker Hub
                sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
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
