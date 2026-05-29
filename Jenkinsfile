pipeline {

    agent any

    environment {
        IMAGE_NAME = 'raushan08/jenkins-demo'
        CONTAINER_NAME = 'myapp'
        DOCKER_HOST = 'tcp://host.docker.internal:2375'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'

                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying container...'

                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker run -d \
                  --name ${CONTAINER_NAME} \
                  -p 3000:3000 \
                  ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning dangling Docker images...'
                sh 'docker image prune -f'
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed! Please check console logs.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}

