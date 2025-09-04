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
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest || true
                '''
            }
        }
        stage('Run Flask App') {
            steps {
                sh '''
                . venv/bin/activate
                nohup python app.py --host=0.0.0.0 --port=5001 &
                '''
            }
        }
    }
}
