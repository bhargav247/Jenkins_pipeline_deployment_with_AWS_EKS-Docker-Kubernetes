pipeline {
  agent any
  stages {
    stage('Lint') {
      steps {
        #Lint Dockerfile
        hadolint Dockerfile
        sh 'tidy -q -e *.html'
      
      }
    }

    stage('Build Docker Image') {
      steps {
        withCredentials([[credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
		      sh 'docker build --tag=bhargav247/greenimage'

        }
      }
    }
   
    stage('Push Docker Image') {
      steps {

      }
    }

    stage('set current kubectl context') {
    steps {

      }
  }

    stage('Deploy Kubernetes') {
      steps {
      
      }
    }
  }
}

   
