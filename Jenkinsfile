pipeline {
  steps {
    stage {
      steps {
        sh 'docker run docker run -it --rm --name jar-builder -v $(pwd):/usr/src/mymaven -w /usr/src/mymaven maven:3-jdk-8 mvn -Dmaven.test.skip=true clean install'
      }
    }
  }
}
