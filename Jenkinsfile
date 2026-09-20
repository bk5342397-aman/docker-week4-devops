pipeline {
    agent any

    options {
        timeout(time: 10, unit: 'MINUTES')
    }

    environment {
        APP_NAME = 'week9-cicd-app'
        IMAGE_NAME = 'week9-cicd-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building application..."'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                sh 'test -f index.html'
                sh 'echo "Basic test passed!"'
            }
        }

        stage('Package') {
            steps {
                sh 'echo "Packaging application..."'
                sh 'tar -czf app-package.tar.gz index.html Dockerfile'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build --progress=plain -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deployment stage completed for build $BUILD_NUMBER"'
            }
        }

        stage('Verify') {
            steps {
                sh 'docker image inspect $IMAGE_NAME:$BUILD_NUMBER >/dev/null'
                sh 'echo "Docker image verification passed!"'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD Pipeline failed.'
        }
    }
}
