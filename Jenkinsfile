pipeline {
    agent any

    environment {
        IMAGE_NAME = 'riset321/simple-app'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
        DOCKER_EXE = 'C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'echo "Build di Windows"'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                    "%DOCKER_EXE%" build -t "%IMAGE_NAME%:%BUILD_NUMBER%" .
                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "${REGISTRY_CREDENTIALS}",
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    bat '''
                        @echo off
                        "%DOCKER_EXE%" login -u "%USER%" -p "%PASS%"
                        if errorlevel 1 exit /b 1

                        "%DOCKER_EXE%" push "%IMAGE_NAME%:%BUILD_NUMBER%"
                        if errorlevel 1 exit /b 1

                        "%DOCKER_EXE%" tag "%IMAGE_NAME%:%BUILD_NUMBER%" "%IMAGE_NAME%:latest"
                        if errorlevel 1 exit /b 1

                        "%DOCKER_EXE%" push "%IMAGE_NAME%:latest"
                        if errorlevel 1 exit /b 1
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline selesai.'
        }
    }
}
