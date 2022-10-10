pipeline {
    agent any

    stages {
        stage('Docker Build') {
            steps {
                sh '''
                
                # login to ecr 
                aws ecr get-login-password --region eu-west-3 | sudo docker login --username AWS --password-stdin 101275806917.dkr.ecr.eu-west-3.amazonaws.com
                # build the image
                sudo docker build -t 537593809027.dkr.ecr.eu-west-3.amazonaws.com/static-nginx:$BUILD_NUMBER .
                #push the image to ecr 
                sudo docker push 537593809027.dkr.ecr.eu-west-3.amazonaws.com/static-nginx:$BUILD_NUMBER
                '''
            }
        }
        stage('EKS Deploy') {
            steps {
                sh '''
                # apply kubectl 
                cd k8s/
                sed -i 's/TAG/$BUILD_NUMBER/g' deployment.yaml
                kubectl apply -f .
                '''
            }
        }
    }
}