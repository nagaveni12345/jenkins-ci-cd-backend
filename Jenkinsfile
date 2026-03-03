pipeline { 
    agent any 
 
    environment { 
        EC2_USER = "ec2-user" 
        EC2_HOST = "<YOUR-APP-EC2-PRIVATE-IP>" 
        APP_DIR = "/home/ec2-user/backend-app" 
    } 
 
    stages { 
 
        stage('Clone Repository') { 
            steps { 
                git 'https://github.com/nagaveni12345/jenkins-ci-cd-backend.git' 
            } 
        } 
 
        stage('Deploy Application') { 
            steps { 
                sh ''' 
                ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} " 
                rm -rf ${APP_DIR} 
                mkdir -p ${APP_DIR} 
                " 
 
                scp -r * ${EC2_USER}@${EC2_HOST}:${APP_DIR} 
 
                ssh ${EC2_USER}@${EC2_HOST} " 
                cd ${APP_DIR} 
                npm install 
                pm2 delete backend-app || true 
                pm2 start app.js --name backend-app 
                pm2 save 
                " 
                ''' 
            } 
        } 
    }

}
