pipeline {
    agent any

    stages {

        stage('Clean workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YareliGabino12/playwright12.git'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                sh '''
                docker run --rm \
                  -v "$WORKSPACE:/repo" \
                  -w /repo \
                  mcr.microsoft.com/playwright:v1.57.0-noble \
                  bash -c "npm ci && npx playwright test"
                '''
            }
        }
    }

    post {
        always {
            publishHTML([
                reportName : 'Playwright Report',
                reportDir  : 'playwright-report',
                reportFiles: 'index.html',
                keepAll    : true,
                alwaysLinkToLastBuild: true,
                allowMissing: false
            ])
        }
    }
}
