pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'ayushhmxn/docker_jenkins_demo'
        IMAGE_TAG = '1.0'
    }

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Push') {
            when {
                expression { currentBuild.changeSets.size() > 0 }
            }
            steps {
                script {
                    def app = docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")

                    docker.withRegistry('https://index.docker.io/v1/', 'my-docker-hub-credentials-id') {
                        app.push()
                    }

                    echo "Successfully pushed ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Cleaning up...'
        }
    }
}
