pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        parallel(
          "Compile": {
            sh 'cd *.parent ;; mvn clean initialize package'
            
          },
          "AWS Token": {
            sh 'aws ecr get-login --no-include-email --region eu-west-1  | sh -'
            
          }
        )
      }
    }
    stage('Docker build') {
      steps {
        sh 'cd *.parent ;; mvn initialize docker:build'
      }
    }
    stage('Docker Push') {
      steps {
        sh 'cd *.parent ;; mvn initialize docker:push'
      }
    }
  }
}