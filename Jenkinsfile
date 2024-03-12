pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Replace this command with your build command
                sh 'npm install' // Or any other build command you use
            }
        }
        stage('Test') {
            steps {
                // Replace this command with your test command
                sh 'npm test' // Or any other test command you use
            }
        }
        stage('Email Approval') {
            steps {
                // Send an email for approval with a link
                mail to: 'sravanisoudampally@gmail.com',
                     subject: 'Approval needed for deployment',
                     body: 'Please approve the deployment by clicking <a href="http://yourdeploymentapprovallink">here</a>.'
            }
        }
        stage('Deploy') {
            when {
                beforeAgent true
                expression { currentBuild.result == 'SUCCESS' }
            }
            steps {
                // Deploy only if the email is approved
                input 'Deploy?'

                script {
                    def nginxServerUsername = 'ubuntu'
                    def nginxServerHost = '35.176.5.16'
                    def nginxServerPath = '/var/www/html'
                    def localFilePath = '/path/to/local/files'

                    // Use SCP to copy files to the Nginx server
                    sh "scp -r ${localFilePath} ${nginxServerUsername}@${nginxServerHost}:${nginxServerPath}"
                }
            }
        }
    }
}
