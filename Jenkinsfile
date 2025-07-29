pipeline {
    agent any // This runs directly on the main Jenkins agent

    stages {
        stage('Build and Deploy') {
            steps {
                sh 'mvn clean package -DskipTests'
                sh 'docker build -t my-web-app:latest .'
                sh '''
                    docker stop my-app || true
                    docker rm my-app || true
                    docker run -d --name my-app -p 8081:8081 my-web-app:latest
                '''
            }
        }
    }
}