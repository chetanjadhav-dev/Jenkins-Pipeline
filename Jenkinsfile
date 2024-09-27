pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'                  // Username for Ubuntu EC2 instance
        EC2_IP = 'http://54.209.53.51/'  // Replace with your EC2 public IP
        SSH_KEY = credentials('f49fcc76-ec68-44f1-9913-e71abb7919f1') // Jenkins credential ID for your SSH key
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git 'https://github.com/chetanjadhav-dev/Jenkins-Pipeline.git' // Replace with your repository URL
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['f49fcc76-ec68-44f1-9913-e71abb7919f1']) {
                    // Upload the HTML files to the EC2 instance's Apache directory
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
