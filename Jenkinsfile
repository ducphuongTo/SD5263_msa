pipeline {
    agent {
        label 'devops-prac'
    }
    environment {
        AWS_REGION = 'ap-northeast-1'
        ECR_REGISTRY = "529088254389.dkr.ecr.${AWS_REGION}.amazonaws.com"
        ECR_REPOSITORY_FE = "${ECR_REGISTRY}/practical-devops-latest"
        ECR_REPOSITORY_BE = "${ECR_REGISTRY}/practical-devops:be-latest"
        KUBECONFIG = "${WORKSPACE}/.kube/config"
    }

    stages {
        stage('Build Front End Image') {
            steps {
                sh '''
                docker build -t practical-devops:latest -f src/frontend/Dockerfile .

                docker tag practical-devops:latest 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops:latest

                aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com

                docker push 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops:latest
                '''
            }
        }

        stage('Build Back End Image') {
            steps {
                sh '''
                # Build the Docker image and tag it as practical-devops:be-latest
                docker build -t practical-devops-be:latest -f src/backend/Dockerfile .

                # Tag the image with the ECR repository URI
                docker tag practical-devops-be:latest 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops:be-latest

                # Log in to ECR
                aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com

                # Push the image to ECR
                docker push 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops:be-latest
                '''
            }
        }

        stage('Deploy k8s') {
            agent any
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    withAWS(region: "${AWS_REGION}", credentials: "aws-creds") {
                        withEnv(["KUBECONFIG=/home/ec2-user/jenkins-agent/workspace/devop-practive@2/.kube/config"]) {
                            sh "aws eks update-kubeconfig --region ap-northeast-1 --name eks-phuong"
                        }
                        sh 'kubectl apply -f k8s/aws/mongodb.yaml'
                        sh 'kubectl apply -f k8s/aws/backend.yaml'
                        sh 'kubectl apply -f k8s/aws/frontend.yaml'
                    }
                }
            }
        }
    }
}
