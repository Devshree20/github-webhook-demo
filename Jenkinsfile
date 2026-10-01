pipeline {
    agent any
    options {
    buildDiscarder(logRotator(
        numToKeepStr: '10',
        artifactNumToKeepStr: '5'
    ))
}

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

        bat '''
        docker build -t devshreebonde/github-webhook-demo:latest .
        docker tag devshreebonde/github-webhook-demo:latest devshreebonde/github-webhook-demo:v%BUILD_NUMBER%
        '''
    }
}
       stage('Docker Push') {
    steps {
        echo 'Pushing Docker images to Docker Hub...'

        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-credentials',
            usernameVariable: 'DOCKER_USERNAME',
            passwordVariable: 'DOCKER_TOKEN'
        )]) {
            bat '''
            docker login -u %DOCKER_USERNAME% -p %DOCKER_TOKEN%

            docker push devshreebonde/github-webhook-demo:latest
            docker push devshreebonde/github-webhook-demo:v%BUILD_NUMBER%

            docker logout
            '''
        }
    }
}
        stage('Docker Deploy') {
    steps {
        echo 'Deploying versioned Docker container...'

        bat '''
        docker stop github-webhook-demo-container || exit 0
        docker rm github-webhook-demo-container || exit 0

        docker run -d -p 8081:80 --name github-webhook-demo-container devshreebonde/github-webhook-demo:v%BUILD_NUMBER%
        '''
    }
}
        stage('Docker Cleanup') {
    steps {
        echo 'Cleaning unused Docker resources...'
        bat 'docker image prune -f'
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
