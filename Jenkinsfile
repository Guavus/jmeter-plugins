pipeline {
  agent {
    docker 'maven:3-jdk-8'
  }
  environment {
    def server = Artifactory.server "SERVER_ID"
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
  }
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
    stage ('publish to artifactory') {
      parallel {
        stage ('Montreal Repo') {
            steps {
              echo "push to Monreal"
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
}
