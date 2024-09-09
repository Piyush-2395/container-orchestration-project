pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'
        DOCKER_IMAGE_FRONTEND = "piyush9523/container-orchestration-frontend"
        DOCKER_IMAGE_BACKEND = "piyush9523/container-orchestration-backend"
    }
    
    stages {
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        sh 'docker build -t $DOCKER_IMAGE_FRONTEND:latest ./learnerReportCS_frontend'
                        sh 'docker build -t $DOCKER_IMAGE_BACKEND:latest ./learnerReportCS_backend'
                        sh 'docker push $DOCKER_IMAGE_FRONTEND:latest'
                        sh 'docker push $DOCKER_IMAGE_BACKEND:latest'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'helm upgrade --install mern-app ./helm-chart-directory'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
