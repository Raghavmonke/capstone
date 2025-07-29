pipeline {
    agent any

    stages {
        stage('Build App & Docker Image') {
            steps {
                sh 'mvn clean package'
                sh 'docker build -t my-web-app:latest .'
            }
        }
        stage('Deploy Docker Container') {
            steps {
                sh '''
                    docker stop my-app || true
                    docker rm my-app || true
                    docker run -d --name my-app -p 8081:8081 my-web-app:latest
                '''
            }
        }
    }
}