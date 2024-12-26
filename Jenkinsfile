pipeline {
    agent any
    environment {
        IMAGE_NAME = "containerguru1/tp-l-project-1"  // Your image name
        CONTAINER_NAME = "tp-l-project-1"  // Container name
        HOST_PORT = "80"  // Port 80 (as you want to deploy to port 80)
        CONTAINER_PORT = "80"  // The internal container port
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Madhu-123-bot/TEST-TASK-1.git'
            }
        }
        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    // Get container ID for the one running on port 80
                    def containerId = sh(
                        script: "docker ps -aq --filter 'name=${CONTAINER_NAME}' --filter 'publish=${HOST_PORT}'",
                        returnStdout: true
                    ).trim()

                    if (containerId) {
                        echo "Stopping and removing existing container with ID: ${containerId}"
                        sh """
                            docker stop ${containerId}
                            docker rm ${containerId}
                        """
                    } else {
                        echo "No existing container found with the name ${CONTAINER_NAME} and port ${HOST_PORT}."
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Deploy Updated Container') {
            steps {
                script {
                    sh """
                        docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}
                    """
                }
            }
        }
    }
}
