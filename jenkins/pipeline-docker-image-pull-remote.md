```groovy
pipeline {
  agent any
  environment {
        // Specify Docker host
        DOCKER_HOST = '172.31.6.200'
        //Specify Docker Registry Name 
        registry = "devops-weekend"
        //Add Credentials in Jenkins to Connect Dockerhub
	registryCredential = 'dockerhub'
        //Name Of Docker Image must be available in Dockerhub
        DOCKER_IMAGE = 'stepstech/devops-weekend:latest'
    }

  stages {
        stage('Build') {
          steps {
            echo 'Building..'
            sshagent(['jenkins-docker-intergation']) { 
                sh "ssh -o StrictHostKeyChecking=no -l ubuntu $DOCKER_HOST  'docker pull $DOCKER_IMAGE'"
                // jenkins-docker-intergation : add credential to connect docker host using Username with Private SSH key
            }
         }
      }
   }
}
```