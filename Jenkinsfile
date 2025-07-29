pipeline {
    agent any

    // THIS BLOCK WAS MISSING
    tools {
        maven 'Default'
    }

    stages {
        stage('Build App & Docker Image') {
            steps {
                // This command will now find 'mvn'
                sh 'mvn clean package -DskipTests'
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