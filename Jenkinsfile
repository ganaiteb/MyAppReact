pipeline {
    agent any
    environment {
        DOCKERHUB = 'itebbb'
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build Server') {
            steps { bat "docker build -t %DOCKERHUB%/mern-server:latest ./Server" }
        }
        stage('Build Client') {
            steps { bat "docker build -t %DOCKERHUB%/mern-client:latest ./Client" }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'U', passwordVariable: 'P')]) {
                    bat "docker login -u %U% -p %P%"
                    bat "docker push %DOCKERHUB%/mern-server:latest"
                    bat "docker push %DOCKERHUB%/mern-client:latest"
                }
            }
        }
    }
}