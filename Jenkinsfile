pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tejasvi3697/fastapi-app"
        TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/poorna484/pipeline_project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub_credential',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
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
                docker rm -f fastapi-container || true

                docker run -d \
                --name fastapi-container \
                -p 8000:8000 \
                $DOCKER_IMAGE:$TAG
                '''
            }
        }

    }
}
