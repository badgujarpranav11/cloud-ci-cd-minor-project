
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

        stage('SonarQube Code Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'

                    withSonarQubeEnv('SonarQube') {
                        bat "\"${scannerHome}\\bin\\sonar-scanner.bat\""
                    }
                }
            }
        }

        stage('Docker Build Image') {
            steps {
                bat 'docker build -t cloud-ci-cd-demo:latest -t cloud-ci-cd-demo:%BUILD_NUMBER% .'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    bat '''
                    @echo off
                    echo %DOCKERHUB_TOKEN%| docker login --username %DOCKERHUB_USER% --password-stdin
                    docker tag cloud-ci-cd-demo:latest badgujarpranav11/cloud-ci-cd-demo:latest
                    docker tag cloud-ci-cd-demo:%BUILD_NUMBER% badgujarpranav11/cloud-ci-cd-demo:%BUILD_NUMBER%
                    docker push badgujarpranav11/cloud-ci-cd-demo:latest
                    docker push badgujarpranav11/cloud-ci-cd-demo:%BUILD_NUMBER%
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check console output.'
        }
    }
}
