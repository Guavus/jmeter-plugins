def server = 'UNKNOWN'
def rtMaven = 'UNKNOWN'
def buildInfo = 'UNKNOWN'

pipeline {
  agent any
  stages {
    stage('Get Artifactory Information') {
      steps {
        script {
          // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
          server = Artifactory.server "mtl-artifactory"

          // Create an Artifactory Maven instance.
          rtMaven = Artifactory.newMavenBuild()
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
        sh 'mvn -Dmaven.test.skip=true -Dadditionalparam=-Xdoclint:none clean package javadoc:jar source:jar gpg:sign deploy'
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
            script {
              server.publishBuildInfo buildInfo
            }
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
