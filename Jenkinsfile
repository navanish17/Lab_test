pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'shiva021'
        DOCKER_HUB_PASS = 'M@chine2002'
        IMAGE_NAME = 'demo'
    }

    triggers {
        // Automatically check GitHub every 2 minutes for changes
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "=== Cloning Repository from GitHub ==="
                git branch: 'b1', url: 'https://github.com/navanish17/Lab_test.git'
            }
        }

        stage('Clean Previous Containers') {
            steps {
                echo "=== Cleaning old Docker containers (if any) ==="
                bat '''
                    docker ps -a -q --filter "name=myapp" | findstr . && docker stop myapp && docker rm myapp || echo "No old container found."
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "=== Building Docker Image ==="
                bat '''
                    docker build -t %DOCKER_HUB_USER%/%IMAGE_NAME%:latest .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "=== Running New Docker Container ==="
                bat '''
                    docker run -d -p 5000:5000 --name myapp %DOCKER_HUB_USER%/%IMAGE_NAME%:latest
                    docker ps
                '''
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                echo "=== Logging into Docker Hub and Pushing Image ==="
                bat '''
                    docker login -u %DOCKER_HUB_USER% -p %DOCKER_HUB_PASS%
                    docker push %DOCKER_HUB_USER%/%IMAGE_NAME%:latest
                    docker logout
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build and push completed successfully!"
        }
        failure {
            echo "❌ Build failed. Check console output for details."
        }
    }
}
