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
                        credentialsId: 'dockerhub-test',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    powershell '''
                        $ErrorActionPreference = "Stop"

                        Write-Host "Username credential:"
                        Write-Host $env:DOCKER_USER

                        $password = ConvertTo-SecureString $env:DOCKER_PASS -AsPlainText -Force
                        $credential = New-Object System.Management.Automation.PSCredential(
                            $env:DOCKER_USER,
                            $password
                        )

                        $plainPassword = $credential.GetNetworkCredential().Password

                        $plainPassword | & $env:DOCKER_EXE login `
                            --username $env:DOCKER_USER `
                            --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            throw "Docker Hub login gagal."
                        }

                        Write-Host "Docker Hub login berhasil."
                    '''
                }
            }
            post {
                always {
                    bat '"%DOCKER_EXE%" logout'
                }
            }
        }
    }
}
