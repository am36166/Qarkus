pipeline {
    agent any

    environment {
        APP_NAME = "hello-quarkus"
        IMAGE_NAME = "amdevops36/hello-world"
        CONTAINER_NAME = "hello-quarkus-app"
        APP_PORT = "8080"
    }

    tools {
        maven 'M3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "Building Docker Image for ${APP_NAME}: ${IMAGE_NAME}:${BUILD_NUMBER}"
                    docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                '''
            }
        }

        stage('Deploy (main only)') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                    docker run -d \
                      -p $APP_PORT:8084 \
                      --name $CONTAINER_NAME \
                      $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for branch: ${env.BRANCH_NAME}"
        }
    }
}
