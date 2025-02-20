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
                    // Clean up any stale docker credentials (optional)
                    sh 'rm -f ~/.dockercfg ~/.docker/config.json || true'
                    
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
                    // Use the ECR plugin syntax to automatically log in:
                    docker.withRegistry("https://${REGISTRY_HOST}", "ecr:${AWS_REGION}:aws-jenkins-credentials") {
                        docker.image(env.CUSTOM_IMAGE).push()
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Optionally remove the local image to free disk space.
                    sh "docker rmi ${REGISTRY_HOST}/${REPO_NAME}:${IMAGE_TAG} || true"
                }
            }
        }
    }
}
