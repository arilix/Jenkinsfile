pipeline {
    agent any

    environment {
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
                bat '"C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t riset321/simple-app:latest .'
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
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker push riset321/simple-app:latest'
                }
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}
