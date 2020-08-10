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
        dockerImage = docker.build('bhargav247/greenimage')
                    docker.withRegistry('', 'dockerhub') {
                        dockerImage.push()
        }
      }
    }
   
    
    }
}