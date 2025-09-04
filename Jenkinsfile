pipeline {
    agent any

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
                source venv/bin/activate
                pytest || true
                '''
            }
        }

        stage('Run Flask App') {
            steps {
                sh '''
                source venv/bin/activate

                # Kill old Flask tmux session if exists
                tmux kill-session -t flask || true

                # Start Flask in detached tmux session
                tmux new-session -d -s flask 'python app.py'

                echo "Flask app started in tmux session 'flask'. Access it at http://localhost:5000"
                '''
            }
        }
    }
}
