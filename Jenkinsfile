pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dockerhub-username/sample-fastapi"
        TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/poorna484/pipeline_project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credential', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker network create myapp-network || true

                docker rm -f fastapi-container || true

                docker run -d \
                --name fastapi-container \
                --network myapp-network \
                -p 8000:8000 \
                $DOCKER_IMAGE:$TAG
                '''
            }
        }

    }
}
