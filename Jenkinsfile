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
                echo 'Building the project...'
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
            archiveArtifacts artifacts: 'build/build-info.txt', fingerprint: true

            echo '================================='
            echo 'CI PIPELINE SUCCESSFUL'
            echo '================================='
        }

        failure {
            echo 'CI PIPELINE FAILED'
        }
    }
}
