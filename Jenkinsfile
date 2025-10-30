pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("ganesh4365/devops-ci-cd:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        def image = docker.image("ganesh4365/devops-ci-cd:${BUILD_NUMBER}")
                        image.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Success!"
        }
        failure {
            echo "❌ Deployment Failed! Check Pipeline logs"
        }
    }
}
