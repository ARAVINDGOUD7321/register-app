pipeline {
    agent { label 'Jenkins-Agent' }
     tools {
        jdk 'java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
           }
    

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/ARAVINDGOUD7321/register-app'
                }
        }

         stage("Build Application"){
             steps {
                sh "mvn clean package"
            }

       }

         stage("Test Application"){
             steps {
                 sh "mvn test"
           }
       }
        stage("sonarQube Analysis"){
             steps {
                 script {
                     withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                     sh "mvn sonar:sonar"
                     }    
                 }
             }    
        }
          stage("Quality Gate"){
             steps {
                 script {
                     waitForQualityGate abortpipeline: false, credentialsId: 'jenkins-sonarqube-token'
                 }  
             }  
         }
    }
}    

