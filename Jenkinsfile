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
        withCredentials([string(credentialsId: 'bhargav247', variable: 'dockerhub')]) {
          sh 'docker login -u bhargav247 -p ${dockerhub}'
		      sh 'docker build --tag=bhargav247/greenimage'

        }
      }
    }
   
    
    }
}