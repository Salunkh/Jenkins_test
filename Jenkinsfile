pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                // Pull your code from GitHub
                git branch: 'main', url: 'https://github.com/Salunkh/Jenkins_test.git'
            }
        }

        stage('Setup Python Env') {
            steps {
                sh '''
                # Create a fresh virtual environment in the workspace
                python3 -m venv venv

                # Activate it (use POSIX '.' for portability)
                . venv/bin/activate

                # Upgrade pip and install dependencies
                pip install --upgrade pip
                pip install -r requirements.txt

                # Install test tools (dev dependency)
                pip install pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                # Run pytest quietly; fail the build if tests fail
                pytest -q
                '''
            }
        }

        stage('Deploy (Run Flask)') {
            steps {
                sh '''
                . venv/bin/activate

                # Stop an older instance if it exists (using saved PID)
                if [ -f flask.pid ]; then
                  if kill -0 $(cat flask.pid) 2>/dev/null; then
                    echo "Stopping previous Flask process $(cat flask.pid)"
                    kill $(cat flask.pid) || true
                    # Give it a moment, force kill if still alive
                    sleep 1
                    kill -9 $(cat flask.pid) 2>/dev/null || true
                  fi
                  rm -f flask.pid
                fi

                # Start Flask in the background; capture logs and PID
                nohup python app.py > flask.log 2>&1 &
                echo $! > flask.pid
                echo "Started Flask with PID $(cat flask.pid)"

                # Wait until the app is responsive (up to ~15s)
                for i in $(seq 1 15); do
                  if curl -fsS http://localhost:5000/ >/dev/null; then
                    echo "Flask is up at http://localhost:5000"
                    exit 0
                  fi
                  echo "Waiting for Flask to start... ($i/15)"
                  sleep 1
                done

                echo "ERROR: Flask did not start in time. Recent log output:"
                tail -n +1 flask.log || true
                exit 1
                '''
            }
        }
    }

    post {
        always {
            echo "Build done. Logs live at: ${env.WORKSPACE}/flask.log"
        }
        failure {
            echo "Build failed. Check ${env.WORKSPACE}/flask.log for details."
        }
    }
}
