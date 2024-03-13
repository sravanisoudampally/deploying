pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mkdir -p build'
                sh 'cp index.html build/'
                sh 'echo Build completed'
            }
        }
        stage('Test') {
            steps {
                script {
                    def fileExists = fileExists('build/index.html')
                    echo "HTML file exists: ${fileExists}"
                }
            }
        }
        stage('Email Approval') {
            steps {
                 input message: 'Click the link in the email to approve deployment and proceed'
                script {
                    // Send an email for approval with a link
                    def approvalLink = 'http://yourdeploymentapprovallink'
                    mail to: 'sravanisoudampally@gmail.com',
                         subject: 'Approval needed for deployment',
                         body: "Please approve the deployment by clicking <a href='${approvalLink}'>here</a>."
                }
            }
        }
        stage('Deploy') {
     
            steps {
              
                input message: 'Click the link in the email to approve deployment and proceed'
                
                script {
                    def nginxServerUsername = 'ubuntu'
            def nginxServerHost = '35.176.5.16'
            def nginxServerPath = '/var/www/html'
            def localFilePath = 'build/'

            // Use SCP to copy files to the Nginx server
            sh "scp -r ${localFilePath} ${nginxServerUsername}@${nginxServerHost}:${nginxServerPath}"
                }
            }
        }
    }
}
