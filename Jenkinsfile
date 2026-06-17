pipeline {
    agent any

    environment {
        SSH_CRED = 'jenkins-exam'
        TARGET_HOST_IP = '172.31.11.240'
        USER = 'ubuntu'
        REPO_URL = 'https://github.com/maheshsuryavanshi96k/Foodhub.git'
        BRANCH = 'main'
        DEPLOY_DIR = '/home/ubuntu/Foodhub'
    }

    stages {

        stage('Git Clone') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Deploy to Target Host') {
            steps {
                sshagent(credentials: ['jenkins-exam']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${USER}@${TARGET_HOST_IP} 'mkdir -p ${DEPLOY_DIR}'
                        scp -o StrictHostKeyChecking=no -r * ${USER}@${TARGET_HOST_IP}:${DEPLOY_DIR}
                    """
                }
            }
        }

        stage('Install & Configure Nginx') {
            steps {
                sshagent(credentials: ['jenkins-exam']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${USER}@${TARGET_HOST_IP} '
                        sudo apt update &&
                        sudo apt install nginx -y &&
                        sudo systemctl enable nginx &&
                        sudo systemctl restart nginx &&
                        sudo rm -rf /var/www/html/* &&
                        sudo cp -r /home/ubuntu/Foodhub /var/www/html/ &&
                        sudo curl http://localhost:80/Foofhub
                        '
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                    curl http://localhost:80/Foodhub}
                """
            }
        }
    }
}