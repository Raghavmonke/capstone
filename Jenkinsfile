pipeline {
    agent any

    tools {
        maven 'Default'
        dockerTool 'Default-Docker'
    }

    stages {
        // This is a temporary stage to print debug information
        stage('Debug Tools') {
            steps {
                echo "--- DEBUGGING INFO ---"

                // Print the system PATH to see if the Docker tool directory was added
                sh 'echo $PATH'

                // Print the full command for 'docker' to see where the system is looking
                sh 'which docker'

                echo "--- END DEBUGGING INFO ---"
            }
        }

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