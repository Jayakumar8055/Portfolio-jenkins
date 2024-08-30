pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-portfolio-image'
        DOCKER_TAG = 'alpine3.17'
        CONTAINER_NAME = 'my-portfolio-container'
        PORT = '8080'
    }
    
    stages {
            stage('Checkout') {
                steps {
                    // Check out the source code from your version control system
                    checkout scm
                }
            }
    

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with the specified tag
                    sh 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
                }
            }
        }

        stage('Stop Existing Container') {
            steps {
                script {
                    // Stop and remove the existing container if it exists
                    sh '''
                    if [ "$(docker ps -q -f name=${CONTAINER_NAME})" ]; then
                        docker stop ${CONTAINER_NAME}
                    fi
                    if [ "$(docker ps -a -q -f name=${CONTAINER_NAME})" ]; then
                        docker rm ${CONTAINER_NAME}
                    fi
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container with the specified image and tag
                    sh 'docker run -d -p ${PORT}:8080 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
