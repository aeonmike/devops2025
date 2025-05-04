pipeline {
  agent {label 'docker'}
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t mikejc30/devops2025:webapp .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh "docker push $IMAGE_NAME"
      }
    }
    stage('Deploy') {
      steps {
        script {
          sh """
            docker rm -f $CONTAINER_NAME || true
            docker pull $IMAGE_NAME
            docker run -d --name $CONTAINER_NAME -p 8087:8087 $IMAGE_NAME
          """
                }
              }
            }
        }
    }
