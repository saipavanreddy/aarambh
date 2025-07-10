pipeline {
    agent any

    environment {
        IMAGE_NAME = '2tier-backend'
        DOCKERHUB_REPO = 'saipavan3105/2tier-backend'
        AWS_ECR_REPO = 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 408041843102.dkr.ecr.ap-south-1.amazonaws.com'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/saipavanreddy/aarambh.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker tag $IMAGE_NAME $DOCKERHUB_REPO:latest
                        docker push $DOCKERHUB_REPO:latest
                    '''
                }
            }
        }

        stage('Push to AWS ECR') {
            steps {
                sh '''
                    aws ecr get-login-password | docker login --username AWS --password-stdin $AWS_ECR_REPO
                    docker tag $IMAGE_NAME $AWS_ECR_REPO:latest
                    docker push $AWS_ECR_REPO:latest
                '''
            }
        }
    }
}
