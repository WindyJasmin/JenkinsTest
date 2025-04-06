pipeline {
    agent any

    stages {
        stage('Force Windows CMD') {
            steps {
                script {
                    def cmd = 'C:\\Windows\\System32\\cmd.exe /c echo Forced CMD Execution: It works!'
                    bat cmd
                }
            }
        }
    }
}
