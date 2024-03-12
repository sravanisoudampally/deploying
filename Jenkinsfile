pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Create a build directory and copy index.html into it
                sh 'mkdir -p build'
                sh 'cp index.html build/'
                
                // Echo a message indicating that the build is completed
                sh 'echo Build completed'
            }
        }
        stage('Test') {
            steps {
                script {
                    // Check if the index.html file exists in the build directory
                    def fileExists = fileExists('build/index.html')
                    echo "HTML file exists: ${fileExists}"
                }
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
