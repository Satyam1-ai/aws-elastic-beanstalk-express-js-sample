pipeline {
  agent {
    docker {
      image 'docker:latest'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  stages {
    stage('Install Dependencies') {
      steps {
        sh 'apk add --no-cache nodejs npm' // Install Node.js and npm in the Docker container
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t my-node-app .'
      }
    }
  }
  post {
    always {
      echo 'Pipeline completed.'
    }
  }
}
