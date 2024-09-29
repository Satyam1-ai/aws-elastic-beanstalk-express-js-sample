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
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Using Snyk Docker image directly to scan the application
                    docker.image('snyk/snyk:docker')
                        .inside('-v /var/run/docker.sock:/var/run/docker.sock') {
                            sh 'snyk auth ${SNYK_TOKEN}'  // Authenticate Snyk using the API token
                            sh 'snyk test || exit 1'  // Run a Snyk security scan; fail if vulnerabilities are found
                        }
                }
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

