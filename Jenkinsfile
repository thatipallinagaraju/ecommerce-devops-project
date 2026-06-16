pipeline {

    agent any

    environment {
        IMAGE_NAME = "ecommerce-app"
        CONTAINER_NAME = "ecommerce-container"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME app/'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker run -d \
                -p 80:80 \
                --name $CONTAINER_NAME \
                $IMAGE_NAME
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 10
                curl -f http://localhost
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }
    }
}
