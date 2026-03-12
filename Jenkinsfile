pipeline {
  agent any

  environment {
    PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    DOCKER_HUB_USER  = 'gonchaung'
    IMAGE_NAME       = 'mern-todo-exam'
    DOCKER_HUB_CREDS = 'docker-hub-credentials'
  }

  stages {
    stage('Containerize') {
      steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'docker --version'
        sh "docker build -t ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest ."
      }
    }

    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
          sh "docker push ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest"
        }
      }
    }
  }
}