pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps  {
                 git branch: 'main', url: 'https://github.com/Salunkh/Jenkins_test.git'
            }
        }
        stage('Setup Python Env') {
            steps {
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip3 install --upgrade pip
                pip3 install -r requirements.txt
                '''
            }
        }
        stage('Run tests') {
            steps {
                sh '''
                source venv/bin/activate
                pytest || true
                '''
            }
        }
        stage('Run Flask App') {
            steps {
                sh '''
                source venv/bin/activate
                nohup python app.py &
                '''
            }
        }
    }
}
