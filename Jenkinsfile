pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                nodejs('mattnode') {
                    sh 'npm install'
                }
              
            }
        }   
        stage('Test') {
            steps {
                nodejs('mattnode') {
                    sh 'npm test'
                }
              
            }
        }
        stage('Docker Image Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerRepo', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        echo ===========================
                        echo $AWS_SECRET_ACCESS_KEY
                        echo $AWS_ACCESS_KEY_ID
                        echo ===========================

                        wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce-cli_18.09.9~3-0~ubuntu-bionic_amd64.deb
                        dpkg -i ./docker-ce-cli_18.09.9~3-0~ubuntu-bionic_amd64.deb
                        apt-get update
                        apt-get install -y awscli
                        docker build -t workshopuser1:latest .
                        docker tag workshopuser1:latest workshopuser1:${BUILD_NUMBER}
                        docker tag workshopuser1:latest 686567993080.dkr.ecr.us-east-1.amazonaws.com/devopsbootcampuser1:latest
                        $(aws ecr get-login --region us-east-1 | sed 's/-e none//g')
                        docker push 686567993080.dkr.ecr.us-east-1.amazonaws.com/devopsbootcampuser1:latest
                    '''
                }
            }
        }
    }
}
