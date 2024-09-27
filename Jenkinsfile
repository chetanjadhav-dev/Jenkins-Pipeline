pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'                  // Username for Ubuntu EC2 instance
        EC2_IP = '54.209.53.51'              // EC2 public IP (without 'http://')
        SSH_KEY = credentials('f49fcc76-ec68-44f1-9913-e71abb7919f1') // SSH key for EC2 instance
        GITHUB_CREDS = credentials('671b24cd-d9b1-4718-9451-c496cd428c0f') // Updated credential ID with PAT
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git credentialsId: '671b24cd-d9b1-4718-9451-c496cd428c0f', url: 'https://github.com/chetanjadhav-dev/Jenkins-Pipeline.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['f49fcc76-ec68-44f1-9913-e71abb7919f1']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY} -r * ${EC2_USER}@${EC2_IP}:/var/www/html/
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Website successfully deployed to EC2!'
        }
        failure {
            echo 'Deployment to EC2 failed!'
        }
    }
}
