pipeline {
    agent any

    environment {
        CREDENTIALS_ID = '3.110.45.32'
        SERVER_USER = 'ubuntu'
        SERVER_IP = '13.201.163.216'
        REMOTE_DIR = '/var/www/html/'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/apc121/my-web-app-backend.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'ng build'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sshagent(credentials: ['3.110.45.32']) {
                        sh '''
                        rsync -avz -e "ssh -o StrictHostKeyChecking=no" --delete ./dist/ ${SERVER_USER}@${SERVER_IP}:${REMOTE_DIR}
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
