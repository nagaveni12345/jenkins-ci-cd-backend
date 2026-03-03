pipeline {
    agent any

    stages {

        stage('Deploy Application') {
            steps {
                sh """
                ssh -i /var/lib/jenkins/.ssh/your-key.pem -o StrictHostKeyChecking=no ec2-user@13.233.179.126 '
                    cd /home/ec2-user/backend-app
                    git pull origin main
                    npm install
                    pm2 restart backend-app || pm2 start app.js --name backend-app
                    pm2 save
                '
                """
            }
        }
    }
}
