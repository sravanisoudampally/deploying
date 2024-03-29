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
                script {
                    def approvalToken = 'approval-' + UUID.randomUUID().toString()
                    def triggerUrl = "${env.BUILD_URL}input/Proceed%20or%20Abort/proceedEmpty"
                    def approvalLink = triggerUrl + "?token=" + approvalToken
                    def body = "Please approve the deployment by clicking the link below:\n${approvalLink}"
                    emailext (
                        subject: 'Approval needed for deployment',
                        body: body,
                        to: 'sravanisoudampally@gmail.com'
                    )
                    // Store the token in an environment variable for further validation
                    env.APPROVAL_TOKEN = approvalToken
                }
            }
            post {
                always {
                    input 'Click the link in the email to approve deployment and proceed'
                }
            }
        }
        stage('Deploy') {
            steps {
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
