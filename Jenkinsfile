pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                withEnv(['PATH+DOCKER=/usr/local/bin']) {
                    sh 'docker --version'
                }
            }
        }
        stage('Clone Repository') {
            steps {
                retry(3) {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        git branch: 'main', url: 'https://github.com/rizgif/ci-cd-demo.git', credentialsId: 'github-pat'
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        docker.build('rizgif/ci-cd-demo:latest', '.')
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                            docker.image('rizgif/ci-cd-demo:latest').push()
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                try {
                    withEnv(['PATH+DOCKER=/usr/local/bin']) {
                        sh 'docker system prune -f'
                    }
                } catch (Exception e) {
                    echo 'Error during Docker cleanup: ' + e.toString()
                }
            }
        }
    }
}
