pipeline {
  agent any  // Specify that the pipeline runs on any available agent

  triggers {
    githubPush()  // Trigger pipeline on GitHub push events
  }

  stages {
    stage('Checkout') {
      steps {
        script {
          // Clone the repository from GitHub
          git branch: 'main', url: 'https://github.com/Madhu-123-bot/TEST-TASK-1.git'
        }
      }
    }

    stage('Stop and Remove Existing Container') {
      steps {
        script {
          // Stop and remove the container if it exists
          sh '''
            if docker ps -q --filter "name=tp-l-project"; then
              docker stop tp-l-project
              docker rm tp-l-project
            fi
          '''
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          // Build the new Docker image
          dockerImage = docker.build("containerguru1/tp-l-project-1")
        }
      }
    }

    stage('Push Docker Image to Docker Hub') {
      steps {
        script {
          // Push the Docker image to Docker Hub
          docker.withRegistry('https://registry.hub.docker.com', 'doc-hub-cred') {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy Updated Container') {
      steps {
        script {
          // Pull the updated Docker image
          sh 'docker pull containerguru1/tp-l-project-1:latest'
          
          // Run the updated Docker container
          sh '''
            docker run -d --name tp-l-project -p 80:80 containerguru1/tp-l-project-1:latest
          '''
        }
      }
    }
  }

  post {
    always {
      echo "Pipeline executed."
    }
    failure {
      echo "Pipeline failed!"
    }
    success {
      echo "Pipeline completed successfully!"
    }
  }
}
