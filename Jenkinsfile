pipeline {
  agent any
  stages {
	stage("Checkout") {
      steps {
        sh 'git clonehttps://github.com/gvsiva2008/K8s_gamukart.git '
	    sh 'K8s_gamukart'
      }
	}
	stage("build ") {
      steps {     
        sh 'mvn clean install'
      }
    }
    stage("Build Image") {
      steps {     
        sh 'docker build -t gvsiva2008/gumukart:1 .'
      }
    }
    stage("pushtoHub") {
      steps {     
        withCredentials([usernamePassword(credentialsID:'dockerhub', passwordVarible:'passwrod', usernameVarible: 'username')])
		  {
		   sh ' docker login -u ${env.username} -p ${env.password}'
           sh 'docker push gvsiva2008/gumukart'
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
