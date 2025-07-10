pipeline {
    agent any

    environment {
        IMAGE_NAME = "saipavan3105/aarambh"
        TAG = "latest"
    }

    stages {
        stage('Pull JAR from GitHub') {
            steps {
                sh 'wget https://raw.githubusercontent.com/saipavanreddy/aarambh/main/demo-0.0.1-SNAPSHOT.jar -O app.jar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME:$TAG
                    """
                }
            }
        }

        stage('Run Container on Host') {
            steps {
                sh 'docker run -d -p 5601:8080 --name aarambh-app $IMAGE_NAME:$TAG'
            }
        }
    }
}

