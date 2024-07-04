pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'flask-app-image'
        DOCKER_CONTAINER = 'flask-app-container'
        HOST_PORT = '5000'
        CONTAINER_PORT = '5000'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }
        stage('Create Docker Container') {
            steps {
                script {
                    echo "Creating Docker container..."
                    sh 'docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${DOCKER_CONTAINER} ${DOCKER_IMAGE}'
                }
            }
        }
        stage('Access Docker Container') {
            steps {
                script {
                    echo "Accessing Docker container..."
                    ssh 'docker exec ${DOCKER_CONTAINER} /bin/sh -c "echo Container accessed"'
                }
            }
        }
    }

    post {
        always {
            script {
                echo "Cleaning up Docker container..."
                sh 'docker stop ${DOCKER_CONTAINER} || true'
                sh 'docker rm ${DOCKER_CONTAINER} || true'
                echo "Cleaning up Docker image..."
                sh 'docker rmi ${DOCKER_IMAGE} || true'
            }
        }
    }
}
