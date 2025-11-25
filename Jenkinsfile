pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'votredockerhub/student-management'
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ayedoumayma/oumayma_ayed_4sim3.git',
                credentialsId: 'dae9c1dd-f025-4134-9f2a-b18869ab4eaa'
            }
        }
        
        stage('Build JAR') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}").push()
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Docker image built and pushed successfully to Docker Hub!'
            echo "Image: ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
        }
        failure {
            echo '❌ Pipeline failed! Check the logs for details.'
        }
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
