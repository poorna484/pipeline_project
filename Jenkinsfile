pipeline {
    agent any

    environment {
        IMAGE_NAME = "tejasvi3697/nginx-app"
        TAG = "latest"
    }

    stages {

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f nginx-container || true
                docker run -d -p 8085:80 --name nginx-container $IMAGE_NAME:$TAG
                '''
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

        stage('Docker Push') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }

    }
}
