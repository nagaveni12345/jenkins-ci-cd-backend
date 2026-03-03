pipeline {
    agent any

    environment {
        EC2_HOST = "13.233.179.126"        // Your EC2 Public IP
        EC2_USER = "ec2-user"             // Change to ubuntu if needed
        KEY_PATH = "/var/lib/jenkins/.ssh/my-ec2-key.pem"
        APP_DIR  = "/home/ec2-user/backend-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/nagaveni12345/jenkins-ci-cd-backend.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

       stage('Deploy to EC2') {
    steps {
        sshagent(['ec2-key']) {
            sh """
            ssh -o StrictHostKeyChecking=no ec2-user@13.233.179.126 '
                rm -rf /home/ec2-user/backend-app
                mkdir -p /home/ec2-user/backend-app
            '

            scp -o StrictHostKeyChecking=no -r * ec2-user@13.233.179.126:/home/ec2-user/backend-app

            ssh -o StrictHostKeyChecking=no ec2-user@13.233.179.126 '
                cd /home/ec2-user/backend-app
                npm install
                pm2 delete backend-app || true
                pm2 start app.js --name backend-app
                pm2 save
            '
            """
        }
    }
}
}

}
