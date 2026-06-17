pipeline{
    Agent any

    environment{
        SSH_CRED = 'jenkins-exam' //ssh credentials id
        TARGET_HOST_IP = '172.31.11.240' //private   ip
        USER = 'ubuntu' //user name
        REPO_URL = 'https://github.com/maheshsuryavanshi96k/Foodhub.git' //git repo url
        BRANCH = 'main' //branch name
        DEPLOY_DIR = '/home/ubuntu/Foodhub' //deployment directory
    }

    stages{
        stage('Git Clone'){
            steps{
                echo 'Cloning Git repository...'
                sh 'git clone -b ${BRANCH} ${REPO_URL}'
            }
        }
        stage('target host'){
            steps{
                echo 'SSH into target host...'
                sh '''
                    ssh -o StrictHostKeyChecking=no ${USER}@${TARGET_HOST_IP} 'mkdir -p ${DEPLOY_DIR}'
                    scp -r * ${USER}@${TARGET_HOST_IP}:${DEPLOY_DIR}
            }
        }
        stage('creating nginx service'){
            steps{
                echo 'Creating Nginx service...'
                sh '''
                    ssh -o StrictHostKeyChecking=no ${USER}@${TARGET_HOST_IP}
                    sudo apt update &&
                    sudo apt install nginx -y &&
                    sudo systemctl start nginx &&
                    sudo systemctl enable nginx &&
                    sudo systemctl status nginx &&
                    sudo cp -r /home/ubuntu/* /var/www/html/
                    
                '''
            }
        }
    }
}
