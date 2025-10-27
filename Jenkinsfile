pipeline {
  agent any

  environment {
    DOCKER_REGISTRY = "docker.io"
    DOCKER_REPO     = "vaishnavi873/flask-app"  // your Docker Hub repo
    IMAGE_TAG       = "${env.BUILD_NUMBER}"
    FULL_TAG        = "${DOCKER_REPO}:${IMAGE_TAG}"
    LATEST_TAG      = "${DOCKER_REPO}:latest"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker image') {
      steps {
        script {
          sh "docker build -t ${FULL_TAG} ."
          sh "docker tag ${FULL_TAG} ${LATEST_TAG}"
        }
      }
    }

    stage('Login & Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
          script {
            sh 'echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin'
            sh "docker push ${FULL_TAG}"
            sh "docker push ${LATEST_TAG}"
            sh 'docker logout'
          }
        }
      }
    }

    stage('Cleanup local images') {
      steps {
        script {
          sh "docker rmi ${FULL_TAG} || true"
          sh "docker rmi ${LATEST_TAG} || true"
        }
      }
    }
  }

  post {
    success {
      echo "✅ Docker image pushed: ${FULL_TAG} and ${LATEST_TAG}"
    }
    failure {
      echo "❌ Pipeline failed. Check logs."
    }
  }
}
