pipeline {
    agent any

    environment {
        // Define environment variables
        EC2_HOST = '51.20.219.202'
        EC2_USER = 'ubuntu'
        // Assuming you've added your SSH private key to Jenkins' credentials store
        SSH_KEY_ID = 'apache-ssh'
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out from GitHub
                git url: 'https://github.com/kamran-bwp/apache-lab.git', branch: 'main'
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // SSH and SCP commands to transfer files and restart Apache
                    sshagent(credentials: [SSH_KEY_ID]) {
                        // Copy files to the Apache directory on EC2
                        sh "scp -o StrictHostKeyChecking=no -r /var/lib/jenkins/workspace/CI-CD-pipeline/index2.html ${EC2_USER}@${EC2_HOST}:/var/www/html/"
                        // Optional: Run any commands on EC2, like setting permissions or restarting Apache
                        sh "ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} 'sudo systemctl restart apache2'"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
