pipeline {
    agent any

    stages {
        stage('Print a Message') {
            steps {
                script {
                    echo "✅ Jenkins is working on Windows!"
                }
            }
        }

        stage('Do Math') {
            steps {
                script {
                    def result = 21 * 2
                    echo "21 x 2 = ${result}"
                }
            }
        }

        stage('List Env Vars') {
            steps {
                script {
                    echo "USER: ${env.USERNAME}"
                    echo "OS: ${env.OS}"
                }
            }
        }
    }
}
