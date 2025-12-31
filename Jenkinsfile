pipeline {
    agent any

    environment {
        REGION = "us-east-1"
        ECR_REPO = "992382545251.dkr.ecr.us-east-1.amazonaws.com/cd-ex-aviv"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t hello-world-app ."
            }
        }

        stage('ECR Login') {
            steps {
                sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_REPO}"
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker tag hello-world-app:latest ${ECR_REPO}:${IMAGE_TAG}"
                sh "docker tag hello-world-app:latest ${ECR_REPO}:latest"
                sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
                sh "docker push ${ECR_REPO}:latest"
            }
        }
    }
}
