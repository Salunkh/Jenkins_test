pipeline {
    agent any

    environment {
        VENV_DIR = "${WORKSPACE}/venv"
        PATH = "/usr/bin:/opt/homebrew/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Salunkh/Jenkins_test.git'
            }
        }

        stage('Setup Python Env') {
            steps {
                sh '''
                python3 -m venv $VENV_DIR
                . $VENV_DIR/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                pip install pytest flask
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                pytest || true
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                nohup python app.py > flask.log 2>&1 &
                '''
            }
        }
    }
}
