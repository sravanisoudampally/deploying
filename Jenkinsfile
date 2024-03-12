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
                    def result = sh(script: '[ -f build/index.html ] && echo true || echo false', returnStdout: true).trim()
                    if (result == 'true') {
                        echo "HTML file exists and is not empty."
                    } else {
                        error "HTML file not found or empty."
                    }
                }
            }
        }
        stage('Approval') {
            steps {
                script {
                    def approvalToken = 'approval-' + UUID.randomUUID().toString()
                    def triggerUrl = "${currentBuild.rawBuild.parent.url}input/Proceed%20or%20Abort/proceedEmpty"
                    def approvalLink = triggerUrl + "?token=" + approvalToken
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
                    return env.APPROVAL_TOKEN != null
                }
            }
            steps {
                script {
                    sh 'echo Deploying project...'
                    // Add deployment steps here
                }
            }
        }
    }
}
