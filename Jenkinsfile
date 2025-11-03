pipeline {
    agent any

    triggers {
        // Poll SCM every 2 minutes for changes (works for Windows agents too)
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                echo "=== Checking out source code from GitHub ==="
                git branch: 'b1', url: 'https://github.com/navanish17/Lab_test.git'
            }
        }

        stage('Build') {
            steps {
                echo "=== BUILD STAGE (Windows) ==="
                // Run commands in Windows shell
                bat '''
                    echo Building project...
                    dir
                    echo Build successful!
                '''
            }
        }

        stage('Test') {
            steps {
                echo "=== TEST STAGE (Windows) ==="
                bat '''
                    echo Running unit tests...
                    echo All tests passed successfully!
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "=== DEPLOY STAGE (Windows) ==="
                bat '''
                    echo Deploying artifacts...
                    echo Deployment complete!
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Please check console logs."
        }
    }
}
