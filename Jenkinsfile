pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token')  // Reference the Snyk API token from Jenkins credentials
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'apk add --no-cache nodejs npm'  // Install Node.js and npm in the Docker container
                sh 'npm install'
            }
        }
        stage('Install Snyk CLI') {
            steps {
                sh 'npm install -g snyk'  // Install Snyk CLI globally using npm
            }
        }
        stage('Snyk Security Scan') {
            steps {
                sh 'snyk auth ${SNYK_TOKEN}'  // Authenticate Snyk CLI
                sh 'snyk test'  // Run the Snyk security scan
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'  // Run tests (the current placeholder script does not execute any real tests)
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-node-app .'  // Build the Docker image
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
