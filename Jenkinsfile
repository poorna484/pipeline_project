pipeline {
    agent any

    environment {
        IMAGE_NAME = "tejasvi3697/node-app"
        CONTAINER_NAME = "node-container"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/poorna484/pipeline_project.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker run -d -p 3000:3000 --name $CONTAINER_NAME $IMAGE_NAME:latest'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub_credential',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

    }
}
