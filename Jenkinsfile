pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/rizgif/ci-cd-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('rizgif/ci-cd-demo:latest')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image('rizgif/ci-cd-demo:latest').push()
                    }
                }
            }
        }
    }
}
