```groovy
pipeline {
    agent any
    tools {
        maven  "MAVEN3"
        jdk  "OracleJDK11"
    }

    environment {
	registry = "devops-weekend"
	registryCredential = 'dockerhub'
        DOCKER_IMAGE = 'stepstech/devops-weekend:latest'
        CONTAINER_NAME = 'webapp2'
    }

    stages {
        stage ('Fetching code') {
            steps {
                git branch : 'main' , url: 'https://github.com/devopshydclub/vprofile-project.git'
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

        stage ('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }

        }

        stage ('Sonar Scanner') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh '''${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=vprofile \
                        -Dsonar.projectName=vprofile \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=src/ \
                        -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest  \
                        -Dsonar.junit.reportPaths=target/surefire-reports/ \
                        -Dsonar.jacoco.reportPaths=target/jacoco.exec \
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }    
        }

        stage ("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                  waitForQualityGate abortPipeline: true
              }
            }
          }

        stage ("upload artifact") {
            steps{
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '3.140.66.172:8081',
                groupId: 'Test',
                version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                repository: 'vprofile-repo',
                credentialsId: 'nexuslogin',
                artifacts: [
                    [artifactId: 'vproapp',
                    classifier: '',
                    file: 'target/vprofile-v2.war',
                    type: 'war']
                ]
             )
            }
        }

        stage('Pull Docker Image') {
            steps {
                script {
                    // Pull Docker image
                    docker.image("${DOCKER_IMAGE}").pull()
                }
            }
        }
    }
```
}