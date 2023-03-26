pipeline {

  environment {
    dockerimagename = "onyiokoligwe/project"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/onyi-git/project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy to K8s')
        {
        steps{ 
		withKubeConfig([credentialsId: 'minikube_config', serverUrl: 'https://192.168.49.2:8443']) {
                sh 'kubectl apply -f pv.yaml'
                sh 'kubectl apply -f pvc.yaml'
                sh 'kubectl apply -f svc.yaml'
                sh 'kubectl apply -f statefulset.yaml'
            }
		}
    }
      
 }

	post {
		always {
			sh 'docker logout'
		}
	}

}
