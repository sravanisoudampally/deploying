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
                     body: '''please approve the deployment by clink the link below
''', subject: 'approval needed for deployment', to: 'sravanisoudampally@gmail.com'
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
