pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps  {
                 git branch: 'main', url: 'https://github.com/Salunkh/Jenkins_test.git'
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run tests') {
            steps {
                sh 'pytest || true'   // add tests in later versions
            }
        }
        stage('Run Flask App') {
            steps {
                sh 'nohup python app.py &'
            }
        }
    }
}
