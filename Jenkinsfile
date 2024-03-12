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
                // Send an email for approval with a link
                script {
                    mail to: 'sravanisoudampally@gmail.com',
                         subject: 'Approval needed for deployment',
                         body: 'Please approve the deployment by clicking <a href="http://yourdeploymentapprovallink">here</a>.'
                }
            }
        }
        stage('Deploy') {
            when {
                beforeAgent true
                expression { currentBuild.result == 'SUCCESS' }
            }
            steps {
                // Wait for manual input to proceed with deployment
                input message: 'Deploy?', ok: 'Deploy'
                
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
