pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t jenkins-deployment-portal:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running application health test...'
                sh 'docker run -d --name ci-test-container -p 5003:5000 jenkins-deployment-portal:latest'
                sh 'sleep 5'
                sh 'curl -f http://localhost:5003/health'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker stop jenkins-deployment-portal || true
                    docker rm jenkins-deployment-portal || true

                    docker run -d \
                      --name jenkins-deployment-portal \
                      -p 5002:5000 \
                      --restart unless-stopped \
                      jenkins-deployment-portal:latest
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up test container...'
            sh 'docker stop ci-test-container || true'
            sh 'docker rm ci-test-container || true'
        }

        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed. Check the console output.'
        }
    }
}
