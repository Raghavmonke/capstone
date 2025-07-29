pipeline {
    // This agent block runs your build inside a container that already has Docker
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock' 
        }
    }

    // We still need Maven, but no longer need to specify a Docker tool
    tools {
        maven 'Default'
    }

    stages {
        stage('Build App & Docker Image') {
            steps {
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