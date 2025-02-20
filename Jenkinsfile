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
                    // Clean up any existing docker config (optional but sometimes helps with auth issues)
                    sh 'rm -f ~/.dockercfg ~/.docker/config.json || true'
                    // Build the image with a tag that only includes the repository name,
                    // docker.withRegistry will prepend the registry host.
                    def customImage = docker.build("${REPO_NAME}:${IMAGE_TAG}")
                    // Store the built image in an environment variable for later stages
                    env.CUSTOM_IMAGE = "${REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                    // The docker.withRegistry block will use the ECR plugin.
                    // Note: The first parameter is the registry URL (without the repository part),
                    // and the second is "ecr:region:credentials-id".
                    docker.withRegistry("https://${REGISTRY_HOST}", "ecr:${AWS_REGION}:aws-jenkins-credentials") {
                        // Push the image built earlier.
                        docker.image(env.CUSTOM_IMAGE).push()
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Remove the local image to free up disk space.
                    sh "docker rmi ${REGISTRY_HOST}/${REPO_NAME}:${IMAGE_TAG} || true"
                }
            }
        }
    }
}
