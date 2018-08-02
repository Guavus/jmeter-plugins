def server = 'UNKNOWN'
def rtMaven = 'UNKNOWN'
def buildInfo = 'UNKNOWN'
def uploadSpec = 'UNKNOWN'

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

    stage('build') {
      steps {
        sh 'mvn -Dmaven.test.skip=true -Dadditionalparam=-Xdoclint:none clean package javadoc:jar source:jar'
      }
    }

    stage('publish to artifactory') {
      parallel {
        stage('Publish Build info') {
          steps {
            echo 'push to Monreal'
            script {
              //
              uploadSpec = """{
                "files": [
                  {
                    "pattern": "plugins/json/target/*.jar",
                    "target": "libs-snapshot-local"
                  }
                ]
              }"""
              // Upload to Artifactory.
              server.publishBuildInfo( server.upload (spec: uploadSpec))
          }
        }

        }
        stage('India repo') {
          steps {
            echo "push to India"
          }
        }
      }
    }
  }
}
