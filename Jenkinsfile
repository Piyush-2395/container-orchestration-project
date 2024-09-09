pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials-id'
        DOCKER_IMAGE_FRONTEND = "piyush9523/container-orchestration frontend"
        DOCKER_IMAGE_BACKEND = "piyush9523/container-orchestration backend"
    }
    stages {
        stage('Build Frontend') {
            steps {
                script {
                    sh 'cd learnerReportCS_frontend && npm install && npm run build'
                }
            }
        }
        stage('Build Backend') {
            steps {
                script {
                    sh 'cd learnerReportCS_backend && npm install'
                }
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        sh 'docker build -t $DOCKER_IMAGE_FRONTEND:latest ./learnerReportCS_frontend'
                        sh 'docker build -t $DOCKER_IMAGE_BACKEND:latest ./learnerReportCS_backend '
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
