pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "saipavan3105/aarambh"
    }

    stages {
        stage('Download JAR from GitHub') {
            steps {
		sh '''
 		 git clone https://github.com/saipavanreddy/aarambh.git
  		 cp aarambh/demo-0.0.1-SNAPSHOT.jar app.jar
		'''

            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        stage('Run Image Locally') {
            steps {
                sh '''
                    docker rm -f aarambh-container || true
                    docker run -d -p 8080:8080 --name aarambh-container $DOCKER_IMAGE:latest
                '''
            }
        }
    }
}

