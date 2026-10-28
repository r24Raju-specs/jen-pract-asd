pipeline {
    agent any

    environment {
        IMAGE_NAME     = 'my-web-app'
        CONTAINER_NAME = 'my-web-app'
        HOST_PORT      = '8081'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} \
                        -p ${HOST_PORT}:80 \
                        --restart unless-stopped \
                        ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success { echo 'Deployed at http://localhost:8081' }
        failure { echo 'Pipeline failed. Check the console output.' }
    }
}
