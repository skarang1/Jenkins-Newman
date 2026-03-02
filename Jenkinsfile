pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS'  // Use the NodeJS tool configured in Jenkins
    }
    
    stages {
        stage('Setup Node.js') {
            steps {
                sh '''
                    # Install Node.js directly using NodeSource
                    curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                    apt-get install -y nodejs libatomic1
                    
                    # Verify installation
                    node --version
                    npm --version
                '''
            }
        }
        stage('Install Newman') {
            steps {
                sh '''
                    npm install -g newman newman-reporter-html
                '''
            }
        }
        stage('Run API Tests') {
            steps {
                sh '''
                    npm install -g newman newman-reporter-html
                    newman run newman-jenkins-collection.postman_collection.json \
                        -e newman-jenkins-environment.postman_environment.json \
                        -r cli,html \
                        --reporter-html-export ./newman-report.html
                '''
            }
        }
    }
    post {
        always {
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'newman-report.html',
                reportName: 'API Test Report'
            ])
        }
    }
}