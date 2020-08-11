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
       withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
         //dockerImage = docker.build('bhargav247/capstone')
         sh '''
              docker build -t bhargav247/capstone . 
          '''
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        //docker.withRegistry('', 'dockerhub') 
        //dockerImage.push('bhargav247/capstone')
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
          docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
          docker push bhargav247/capstone
           '''
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