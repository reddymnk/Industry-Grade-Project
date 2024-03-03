pipeline {
   agent { label 'neelapc1' }


  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockercreds')
    //REMOTE_SERVER = '3.89.140.22'
    REMOTE_USER = 'neela1'
  }

  // Fetch code from GitHub

  stages {
    stage('Fetch the Github repo') {
      steps {
        git 'https://github.com/reddymnk/Industry-Grade-Project'

      }
    }

   // Build Java application

    stage('Maven Build') {
      steps {
        sh 'mvn clean install'
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
        sh 'mvn test'
      }
    }

   // Build docker image in Jenkins

    stage('Build Docker Image') {

    steps {
        sh 'docker build -t mydevopsabc:latest .'
        sh 'docker tag mydevopsabc  reddymnk/mydevopsabc:latest'
    }
}

   // Login to DockerHub before pushing docker Image

     stage('Login to DockerHub') {
      steps {
              script {
                 sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u    $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
      }
   }

   // Push image to DockerHub registry

    stage('Push Image to dockerHUb') {
      steps {
        sh 'docker push reddymnk/mydevopsabc:latest'
      }
      post {
       always {
        sh 'docker logout'
       }
    }

 }
    // Pull docker image from DockerHub and run in  agent instance neelapc1

     stage('Deploy to K8 cluster') {
         steps {
                sh  'kubectl --kubeconfig=/etc/kubernetes/admin.conf apply -f /home/neela1/Industry-Grade-Project/deployment2.yml'
           
         }
     }
  }
}
