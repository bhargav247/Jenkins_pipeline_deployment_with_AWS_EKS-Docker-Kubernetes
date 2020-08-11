pipeline {
  agent any
  stages {
    stage('Lint') {
      steps {
       // hadolint Dockerfile
        sh 'tidy -q -e *.html'
      
      }
    }

    stage('Build Docker Image') {
      steps {
        script{
        dockerImage = docker.build('bhargav247/greenimage')
        docker.withRegistry('', 'dockerhub') {
          dockerImage.push()
        }
        }
      }
    }
   stage('Deploy Kubernetes') {
      steps {
        withAWS(region:'us-west-2', credentials:'capstone') {
        sh 'kubectl apply -f ./blue-green-service.json'
        }
      }
    }
  }
}