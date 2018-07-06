pipeline {
  agent { docker 'maven:3-jdk-8' }
  stages {
    stage ('build') {
      steps {
        sh 'mvn compiler:compile'
      }
    }
    stage ('test') {
      parallel {
        stage ('Test compile') {
          steps {
            sh 'mvn compiler:testCompile'
          }
        }
        stage ('Test Surefire') {
          steps {
            sh 'mvn surfire:test'
          }
        }
      }
    }
    stage ('package') {
      steps {
        sh 'mvn jar:jar'
      }
    }
  }
}
