```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "praveenpathange/frontend"
        DOCKER_CREDS = credentials('dockerhub')
    }

    stages {

        stage('Verify Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'ls -la frontend'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER ./frontend'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin'

                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'

                sh 'docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest'

                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {

                sh '''
                sed -i "s|IMAGE_PLACEHOLDER|$IMAGE_NAME:$BUILD_NUMBER|g" k8s/deployment.yaml
                '''

                sh 'kubectl apply -f k8s/deployment.yaml'

                sh 'kubectl apply -f k8s/service.yaml'

                sh 'kubectl get pods'

                sh 'kubectl get svc'
            }
        }
    }

    post {

        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
```
