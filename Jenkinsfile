pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'adrian0526'
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds')
        IMAGE_NAME = 'proyecto-segundo-corte'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Adrian25450/proyecto-segundo-corte.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "whoami"
                    sh "groups"
                    sh "echo $PATH"
                    sh "ls -l /usr/bin/docker" // Verificar permisos del binario docker
                    sh "ls -l /var/run/docker.sock" // Verificar permisos del socket docker
                    sh "sh -c '/usr/bin/docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER} -f ./2fa-nodejs/Dockerfile ./2fa-nodejs'"
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh "docker rmi ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER} || true"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completado exitosamente. Imagen subida a Docker Hub."
        }
        failure {
            echo "❌ Hubo un error durante la ejecución del pipeline."
        }
    }
}
