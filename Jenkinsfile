pipeline {
    agent any
    
    triggers {
        pollSCM('H/2 * * * *')
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                echo "=== Checking out code from GitHub ==="
                git branch: 'b1', url: 'https://github.com/navanish17/Lab_test.git'
            }
        }
        
        stage('List Files') {
            steps {
                echo "=== Listing files in workspace ==="
                bat 'dir'
            }
        }
        
        stage('Check Docker') {
            steps {
                echo "=== Checking Docker ==="
                bat 'docker --version'
                bat 'docker ps'
            }
        }
        
        stage('Clean Old Container') {
            steps {
                echo "=== Cleaning old Docker container ==="
                script {
                    bat '''
                        docker stop demo 2>nul || exit 0
                        docker rm demo 2>nul || exit 0
                    '''
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "=== Building Docker Image: demo:latest ==="
                bat 'docker build -t demo:latest .'
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
            echo "✅ SUCCESS! Container running on http://localhost:5000"
        }
        failure {
            echo "❌ FAILED! Check console output for errors."
        }
    }
}
