pipeline {
    agent any

    stages {
        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🚀 Building Docker image oumayma42/test:latest"
                    def customImage = docker.build("oumayma42/test:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-oumayma') {
                        echo "Pushing image..."
                        docker.image("oumayma42/test:latest").push()
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline error. Check logs."
        }
    }
}
