pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // This pulls your collection files
            }
        }
        stage('Run API Tests') {
            steps {
                sh '''
                    npm install -g newman
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
                reportDir: '.',
                reportFiles: 'newman-report.html',
                reportName: 'API Test Report'
            ])
        }
    }
}