pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'oumayma42/test'
        DOCKER_TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🚀 Building Docker image ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
                    docker.build("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "📤 Pushing Docker image to Docker Hub..."
                    docker.withRegistry('', 'docker-hub-oumayma') {
                        docker.image("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}").push()
                    }
                    echo "✅ Docker image pushed successfully!"
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline finished! Pushed: ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
        }
        failure {
            echo "❌ Pipeline error. Check logs."
        }
    }
}
