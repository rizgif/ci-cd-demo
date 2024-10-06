pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/ci-cd-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('your-dockerhub-username/ci-cd-demo:latest')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image('your-dockerhub-username/ci-cd-demo:latest').push()
                    }
                }
            }
        }
    }
}
