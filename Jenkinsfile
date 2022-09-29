pipeline {
  agent any
  stages {
	stage("Checkout") {
      steps {
        sh 'git clone https://github.com/gvsiva2008/K8s_gamukart.git '
	    sh 'cd K8s_gamukart'
      }
	}
	stage("build ") {
      steps {     
        sh 'mvn clean install'
      }
    }
    stage("Build Image") {
      steps {     
	sh 'whoami'      
        sh 'docker build -t gvsiva2008/gumukart .'
      }
    }
    stage("pushtoHub") { 
        steps{
           withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
             sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
             sh 'docker push gvsiva2008/gumukart:latest'
           }
        }
     }
     stage("Deploying App to Kubernetes") {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "k8s")
        }
      }
    }
  }
}	
