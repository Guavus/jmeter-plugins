pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'mvn compiler:compile'
      }
    }
    stage('test') {
      steps {
        sh 'mvn compiler:testCompile'
      }
    }
    stage('package') {
      steps {
        sh 'mvn jar:jar'
      }
    }
    stage('publish to artifactory') {
      parallel {
        stage('Montreal Repo') {
          steps {
            echo 'push to Monreal'
            script {
              // Get Artifactory server instance, defined in the Artifactory Plugin administration page.

              def server = Artifactory.server "mtl-artifactory"

              // Create an Artifactory Maven instance.

              def rtMaven = Artifactory.newMavenBuild()
              def buildInfo
            }

          }
        }
        stage('India repo') {
          steps {
            echo 'push to India'
          }
        }
      }
    }
  }
}