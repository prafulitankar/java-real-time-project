```grrovy
pipeline {
    agent any
    tools {
        maven  "MAVEN3"
        jdk  "OracleJDK11"
    }

    stages {
        stage ('Fetching code') {
            steps {
                git branch : 'main' , url: 'https://github.com/hkhcoder/vprofile-project.git'
            }
        }

        stage ('Source code build'){
            steps {
                sh 'mvn install -DskipTests'  
            }

            post {
            success {  
                echo 'Archieving the Artifact'
                archiveArtifacts artifacts: '**/*.war'
            }
          }
        }

        

        stage ('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```