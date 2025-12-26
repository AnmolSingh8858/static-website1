pipeline {
    agent any

    environment {
        IMAGE_NAME = "anmolsingh8858/static-website"  // <-- YAHAN APNA DOCKER HUB USERNAME DAAL
        CONTAINER_NAME = "my-static-site"
        HOST_PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'GitHub se code pull ho gaya!'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Docker image bana raha hoon...'
                sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo 'Purana container band kar raha hoon...'
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Deploy New Container') {
            steps {
                echo 'Naya container deploy kar raha hoon...'
                sh 'docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

        stage('Test Site') {
            steps {
                echo 'Site test kar raha hoon...'
                sh 'curl -f http://localhost:${HOST_PORT} || exit 1'
                echo 'Site successfully live ho gaya! 🎉'
            }
        }
    }

    post {
        success {
            echo 'Full CI/CD Pipeline Successful! Har Har Mahadev 🙏🔱'
        }
        failure {
            echo 'Pipeline fail hua... logs check kar bhai'
        }
        always {
            echo 'Pipeline khatam!'
        }
    }
}
