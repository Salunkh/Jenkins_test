pipeline {
    agent any

    environment {
        WORKSPACE_DIR = "${env.WORKSPACE}"
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
                # Create virtual environment
                python3 -m venv venv

                # Activate virtualenv
                source venv/bin/activate

                # Upgrade pip and install dependencies
                pip install --upgrade pip
                pip install -r requirements.txt
                pip install pytest flask
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                # Activate virtualenv
                source venv/bin/activate

                # Run tests (optional)
                pytest || true
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                # Activate virtualenv
                source venv/bin/activate

                # Stop old Flask process if exists
                pkill -f "python app.py" || true

                # Run Flask app in background
                nohup python app.py > flask.log 2>&1 &
                echo "Flask app started. Access it at http://localhost:5000"
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Flask logs are in ${WORKSPACE_DIR}/flask.log"
        }
    }
}
