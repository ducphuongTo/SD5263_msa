pipeline {
    agent {
        label 'devops-prac'
    }
    stages {
        stage('Build Front End Image') {
            steps {
                sh '''
                docker build -t fe-image -f src/frontend/Dockerfile .

                docker tag fe-image:latest 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops/fe-image:latest

                aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com

                docker push 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops/fe-image:latest
                '''
            }
        }

        // stage('Build Backend Image') {
        //     steps {
        //         sh '''
        //         docker build -t be-image -f src/backend/Dockerfile .
        //         aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com
        //         docker push 529088254389.dkr.ecr.ap-northeast-1.amazonaws.com/practical-devops/be-image
        //         '''
        //     }
        // }
    }
}
