def server = 'UNKNOWN'
def rtMaven = 'UNKNOWN'
def buildInfo = 'UNKNOWN'
def uploadSpec = readFile './upload-properties.json'

pipeline {
  agent any
  stages {
    stage('Get Artifactory Information') {
      steps {
        script {
          // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
          server = Artifactory.server "mtl-artifactory"
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
        sh 'mvn -Dmaven.test.skip=true -Dadditionalparam=-Xdoclint:none clean package javadoc:jar source:jar'
        //
        // script {
        //   // mvn -Dmaven.test.skip=true -Dadditionalparam=-Xdoclint:none clean package javadoc:jar source:jar gpg:sign deploy
        //   // rtMaven.opts = '-Dmaven.test.skip=true -Dadditionalparam=-Xdoclint:none'
        //   // buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean package javadoc:jar source:jar deploy'
        //
        // }
      }
    }

    stage('publish to artifactory') {
      parallel {
        stage('Publish Build info') {
          steps {
            echo 'push to Monreal'
            sript {
              // Upload to Artifactory.
              server.publishBuildInfo( server.upload (spec: uploadSpec))
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
