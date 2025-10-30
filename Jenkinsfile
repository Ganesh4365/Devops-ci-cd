pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = "ganesh4365/devops-ci-cd"
        CONTAINER_NAME = "devops-app"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Ganesh4365/Devops-ci-cd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('DockerHub Login') {
            steps {
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }

        stage('Deploy on EC2') {
            steps {
                sh """
                if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                    docker stop ${CONTAINER_NAME}
                    docker rm ${CONTAINER_NAME}
                fi

                docker pull ${IMAGE_NAME}:latest
                docker run -d -p 80:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Visit: http://<EC2-Public-IP>/"
        }
        failure {
            echo "❌ Deployment Failed! Check Pipeline logs"
        }
    }
}
