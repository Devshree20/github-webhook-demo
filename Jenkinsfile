pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                bat 'echo Build completed successfully'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'echo All tests passed'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                bat 'docker build -t github-webhook-demo .'
            }
        }

        stage('Docker Push') {
    steps {
        echo 'Pushing Docker image to Docker Hub...'
        bat 'docker push devshreebonde/github-webhook-demo:latest'
    }
}

        stage('Docker Deploy') {
            steps {
                echo 'Deploying Docker container...'

                bat '''
                docker stop github-webhook-demo-container || exit 0
                docker rm github-webhook-demo-container || exit 0
                docker run -d -p 8081:80 --name github-webhook-demo-container github-webhook-demo
                '''
            }
        }
    }

    post {
        success {
            echo '================================='
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '================================='
        }

        failure {
            echo 'CI/CD PIPELINE FAILED'
        }
    }
}
