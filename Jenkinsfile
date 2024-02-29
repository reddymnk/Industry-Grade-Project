pipeline {
   agent { label 'neelapc2' }

	
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dcoker-hub-credentials')
    ANSIBLE_PRIVATE_KEY = credentials('ansible_key')
    //REMOTE_SERVER = '192.168.1.11'
    REMOTE_USER = 'neela1' 	  	  
  }
	
  // Fetch code from GitHub
	
  stages {
    stage('Clone the Repo') {
      steps {
        git 'https://github.com/reddymnk/Industry-Grade-Project'

      }
    }
	  
   // Build Java application
	  
    stage('Maven Build') {
      steps {
        sh 'sudo mvn clean install'
      }
	    
     // Post building archive Java application
	    
      post {
        success {
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
	  
  // Test Java application
	  
    stage('Maven Test') {
      steps {
        sh 'sudo mvn test'
      }
    }
	  
   // Build docker image in Jenkins
	  
    stage('Build Docker Image') {

      steps {
        sh 'sudo docker build -t myabc-app:latest .'
        //sh 'sudo docker tag abctechnologies reddymnk/myabc-app:latest'
      }
    }
	  
   // Login to DockerHub before pushing docker Image
	  
    stage('Login to DockerHub') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS | docker login -u    $DOCKERHUB_CREDENTIALS --password-stdin'
      }
    }
	  
   // Push image to DockerHub registry
	  
    stage('Push Image to dockerHUb') {
      steps {
        sh 'sudo docker push reddymnk/myabc-app:latest'
      }
      post {
        always {
          sh 'sudo docker logout'
        }
      }

    }
}
