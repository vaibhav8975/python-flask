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
                script {
                    sh 'python3 -m venv venv'  // Create virtual environment
                    sh '. venv/bin/activate && python3 -m pip install -r requirements.txt'  // Activate and install dependencies
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh '. venv/bin/activate && pytest'  // Activate virtual environment and run tests
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t flask-app:${env.BUILD_NUMBER} .'  // Build Docker image
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker run -d -p 5000:5000 flask-app:${env.BUILD_NUMBER}'  // Deploy Docker container
                }
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
