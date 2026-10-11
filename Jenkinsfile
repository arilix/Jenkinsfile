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
                echo 'Mengambil source code aplikasi dari GitHub...'

                git branch: 'main',
                    url: "${APP_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Membangun Docker image...'

                bat '''
                    @echo off

                    "%DOCKER_EXE%" build -t "%DOCKERHUB_USERNAME%/%IMAGE_NAME%:latest" .

                    if errorlevel 1 exit /b 1
                '''
            }
        }

        stage('Login and Push to Docker Hub') {
            steps {
                echo 'Login dan mengunggah Docker image ke Docker Hub...'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat '''
                        @echo off

                        powershell.exe -NoProfile -NonInteractive -ExecutionPolicy Bypass -Command "$env:DOCKER_PASS | & $env:DOCKER_EXE login --username $env:DOCKER_USER --password-stdin; if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }; $image = $env:DOCKERHUB_USERNAME + '/' + $env:IMAGE_NAME + ':latest'; & $env:DOCKER_EXE push $image; exit $LASTEXITCODE"

                        if errorlevel 1 exit /b 1
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline berhasil! Docker image sudah diunggah ke Docker Hub.'
            echo 'Repository: https://hub.docker.com/r/riset321/simple-app'
        }

        failure {
            echo 'Pipeline gagal. Periksa Console Output untuk melihat error.'
        }

        always {
            bat '''
                @echo off
                "%DOCKER_EXE%" logout
            '''
        }
    }
}
