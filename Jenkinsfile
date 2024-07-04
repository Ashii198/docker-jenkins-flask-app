pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'flask-app-image'
        DOCKER_CONTAINER = 'flask-app-container'
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
                    sh 'docker run -d --name ${DOCKER_CONTAINER} ${DOCKER_IMAGE}'
                }
            }
        }
        stage('Access Docker Container') {
            steps {
                script {
                    echo "Accessing Docker container..."
                    sh 'docker exec -it ${DOCKER_CONTAINER} /bin/sh'
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
