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
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerhubpwd')]) {
          sh 'docker login -u bhargav247 -p ${dockerhubpwd}'
		      sh 'docker build --tag=bhargav247/greenimage'

        }
      }
    }
   
    
    }
}