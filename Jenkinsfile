pipeline {
  agent any
  stages {
    stage('Lint') {
      steps {
       // hadolint Dockerfile
        sh 'tidy -q -e ./blue/*.html'
        sh 'tidy -q -e ./green/*.html'
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
   
    stage('Setting Kubectl context') {
			steps {
				withAWS(region:'us-west-2', credentials:'capstone') {
					sh '''
            aws eks --region us-west-2 update-kubeconfig --name capstone-cluster
						kubectl config use-context arn:aws:eks:us-west-2:625264722585:cluster/capstone-cluster
					'''
				}
			}
		}
   
   stage('Deploy blue container') {
      steps {
        withAWS(region:'us-west-2', credentials:'capstone') {
        sh 'kubectl apply -f ./blue/blue-controller.json'
        }
      }
    }
    
    stage('Deploy green container') {
      steps {
        withAWS(region:'us-west-2', credentials:'capstone') {
        sh 'kubectl apply -f ./green/green-controller.json'
        }
      }
    }

    stage('Deploy green service') {
      steps {
        withAWS(region:'us-west-2', credentials:'capstone') {
        sh 'kubectl apply -f ./blue-green-service.json'
        }
      }
    }
    stage('Switch deployment?') {
      steps {
        input "Switch deployment?"
    } 
    }
    stage('Deploy blue service') {
      steps {
        withAWS(region:'us-west-2', credentials:'capstone') {
        sh 'kubectl apply -f ./blue-service.json'
          }
        }
      }
  }
}