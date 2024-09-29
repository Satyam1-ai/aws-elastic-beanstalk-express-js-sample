pipeline {
  agent {
    docker {
      image 'node:16'
    }
  }
  stages {
    stage('Install Dependencies') {
      steps {
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
