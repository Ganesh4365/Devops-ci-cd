pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = 'ganesh4365/devops-ci-cd-project'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Ganesh4365/Devops-ci-cd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push()
                        docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }

        stage('Deploy Container on EC2') {
            steps {
                script {
                    sh "docker stop devops-ci || true"
                    sh "docker rm devops-ci || true"
                    sh "docker run -d -p 3000:3000 --name devops-ci ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
}
