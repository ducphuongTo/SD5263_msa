pipeline {
    agent {
        label 'devops-prac'
    }
    stages {
        stage('Build Front End Image') {
            steps {
                sh '''
                # Build the Docker image and tag it as practical-devops:latest
                docker build -t practical-devops:latest -f src/frontend/Dockerfile .

                # Tag the image with the ECR repository URI
                docker tag practical-devops:latest 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops:latest

                # Log in to ECR
                aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com

                # Push the image to ECR
                docker push 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops:latest
                '''
            }
        }

        stage('Build Back End Image') {
            steps {
                sh '''
                # Build the Docker image and tag it as practical-devops:latest
                docker build -t practical-devops-be:latest -f src/backend/Dockerfile .

                # Tag the image with the ECR repository URI
                docker tag practical-devops-be:latest 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops-be:latest

                # Log in to ECR
                aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com

                # Push the image to ECR
                docker push 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops-be:latest
                '''
            }
        }
    }
}
