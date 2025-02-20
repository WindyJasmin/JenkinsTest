pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1" 
        ECR_REPO = "767397930329.dkr.ecr.us-east-1.amazonaws.com/deployment/jenkins"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Login to AWS ECR') {
            steps {
                script {
                   withCredentials([aws(credentialsId: 'aws-jenkins-credentials', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $ECR_REPO:$IMAGE_TAG ."
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                    sh "docker push $ECR_REPO:$IMAGE_TAG"
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh "docker rmi $ECR_REPO:$IMAGE_TAG"
                }
            }
        }
    }
}
