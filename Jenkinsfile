pipeline {
    agent jenkins

    environment {
        DOCKER_REPO = "amdevops36/hello-world"
    }

    stages {

        stage('Prepare Tag') {
            steps {
                script {
                    echo "Current branch: ${env.BRANCH_NAME}"

                    if (env.BRANCH_NAME == 'main') {
                        env.IMAGE_TAG = "prod"
                    } else if (env.BRANCH_NAME == 'dev') {
                        env.IMAGE_TAG = "dev"
                    } else {
                        env.IMAGE_TAG = env.BRANCH_NAME.replaceAll('/', '-')
                    }

                    echo "Using image tag: ${env.IMAGE_TAG}"
                }
            }
        }

        stage('Build Image') {
            steps {
                sh """
                  docker build -t ${env.DOCKER_REPO}:${env.IMAGE_TAG} .
                """
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-credentiels',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh """
                  docker push ${env.DOCKER_REPO}:${env.IMAGE_TAG}
                """
            }
        }
    }
}
