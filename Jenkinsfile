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
                    echo "✅ Authenticating to AWS ECR"
                    echo "Region: ${env.AWS_REGION}"
                    echo "Registry: ${env.ECR_REGISTRY}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🚧 Building Docker Image: ${env.IMAGE_NAME}:${env.IMAGE_TAG}"
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    echo "🏷️ Tagging Image for ${env.ECR_REPO}"
                }
            }
        }

        stage('Push Image to AWS ECR') {
            steps {
                script {
                    echo "📤 Pushing Image to ${env.ECR_REGISTRY}/${env.ECR_REPO}"
                }
            }
        }

        stage('Clean Up Docker') {
            steps {
                script {
                    echo "🧹 Cleaning up Docker images"
                }
            }
        }
    }
}
