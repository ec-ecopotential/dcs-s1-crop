def artserver = Artifactory.server('store.terradue.com')
def buildInfo = Artifactory.newBuildInfo()
buildInfo.env.capture = true

pipeline {

  options {
    // Kepp 5 builds history
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  agent {
    node {
      // docker community builder
      label 'ci-community-docker'
    }
  }

  stages {

    // Let's go!
    stage('Package & Dockerize') {
      steps {

        withMaven(
          // Maven installation declared in the Jenkins "Global Tool Configuration"
          maven: 'apache-maven-3.0.5' ) {
            sh 'mvn -B deploy'
        }

        script {
          artserver.publishBuildInfo buildInfo
        }
      }
    }

  }
}

