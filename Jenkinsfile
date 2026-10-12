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

                        $psi = New-Object System.Diagnostics.ProcessStartInfo
                        $psi.FileName = $env:DOCKER_EXE
                        $psi.Arguments = "login --username $env:DOCKER_USER --password-stdin"
                        $psi.RedirectStandardInput = $true
                        $psi.UseShellExecute = $false
                        $psi.RedirectStandardOutput = $true
                        $psi.RedirectStandardError = $true

                        $process = [System.Diagnostics.Process]::Start($psi)
                        $process.StandardInput.Write($env:DOCKER_PASS)
                        $process.StandardInput.Close()
                        $process.WaitForExit()

                        Write-Host $process.StandardOutput.ReadToEnd()
                        Write-Host $process.StandardError.ReadToEnd()

                        if ($process.ExitCode -ne 0) {
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
