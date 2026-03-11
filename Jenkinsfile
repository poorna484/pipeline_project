pipeline {
    agent any

    environment {
        IMAGE_NAME = "tejasvi3697/node-app"
        CONTAINER_NAME = "node-container"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/poorna484/pipeline_project.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p 3000:3000 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }
}
