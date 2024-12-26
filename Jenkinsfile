pipeline {
  agent any

  triggers {
    githubPush()
  }

  stages {
    stage('Checkout') {
      steps {
        script {
          git branch: 'main', url: 'https://github.com/Madhu-123-bot/TEST-TASK-1.git'
        }
      }
    }

    stage('Stop and Remove Existing Container') {
      steps {
        script {
          sh '''
            container_id=$(docker ps -aq --filter "name=tp-l-project")
            if [ -n "$container_id" ]; then
              docker stop $container_id || true
              docker rm $container_id || true
            else
              echo "No existing container found with name tp-l-project."
            fi
          '''
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("containerguru1/tp-l-project-1")
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

    stage('Deploy Updated Container') {
      steps {
        script {
          sh '''
            docker pull containerguru1/tp-l-project-1:latest
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
