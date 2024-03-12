pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mkdir -p build'
                sh 'cp index.html build/'
                sh 'echo Build completed'
                input message: 'Approve deployment?', ok: 'Proceed', submitter: 'sravanisoudampally'
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
        
        stage('Approval') {
            steps {
                script {
                    def approvalToken = 'approval-' + UUID.randomUUID().toString()
                    def approvalLink = "${env.BUILD_URL}input/Proceed%20or%20Abort/proceedEmpty?token=${approvalToken}"
                    def body = "Please approve the deployment by clicking the link below:\n${approvalLink}"
                    emailext (
                        body: body,
                        subject: 'Approval needed for deployment',
                        to: 'sravanisoudampally@gmail.com'
                    )
                    // Store the token in environment variable for further validation
                    env.APPROVAL_TOKEN = approvalToken
                }
            }
        }
        
        stage('Deploy') {
            when {
                expression {
                    // Check if the approval token matches the expected token
                    return env.APPROVAL_TOKEN != null && params.token == env.APPROVAL_TOKEN
                }
            }
            steps {
                // Add deployment steps here
                sh 'echo Deployment started'
            }
        }
    }
}
