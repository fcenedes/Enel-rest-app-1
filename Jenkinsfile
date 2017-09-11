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
        sh 'cd *.parent && mvn initialize docker:push'
      }
    }
    stage('Run') {
      steps {
        sh 'aws ecs run-task --cluster BWECS --task-definition getsfaccount:5 > run.json'
      }
    }
    stage('Check') {
      steps {
        sh 'aws ecs wait tasks-running --cluster BWECS --tasks $(cat run.json | jq \'.tasks[0].taskArn\' | sed -e \'s/^"//\' -e \'s/"$//\')'
      }
    }
  }
}