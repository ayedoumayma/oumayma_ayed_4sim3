pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'votre-username/student-management'
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/votre-repo/student-management.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Docker image built and pushed successfully to Docker Hub!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
    }
}
