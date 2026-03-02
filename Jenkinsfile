pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS'  // Use the NodeJS tool configured in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // This pulls your collection files
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