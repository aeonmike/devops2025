pipeline {
  agent { label 'docker-server' }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    DOCKER_REPO = 'mikejc30/devops2025'
    CONTAINER_NAME = 'webapp-container'
  }

  stages {
    stage('Prepare') {
      steps {
        script {
          // Use build number or date to generate tag
          env.IMAGE_TAG = "build-${env.BUILD_NUMBER}"
          env.IMAGE_NAME = "${env.DOCKER_REPO}:${env.IMAGE_TAG}"
        }
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('Push') {
      steps {
        sh 'docker push $IMAGE_NAME'
      }
    }

    stage('Deploy') {
      steps {
        script {
          sh """
            docker rm -f $CONTAINER_NAME || true
            docker pull $IMAGE_NAME
            docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE_NAME
          """
        }
      }
    }
  }
}
