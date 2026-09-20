pipeline {
    agent any

    options {
        timeout(time: 10, unit: 'MINUTES')
    }

    environment {
        APP_NAME = 'week9-app'
        IMAGE_NAME = 'week9-cicd-app'
        APP_PORT = '8082'
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
                sh 'grep -q "Welcome to My Docker Website" index.html'
                sh 'echo "Automated tests passed!"'
            }
        }

        stage('Package') {
            steps {
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
                sh '''
                    docker rm -f $APP_NAME 2>/dev/null || true
                    docker run -d --name $APP_NAME -p $APP_PORT:80 $IMAGE_NAME:$BUILD_NUMBER
                    sleep 3
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'docker ps --filter name=$APP_NAME --format "{{.Names}}" | grep -q "$APP_NAME"'
                sh 'docker image inspect $IMAGE_NAME:$BUILD_NUMBER >/dev/null'
                sh 'echo "Deployment verification passed!"'
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
