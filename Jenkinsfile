pipeline {
  agent any

  environment {
    PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    DOCKER_HUB_USER  = 'gonchaung'
    IMAGE_NAME       = 'mern-todo-exam'
    DOCKER_HUB_CREDS = 'docker-hub-credentials'
  }

  stages {
    stage('Build Frontend') {
      steps {
        dir('TODO/todo_frontend') {
          sh 'node -v'
          sh 'npm -v'
          sh 'npm install'
          sh 'npm run build'
        }
      }
    }

    stage('Build Backend') {
      steps {
        dir('TODO/todo_backend') {
          sh 'npm install'
        }
      }
    }

    stage('Prepare Static Files') {
      steps {
        sh 'mkdir -p TODO/todo_backend/static'
        sh 'rm -rf TODO/todo_backend/static/build || true'
        sh 'cp -r TODO/todo_frontend/build TODO/todo_backend/static/'
      }
    }

    stage('Containerize') {
      steps {
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
