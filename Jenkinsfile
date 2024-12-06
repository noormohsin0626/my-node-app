pipeline {
    agent any

    tools {
        nodejs "NodeJS" // Replace with the NodeJS name from Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/noormohsin0626/my-node-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy to Staging') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'staging-server-ssh-key', keyFileVariable: 'KEYFILE')]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i $KEYFILE user@staging-server \
                    "cd /path/to/app && git pull && npm install --production && pm2 restart all"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Build and deployment succeeded!"
        }
        failure {
            echo "Build or deployment failed."
        }
    }
}
