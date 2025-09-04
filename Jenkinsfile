pipeline {
    agent any

    environment {
        DOCKER_PATH = "/opt/homebrew/bin/docker"  // absolute path to Docker
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Salunkh/Jenkins_test.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                ${DOCKER_PATH} build -t my-flask-app .
                '''
            }
        }

        stage('Run Flask in Docker') {
            steps {
                sh '''
                # Stop old container if exists
                ${DOCKER_PATH} rm -f my-flask-container || true

                # Run new container in detached mode, map port 5000
                ${DOCKER_PATH} run -d --name my-flask-container -p 5000:5000 my-flask-app

                echo "Flask app is running in Docker container. Access it at http://localhost:5000"
                '''
            }
        }

        stage('Run Tests (Optional)') {
            steps {
                sh '''
                # Run tests inside Docker container
                ${DOCKER_PATH} exec my-flask-container pytest || true
                '''
            }
        }
    }
}
