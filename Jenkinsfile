pipeline {
    agent any

    environment {
        // וודא שהפרטים כאן תואמים ל-ECR שיצרת ב-AWS
        AWS_ACCOUNT_ID = '123456789012' 
        AWS_REGION     = 'us-east-1'
        IMAGE_NAME     = 'flask-app'
        ECR_URL        = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }

    stages {
        stage('Checkout') {
            steps {
                // משיכת הקוד מה-Git (כאן עדיין תצטרך Credentials של GitHub)
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/avivhananiya/CD-EX.git'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
                sh "docker tag ${IMAGE_NAME}:latest ${ECR_URL}/${IMAGE_NAME}:latest"
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // התחברות ל-ECR ללא מפתחות סטטיים - ה-Role עושה את העבודה!
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_URL}"
                    
                    // דחיפה ל-ECR
                    sh "docker push ${ECR_URL}/${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
