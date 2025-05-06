pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
  }
  stages {
    stage('Clone') {
      steps {
        git branch: 'main', url: 'https://github.com/t4mirci/t4mirci.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/flask-api:latest .'
      }
    }
    stage('Push to DockerHub') {
      steps {
        sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
        sh 'docker push $DOCKERHUB_CREDENTIALS_USR/flask-api:latest'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'helm upgrade --install flask-api ./helm/flask-api --set image.repository=$DOCKERHUB_CREDENTIALS_USR/flask-api,image.tag=latest'
      }
    }
  }
}
