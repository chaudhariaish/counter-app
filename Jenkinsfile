pipeline {
    agent any

    stages {
        stage('Install the dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('Build the code') {
            steps {
                sh "npm run build"
            }
        }

        stage('Deploy') {
            steps {
                 withCredentials([sshUserPrivateKey(credentialsId: 'ec2-jenkins', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                     sh '''
                     mkdir -p ~/.ssh
                     ssh-keyscan -H 13.235.100.51 >> ~/.ssh/known_hosts
                        scp -i $SSH_KEY -r build/* $SSH_USER@13.235.100.51:/var/www/html
                        ssh -i $SSH_KEY $SSH_USER@13.235.100.51 'sudo systemctl restart httpd'
                     '''
                 }   
            }
        }
    }
}
