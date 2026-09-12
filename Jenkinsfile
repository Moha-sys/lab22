pipeline {
    agent any

    environment {
        // Fetches username and password/token from Jenkins credentials store (Credential ID: 'docker')
        DOCKER_HUB_CREDS = credentials('docker')
        IMAGE_NAME       = 'mohadevv/hello-app'
        IMAGE_TAG        = "${BUILD_NUMBER}"
    }

    stages {
        stage('Build Setup') {
            steps {
                echo '=== Build Setup ==='
                sh 'docker --version'
            }
        }

        stage('Docker Build') {
            steps {
                echo '=== Docker Build ==='
                sh 'docker build -t app:local .'
            }
        }

        stage('Login & Tag') {
            steps {
                echo '=== Login & Tag ==='
                sh '''
                    echo "$DOCKER_HUB_CREDS_PSW" | docker login -u "$DOCKER_HUB_CREDS_USR" --password-stdin
                    docker tag app:local ${IMAGE_NAME}:${IMAGE_TAG}
                    docker tag app:local ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Docker Push') {
            steps {
                echo '=== Docker Push ==='
                sh '''
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        always {
            echo '=== Cleanup ==='
            sh '''
                docker logout || true
                docker rmi app:local ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest || true
            '''
        }
    }
}
