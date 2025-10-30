pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    image = docker.build("ganesh4365/devops-ci-cd:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        image.push()   // Push build number tag
                        image.push("latest") // Push latest tag ✅
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker Image pushed to DockerHub successfully!"
            echo "👉 Tags: ${BUILD_NUMBER}, latest"
        }
        failure {
            echo "❌ Deployment Failed! Check Pipeline logs"
        }
    }
}
