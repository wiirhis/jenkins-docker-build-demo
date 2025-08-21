pipeline {
  agent {
    docker {
      image 'docker:dind'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
    IMAGE_NAME = "rhisland/jenkins-docker-demo"
    IMAGE_TAG  = "latest"
  }

  stages {
    stage('Construire l’image Docker') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
        }
      }
    }

    stage('Publier sur Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          script {
            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
              docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
            }
          }
        }
      }
    }
  }

  post {
    success {
      echo "✅ Image poussée sur Docker Hub avec succès"
    }
    failure {
      echo "❌ Le pipeline a échoué"
    }
  }
}
