pipeline {
  agent any
  stages {
    stage('Prepare') {
      steps {
        sh 'aws ecr get-login --no-include-email --region eu-west-1  | sh -'
      }
    }
    stage('Build') {
      steps {
        sh 'cd *.parent && mvn clean initialize package docker:build'
      }
    }
    stage('Push') {
      steps {
        sh 'cd *.parent && mvn docker:push'
      }
    }
    stage('Run') {
      steps {
        sh 'aws ecs run-task --cluster BWECS --task-definition getsfaccount:4'
      }
    }
  }
}