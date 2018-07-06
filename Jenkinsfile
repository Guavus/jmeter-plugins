pipeline {
  agent { docker 'maven:3-jdk-8' }
  stages {
    stage ('build') {
      steps {
        sh 'mvn compiler:compile'
      }
    }
    stage ('test') {
      steps {
        sh 'mvn compiler:testCompile'
      }
    }
    stage ('package') {
      steps {
        sh 'mvn jar:jar'
      }
    }
    stage ('publish to artifactory')
    parallel {
      stage ('Montreal Repo') {
        steps {
          echo "push to Montreal"
        }
      }
      stage ('India repo') {
        steps {
          echo "push to India"
        }
      }
    }

  }
}
