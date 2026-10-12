node {
    stage('Check Docker Access') {
        bat '''
            echo ==== Windows Account ====
            whoami

            echo ==== Docker CLI ====
            "C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" --version

            echo ==== Docker Engine ====
            "C:\\Users\\HP\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" info
        '''
    }
}
