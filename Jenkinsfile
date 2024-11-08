pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/BasavarajSamrat/react-jenkins-docker.git'
               
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'cp -r build/* /opt/tomcat/webapps/ROOT/'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
            deleteDir()
        }
    }
}
