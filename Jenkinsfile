pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'votre-username/student-management'
        DOCKER_TAG = 'latest'
        MAVEN_OPTS = '-Dmaven.wagon.http.retryHandler.count=3 -Dmaven.wagon.httpconnectionManager.ttlSeconds=120'
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
                // Ajoutez des options pour les timeouts
                sh 'mvn clean package -DskipTests -Dmaven.wagon.http.readTimeout=300000 -Dmaven.wagon.http.connectionTimeout=300000'
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
}
