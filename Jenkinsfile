pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './gradlew clean build'
      }
    }
    stage('Deploy') {
      steps {
        sshPublisher(
          publishers: [
            sshPublisherDesc(
              configName: 'test-app',
              transfers: [
                sshTransfer(
                  execCommand: '''
                    cd opt/backend
                    docker stop backend || true
                    docker rm backend || true
                    docker build --no-cache -t backend:${BUILD_NUMBER} .
                    docker run -d --name backend -p 8080:8080 backend:${BUILD_NUMBER}
                    echo $?
                  '''
                )
              ]
            )
          ]
        )
      }
    }
  }
}