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
			withKubeConfig([credentialsId: 'K8s-username', serverUrl: 'http://3.10.199.130:8080']) {
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
