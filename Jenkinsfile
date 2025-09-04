pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'custom', url: 'https://github.com/Salunkh/Jenkins_test.git'
            }
        }

        stage('Setup Python Env') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install flask pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                python -m pytest -q || true
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                . venv/bin/activate
                # kill old Flask app if running
                pkill -f "python app.py" || true
                # run Flask app in background
                nohup python app.py > flask.log 2>&1 &
                '''
            }
        }
    }
}
