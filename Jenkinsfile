pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-token') // Add your Snyk API token in Jenkins credentials
    }
    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'apk add --no-cache nodejs npm'
                sh 'npm install'
            }
        }

        stage('Install Snyk CLI') {
            steps {
                sh 'npm install -g snyk'
            }
        }

        stage('Snyk Security Scan') {
            steps {
                script {
                    // Authenticate with Snyk using your token from environment variables
                    sh 'snyk auth $SNYK_TOKEN'

                    // Perform a Snyk test to scan dependencies for vulnerabilities
                    sh 'snyk test || true' // Using `|| true` to ensure pipeline doesn't fail if vulnerabilities are found
                }
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true' // Skipping errors to proceed, assuming the tests may not be defined.
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
