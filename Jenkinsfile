pipeline {
    agent any
    environment {
        IMAGE_NAME = "containerguru1/tp-l-project-1"  // Removed .git extension
        CONTAINER_NAME = "tp-l-project-1"  // Removed .git extension
        HOST_PORT = "8080"
        CONTAINER_PORT = "80"
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
                    def containerId = sh(
                        script: "docker ps -aq --filter name=${CONTAINER_NAME}",
                        returnStdout: true
                    ).trim()
                    if (containerId) {
                        echo "Stopping and removing existing container: ${containerId}"
                        sh """
                            docker stop ${containerId}
                            docker rm ${containerId}
                        """
                    } else {
                        echo "No existing container found with name ${CONTAINER_NAME}."
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
