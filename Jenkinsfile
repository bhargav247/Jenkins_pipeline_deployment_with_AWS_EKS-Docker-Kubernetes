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
         sh '''
              docker build -t bhargav247/capstone . 
          '''
        }
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
					sh '''
          docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
          docker push bhargav247/capstone
           '''
         }
      }
    }
    stage('Set kubectl context') {
			steps {
				withAWS(region:'us-west-2', credentials:'capstone') {
					sh '''
            aws eks --region us-east-2 update-kubeconfig --name capstone-cluster
						kubectl config use-context arn:aws:eks:us-west-2:625264722585:cluster/capstone-cluster
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