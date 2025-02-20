pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        REGISTRY_HOST = "767397930329.dkr.ecr.us-east-1.amazonaws.com"
        REPO_NAME = "deployment/jenkins"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Debug AWS Credentials') {
            steps {
                script {
                    sh '''
                    echo "AWS CLI Version:"
                    aws --version

                    echo "Checking AWS Credentials..."
                    env | grep AWS

                    echo "Checking if we can describe ECR Repositories..."
                    aws ecr describe-repositories --region $AWS_REGION || echo "Failed to access ECR"
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // 🔥 REMOVE cleanup, focus on building Docker image
                    echo "Skipping Docker credential cleanup."

                    // Check if Docker is installed and accessible
                    sh 'docker --version'

                    // Build the image; tag it without the registry host.
                    def customImage = docker.build("${REPO_NAME}:${IMAGE_TAG}")
                    
                    // Save the tag for later use
                    env.CUSTOM_IMAGE = "${REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                    // 🔥 Use ECR Plugin for authentication
                    docker.withRegistry("https://${REGISTRY_HOST}", "ecr:${AWS_REGION}:aws-jenkins-credentials") {
                        docker.image(env.CUSTOM_IMAGE).push()
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // 🔥 Make sure cleanup doesn’t block the pipeline
                    sh "docker rmi ${REPO_NAME}:${IMAGE_TAG} || true"
                }
            }
        }
    }
}
