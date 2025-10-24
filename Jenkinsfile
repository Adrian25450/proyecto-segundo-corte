pipeline {
    agent any

    environment {
        // Define variables for Docker Hub
        DOCKERHUB_USERNAME = 'adrian0526' 
        DOCKERHUB_TOKEN = credentials('dockerhub-token') 
        IMAGE_NAME = 'segundocorte-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/wpcodevo/2fa-nodejs.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using docker-compose
                    sh "docker-compose build"
                    // Tag the image for Docker Hub
                    sh "docker tag 2fa-nodejs_app ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using credentials
                    sh "echo ${DOCKERHUB_TOKEN} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Remove the Docker image locally after pushing
                    sh "docker rmi ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}"
                    // Remove docker-compose created images
                    sh "docker-compose down --rmi all"
                }
            }
        }
    }
}
