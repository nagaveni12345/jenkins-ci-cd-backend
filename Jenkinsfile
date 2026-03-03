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
                sh """
                ssh -i ${KEY_PATH} -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                    rm -rf ${APP_DIR}
                    mkdir -p ${APP_DIR}
                '

                scp -i ${KEY_PATH} -o StrictHostKeyChecking=no -r * ${EC2_USER}@${EC2_HOST}:${APP_DIR}

                ssh -i ${KEY_PATH} -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                    cd ${APP_DIR}
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
