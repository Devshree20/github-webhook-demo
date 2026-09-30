pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
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

        stage('Package') {
            steps {
                echo 'Creating build package...'
                bat 'if not exist build mkdir build'
                bat 'echo Jenkins CI/CD Demo > build\\build-info.txt'
            }
        }
    }

    post {
        success {
            echo '================================='
            echo 'CI PIPELINE SUCCESSFUL'
            echo '================================='
        }

        failure {
            echo 'CI PIPELINE FAILED'
        }
    }
}
post {
    success {
        archiveArtifacts artifacts: 'build/build-info.txt', fingerprint: true

        echo '================================='
        echo 'CI PIPELINE SUCCESSFUL'
        echo '================================='
    }

    failure {
        echo 'CI PIPELINE FAILED'
    }
}
