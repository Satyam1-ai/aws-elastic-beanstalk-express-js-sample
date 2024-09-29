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
                    // Use `docker run` to directly run Snyk without nesting containers
                    sh '''
                        docker run -v /var/run/docker.sock:/var/run/docker.sock -e SNYK_TOKEN=${SNYK_TOKEN} snyk/snyk:docker snyk auth ${SNYK_TOKEN}
                        docker run -v /var/run/docker.sock:/var/run/docker.sock -e SNYK_TOKEN=${SNYK_TOKEN} snyk/snyk:docker snyk test
                    '''
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
