pipeline {
  agent any  // This specifies the pipeline will run on any available Jenkins agent

  triggers {
    githubPush()  // Ensures the pipeline triggers automatically on GitHub push events
  }

  stages {
    stage('Checkout') {
      steps {
        script {
          // Clone the repository
          git branch: 'main', url: 'https://github.com/Madhu-123-bot/TEST-TASK-1.git'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("containerguru1/tp-l-project-1.git")
        }
      }
    }

    stage('Push Docker Image to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'doc-hub-cred') {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy to AWS EC2 (Same Server)') {
      steps {
        script {
          // Pull the Docker image from Docker Hub
          sh 'docker pull containerguru1/tp-l-project-1.git:latest'

          // Run the Docker container
          sh 'docker run -d -p 80:80 containerguru1/tp-l-project-1.git:latest'
        }
      }
    }
  }

  post {
    failure {
      echo "Pipeline failed!"
    }
    success {
      echo "Pipeline completed successfully!"
    }
  }
}
