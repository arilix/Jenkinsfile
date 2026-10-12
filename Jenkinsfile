pipeline {
    agent any

    environment {
        DOCKER_EXE = 'C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
    }

    stages {
        stage('Test Docker Hub Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    powershell '''
                        $ErrorActionPreference = "Stop"

                        Write-Host "Menguji login Docker Hub..."
                        Write-Host "Username credential: $env:DOCKER_USER"

                        $env:DOCKER_PASS |
                            & $env:DOCKER_EXE login `
                                --username $env:DOCKER_USER `
                                --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            throw "Login Docker Hub gagal."
                        }

                        Write-Host "Login Docker Hub berhasil."
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pengujian login selesai.'
            bat '''
                "%DOCKER_EXE%" logout
            '''
        }
    }
}
