pipeline {
    agent any

    stages {
        stage('Install Node.js (If Required)') {
            steps {
                sh '''
                curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
                sudo apt-get install -y nodejs
                node -v
                npm -v
                '''
            }
        }

        stage('Checkout Code') {
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

