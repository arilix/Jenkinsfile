node {
    stage('Check Docker Access') {
        bat '''
            echo ==== Windows Account ====
            whoami

            echo ==== Docker CLI ====
            where.exe docker
            docker --version

            echo ==== Docker Engine ====
            docker info
        '''
    }
}
