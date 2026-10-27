pipeline {
    agent any

    environment {
        APP_NAME = 'student-app'
        APP_PORT = '5000'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Checking out source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📦 Installing Python dependencies...'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running unit tests...'
                bat 'python -m pytest test_app.py -v'
            }
        }

        stage('Docker Build') {
            steps {
                echo '🐳 Building Docker image...'
                bat "docker build -t %APP_NAME% ."
            }
        }

        stage('Docker Deploy') {
            steps {
                echo '🚀 Deploying container...'
                // Stop and remove existing container if running
                bat "docker stop %APP_NAME% || exit 0"
                bat "docker rm %APP_NAME% || exit 0"
                // Run new container
                bat "docker run -d --name %APP_NAME% -p %APP_PORT%:%APP_PORT% %APP_NAME%"
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '✅ Verifying deployment...'
                bat 'timeout /t 5 /nobreak > nul'
                bat "docker ps --filter name=%APP_NAME%"
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
    }
}
