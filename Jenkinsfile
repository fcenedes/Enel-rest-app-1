pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        parallel(
          "Package and Docker Build": {
            sh 'cd *.parent && mvn clean initialize package docker:build'
            
          },
          "AWS Token": {
            sh 'aws ecr get-login --no-include-email --region eu-west-1  | sh -'
            
          }
        )
      }
    }
    stage('Docker Push') {
      steps {
        sh 'cd *.parent && mvn initialize docker:push'
      }
    }
  }
}