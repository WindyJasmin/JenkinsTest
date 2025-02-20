pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REGISTRY = "767397930329.dkr.ecr.us-east-1.amazonaws.com"
        ECR_REPO = "deployment/jenkins"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Login to AWS ECR') {
            steps {
                withAWS(credentials: 'aws-jenkins-credentials', region: 'us-east-1') {
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 767397930329.dkr.ecr.us-east-1.amazonaws.com
                    """
                }
            }
        }


        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG ."
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                    sh "docker push $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh "docker rmi $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }
    }
}
