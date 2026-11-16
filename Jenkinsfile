
pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checkout successful'
            }
        }

        stage('Check Environment') {
            steps {
                bat 'java -version'
                bat 'node --version'
                bat 'npm --version'
                bat 'python --version'
                bat 'docker --version'
            }
        }

        stage('Install React Dependencies') {
            steps {
                dir('frontend') {
                    bat 'npm ci'
                }
            }
        }

        stage('Build React Application') {
            steps {
                dir('frontend') {
                    bat 'npm run build'
                }
            }
        }

        stage('Verify Python Backend') {
            steps {
                bat 'python -m py_compile app.py'
            }
        }

        stage('Docker Build Image') {
            steps {
                bat 'docker build -t cloud-ci-cd-demo:latest -t cloud-ci-cd-demo:%BUILD_NUMBER% .'
            }
        }
    }

    post {
        success {
            echo 'CI pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check console output.'
        }
    }
}
