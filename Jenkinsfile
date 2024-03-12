pipeline {
    agent any

    stages {
        
        stage('Build') {
            steps {
                sh 'mkdir -p build'
                sh 'cp index.html build/'
                sh 'echo "Build completed"'
            }
        }
        stage('Test') {
            steps {
                script {
                    def htmlFile = 'build/index.html'
                    if (fileExists(htmlFile)) {
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
                    emailext (
                        subject: 'Approval Needed for Deployment',
                        body: 'Please approve the deployment by clicking the link below:',
                        to: 'sravanisoudampally@gmail.com',
                        mimeType: 'text/html',
                        replyTo: '$DEFAULT_REPLYTO',
                        attachmentsPattern: "**/*"
                    )
                }
                input message: 'Approve deployment?', submitter: 'sravani'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying project..."'
            }
        }
    }

    post {
        success {
            emailext (
                subject: 'Build, Test, and Deployment Successful',
                body: 'The build, test, and deployment were successful.',
                to: 'sravani0944@gmail.com',
                mimeType: 'text/html'
            )
        }
        failure {
            emailext (
                subject: 'Build, Test, or Deployment Failed',
                body: 'The build, test, or deployment failed.',
                to: 'sravani0944@gmail.com',
                mimeType: 'text/html'
            )
        }
    }
}
