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
                emailext (
                    body: 'Please approve the deployment by clicking the link below.',
                    subject: 'Approval needed for deployment',
                    to: 'sravanisoudampally@gmail.com'
                )
                input message: 'Approve deployment?', submitter: 'sravani'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo Deploying project...'
                // Add deployment steps here
            }
        }
    }
}
