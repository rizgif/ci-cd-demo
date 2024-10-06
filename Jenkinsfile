pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                script {
                    // Check if Docker is available
                    try {
                        sh 'docker --version'
                        echo "Docker is accessible."
                    } catch (Exception e) {
                        error "Docker is not available: ${e}"
                    }
                }
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
                    // Build the Docker image using the current workspace as context
                    docker.build('rizgif/ci-cd-demo:latest', '.')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image('rizgif/ci-cd-demo:latest').push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up dangling images after pipeline completion
            script {
                try {
                    sh 'docker system prune -f'
                } catch (Exception e) {
                    echo 'Error during Docker cleanup: ' + e.toString()
                }
            }
        }

        failure {
            echo 'Pipeline failed.'
        }

        success {
            echo 'Pipeline succeeded.'
        }
    }
}
