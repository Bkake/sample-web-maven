pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages{
        stage ('Init'){
            steps {
               sh 'mvn --version'
            }
        }
        stage ('Build'){
            steps {
                echo 'Now Building...'
                sh 'mvn clean package'
            }
            post {
              success {
                 echo 'Now Archiving...'
                 archiveArtifacts artifacts: '**/target/*.war'
              }
            }
        }
    }
}