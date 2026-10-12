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
                        Write-Host "Username: $env:DOCKER_USER"

                        $psi = New-Object System.Diagnostics.ProcessStartInfo
                        $psi.FileName = $env:DOCKER_EXE
                        $psi.Arguments = "login --username $env:DOCKER_USER --password-stdin"
                        $psi.UseShellExecute = $false
                        $psi.RedirectStandardInput = $true
                        $psi.RedirectStandardOutput = $true
                        $psi.RedirectStandardError = $true
                        $psi.CreateNoWindow = $true

                        $process = New-Object System.Diagnostics.Process
                        $process.StartInfo = $psi

                        [void]$process.Start()

                        $stdoutTask = $process.StandardOutput.ReadToEndAsync()
                        $stderrTask = $process.StandardError.ReadToEndAsync()

                        $process.StandardInput.Write($env:DOCKER_PASS)
                        $process.StandardInput.Close()

                        $process.WaitForExit()

                        $stdout = $stdoutTask.Result
                        $stderr = $stderrTask.Result

                        if ($stdout) {
                            Write-Host $stdout
                        }

                        if ($stderr) {
                            Write-Host $stderr
                        }

                        if ($process.ExitCode -ne 0) {
                            throw "Login Docker Hub gagal dengan exit code $($process.ExitCode)."
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
