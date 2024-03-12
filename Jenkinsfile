pipeline {
    agent any
    
    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }
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
        stage('Approval') {
            steps {
                script {
                    def approvalToken = 'approval-' + UUID.randomUUID().toString()
                    def triggerUrl = "${env.BUILD_URL}input/Proceed%20or%20Abort/proceedEmpty"
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
    }
    
    post {
        failure {
            script {
                // If the deployment fails, send a notification
                emailext (
                    subject: "Deployment failed: ${currentBuild.fullDisplayName}",
                    body: "The deployment of ${env.JOB_NAME} (${env.BUILD_NUMBER}) has failed.",
                    to: "sravanisoudampally@gmail.com"
                )
            }
        }
    }
}

// Deploy stage runs only after approval is granted
stage('Deploy') {
    when {
        expression {
            // Check if the approval token matches the expected token
            return env.APPROVAL_TOKEN != null && params.token == env.APPROVAL_TOKEN
        }
    }
    steps {
        script {
            parallel(
                "Deploy-Branch1": {
                    sh 'echo Deploying project to Branch1...'
                    // Add deployment steps for Branch1 here
                },
                "Deploy-Branch2": {
                    sh 'echo Deploying project to Branch2...'
                    // Add deployment steps for Branch2 here
                }
            )
        }
    }
}
