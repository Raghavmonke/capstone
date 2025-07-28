pipeline {
    agent any

    tools {
        // This tells Jenkins to use the Maven tool we will configure in the UI
        maven 'Default'
    }

    stages {
        stage('Build') {
            steps {
                // This command compiles your Java code into a .jar file
                sh 'mvn clean package'
            }
        }

        stage('Deploy to EC2 Host') {
            steps {
                // This block uses the SSH plugin to deploy the app
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        // This must match the server name you configure in Jenkins
                        configName: 'ec2-host',
                        transfers: [
                            sshTransfer(
                                sourceFiles: 'target/*.jar',
                                removePrefix: 'target',
                                remoteDirectory: 'app'
                            )
                        ],
                        // This is the script that will run on your server
                        execCommand: '''
                            # Find and stop the old version of the app, if it exists
                            PID=$(pgrep -f 'app.jar' || true)
                            if [ -n "$PID" ]; then
                              echo "Stopping old process with PID: $PID"
                              kill -9 $PID
                            fi

                            # Start the new version of the app
                            echo "Starting new application on port 8081..."
                            nohup java -jar ~/app/*.jar > app.log 2>&1 &
                        '''
                    )
                ])
            }
        }
    }
}