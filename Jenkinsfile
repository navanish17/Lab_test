pipeline {
    agent any
    
    triggers {
        pollSCM('H/2 * * * *')  // Auto-check GitHub every 2 minutes
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                echo "=== Checking out code from GitHub ==="
                git branch: 'b1', url: 'https://github.com/navanish17/Lab_test.git'
            }
        }
        
        stage('Clean Old Container') {
            steps {
                echo "=== Cleaning old Docker container ==="
                bat '''
                    docker stop demo 2>nul || echo No container to stop
                    docker rm demo 2>nul || echo No container to remove
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "=== Building Docker Image: demo:latest ==="
                bat 'docker build -t demo:latest .'
            }
        }
        
        stage('Verify Image') {
            steps {
                echo "=== Verifying Docker Image ==="
                bat 'docker images demo:latest'
            }
        }
        
        stage('Run Container') {
            steps {
                echo "=== Running Docker Container ==="
                bat 'docker run -d --name demo -p 5000:5000 demo:latest'
                bat 'docker ps'
            }
        }
    }
    
    post {
        success {
            echo "✅ SUCCESS! Container 'demo' is running on http://localhost:5000"
        }
        failure {
            echo "❌ FAILED! Check console output for errors."
        }
    }
}
