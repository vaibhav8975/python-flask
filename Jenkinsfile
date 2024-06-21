pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/vaibhav8975/python-flask.git'
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
                    docker.build("flask-app:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deployment steps (e.g., running the Docker container)
                sh 'docker run -d -p 5000:5000 flask-app:${env.BUILD_NUMBER}'
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
