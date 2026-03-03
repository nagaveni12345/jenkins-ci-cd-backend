pipeline {
    agent any

    environment {
        SERVER_IP = "YOUR_SERVER_IP"
        SERVER_USER = "ubuntu"
        APP_NAME = "backend-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['server-ssh-key']) {
                    sh """
                    scp target/*.jar $SERVER_USER@$SERVER_IP:/home/ubuntu/$APP_NAME.jar
                    ssh $SERVER_USER@$SERVER_IP '
                        pkill -f $APP_NAME.jar || true
                        nohup java -jar /home/ubuntu/$APP_NAME.jar > app.log 2>&1 &
                    '
                    """
                }
            }
        }
    }
}
