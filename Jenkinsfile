pipeline {
  agent { docker 'maven:3-jdk-8' }
  stages {
    stage ('build') {
      steps {
        sh 'mvn clean install'
      }
    }
  }
}
