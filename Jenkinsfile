pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/frontend"
        DOCKER_CREDS = credentials('dockerhub')
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER ./frontend'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl get nodes'
            }
        }
    }
}
