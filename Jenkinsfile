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
                        aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag $IMAGE_NAME:$IMAGE_TAG $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }

        stage('Push Image to AWS ECR') {
            steps {
                script {
                    sh "docker push $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }

        stage('Clean Up Docker') {
            steps {
                script {
                    sh "docker rmi $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }
    }
}
