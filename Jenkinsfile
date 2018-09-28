pipeline {
    agent { docker { image 'maven:3.3.3' } }
    stages{
        stage ('Init'){
            steps {
               sh 'mvn --version'
            }
        }
        stage ('Build'){
            steps {
                bat 'mvn clean package'
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