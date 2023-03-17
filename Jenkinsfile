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
				
					
				script{
					try{
						sh 'ssh ubuntu@ip-10-0-7-73 kubectl apply -f statefulset.yaml --kubeconfig=/home/ubuntu/.kube/config'

						}catch(error)
						{

						}
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
