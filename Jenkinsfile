pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'shiva021'
        IMAGE_NAME = 'demo'
    }

    triggers {
        // Check for code changes every 2 minutes (optional)
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout Code from GitHub') {
            steps {
                echo "=== Cloning Repository from GitHub (branch: b1) ==="
                git branch: 'b1', url: 'https://github.com/navanish17/Lab_test.git'
            }
        }

        stage('Clean Old Containers (if any)') {
            steps {
                echo "=== Cleaning old Docker containers ==="
                bat '''
                    docker ps -a -q --filter "name=myapp" | findstr . && docker stop myapp && docker rm myapp || echo "No old container found."
                '''
            }
        }

        stage('Build Docker Image Locally') {
            steps {
                echo "=== Building Docker Image Locally ==="
                bat '''
                    docker build -t %DOCKER_HUB_USER%/%IMAGE_NAME%:latest .
                '''
            }
        }

        stage('Verify Image Build') {
            steps {
                echo "=== Listing Docker Images ==="
                bat 'docker images'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "=== Running Container on Docker Desktop ==="
                bat '''
                    docker run -d -p 5000:5000 --name myapp %DOCKER_HUB_USER%/%IMAGE_NAME%:latest
                    docker ps
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Docker image built and container running successfully on Docker Desktop!"
        }
        failure {
            echo "❌ Build failed. Check console output for errors."
        }
    }
}
