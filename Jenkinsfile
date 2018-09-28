pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages{
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
        stage ('Analyse static'){
           steps {
             echo 'Now Checkstyle analyse static'
             build job: 'checkstyle-analyse-static'

             echo 'Now PMD analyse static'
             build job: 'pmd-analyse-static'

             timeout(time: 3, unit: 'MINUTES') {
               retry(3) {
                  echo 'Now FindBug analyse static'
                  build job: 'findbug-analyse-static'
               }
             }
           }
        }
        stage ('Deploy to staging'){
           steps{
              echo 'Now Deploying to staging'
              build job: 'deploy-to-staging'
           }
        }
        stage ('Deploy to production'){
           steps{
              timeout(time:5, unit:'DAYS'){
                  input message: 'Approve PRODUCTION Deployment?'
              }

              echo 'Now Deploying to production'
              build job: 'deploy-to-prod'
           }
           post {
             success {
               echo 'App deployed to Production.'
             }

             failure {
               echo 'Deployment failed.'
             }
           }
        }
    }
}