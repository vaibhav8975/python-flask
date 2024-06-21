pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/flask-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("yourusername/flask-app:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        docker.image("yourusername/flask-app:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deployment steps (e.g., Kubernetes, ECS, or SSH to a server)
                sh 'echo "Deploying application..."'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
