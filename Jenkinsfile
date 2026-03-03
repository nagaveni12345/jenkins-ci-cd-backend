pipeline {
    agent any

    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "13.233.179.126"   // 🔥 REPLACE WITH YOUR REAL EC2 IP
        APP_DIR  = "/home/ec2-user/backend-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/nagaveni12345/jenkins-ci-cd-backend.git',
                    credentialsId: 'git-hub'
            }
        }

        stage('Deploy Application') {
    steps {
        sh """
        ssh -i /var/lib/jenkins/.ssh/ra-key-mumbai.pem -o StrictHostKeyChecking=no ec2-user@13.233.179.126 '
            rm -rf /home/ec2-user/backend-app
            mkdir -p /home/ec2-user/backend-app
        '

        scp -i /var/lib/jenkins/.ssh/ra-key-mumbai.pem -o StrictHostKeyChecking=no -r * ec2-user@13.233.179.126:/home/ec2-user/backend-app

        ssh -i /var/lib/jenkins/.ssh/ra-key-mumbai -o StrictHostKeyChecking=no ec2-user@13.233.179.126 '
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
