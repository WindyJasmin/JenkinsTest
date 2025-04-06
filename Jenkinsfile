pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REGISTRY = "767397930329.dkr.ecr.us-east-1.amazonaws.com"
        ECR_REPO = "deployment/jenkins"
        IMAGE_NAME = "deployment/jenkins"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Authenticate to AWS ECR') {
            steps {
                script {
                    sh """
                        echo "Authenticating to AWS ECR"
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "Building Docker Image"
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "Tagging Docker Image"
                }
            }
        }

        stage('Push Image to AWS ECR') {
            steps {
                script {
                    sh "Pushing Image to AWS ECR"
                }
            }
        }

        stage('Clean Up Docker') {
            steps {
                script {
                    sh "Cleaning Up Docker"
                }
            }
        }
    }
}
