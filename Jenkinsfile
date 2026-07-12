pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO   = '550168773269.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr-demo'
        IMAGE_TAG  = "${BRANCH_NAME}-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."
            }
        }

        stage('Login to ECR') {
            steps {
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}"
            }
        }

        stage('Push to ECR') {
            steps {
                sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "Successfully pushed ${ECR_REPO}:${IMAGE_TAG}"
        }
        failure {
            echo "Pipeline failed for branch ${BRANCH_NAME}"
        }
    }
}
