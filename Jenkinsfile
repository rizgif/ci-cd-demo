pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                sh 'docker --version'
            }
        }
        stage('Clone Repository') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/rizgif/ci-cd-demo.git', credentialsId: 'github-pat'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('rizgif/ci-cd-demo:latest', '.')
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

    post {
        always {
            script {
                try {
                    sh 'docker system prune -f'
                } catch (Exception e) {
                    echo 'Error during Docker cleanup: ' + e.toString()
                }
            }
        }
    }
}
