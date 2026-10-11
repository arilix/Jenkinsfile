pipeline {
    agent any

    environment {
        DOCKER_EXE = 'C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials'
        DOCKERHUB_USERNAME = 'riset321'
        IMAGE_NAME = 'simple-app'
        APP_REPO = 'https://github.com/arilix/simple-app.git'
    }

    stages {
        stage('Checkout Application') {
            steps {
                git branch: 'main', url: "${APP_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"%DOCKER_EXE%" build -t %DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest .'
            }
        }

        stage('Login and Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat '''
                        @echo off
                        echo %DOCKER_PASS% | "%DOCKER_EXE%" login -u "%DOCKER_USER%" --password-stdin
                        if errorlevel 1 exit /b 1

                        "%DOCKER_EXE%" push "%DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest"
                    '''
                }
            }
        }
    }

    post {
        always {
            bat '"%DOCKER_EXE%" logout'
        }
    }
}
