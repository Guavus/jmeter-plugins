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
    stage ('publish to artifactory') {
      parallel {
        stage ('Montreal Repo') {
            steps {
                script{
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
                    def server = Artifactory.server 'mtl-docker-artifactory'
                    def rtDocker = Artifactory.docker server: server

                    // Read the download and upload specs:
                    def uploadSpec = readFile 'jenkins-examples/pipeline-examples/resources/props-upload.json'

                    // Upload files to Artifactory:
                    def buildInfo = server.upload spec: uploadSpec

                    // Publish the merged build-info to Artifactory
                    server.publishBuildInfo buildInfo
                }
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
