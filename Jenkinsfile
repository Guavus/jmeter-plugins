pipeline {
  agent any
  stages {
    stage('Get Artifactory Information') {
      steps {
        script {
          // Get Artifactory server instance, defined in the Artifactory Plugin administration page.

          def server = Artifactory.server "mtl-artifactory"

          // Create an Artifactory Maven instance.

          def rtMaven = Artifactory.newMavenBuild()
          def buildInfo
        }
      }
    }
    stage('Artifactory configuration') {
      steps {
        script {
          // Tool name from Jenkins configuration
          rtMaven.tool = "Maven-3.3.9"
          // Set Artifactory repositories for dependencies resolution and artifacts deployment.
          rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
          rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
        }
      }
    }

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
