pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.42.1-jammy'
            args '--ipc=host'
        }
    }

    stages {

        stage('Debug workspace') {
            steps {
                sh '''
                  echo "=== Workspace inside container ==="
                  pwd
                  ls -la
                '''
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                sh 'npx playwright test'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
        }
        success {
            echo '✅ Pruebas ejecutadas correctamente'
        }
        failure {
            echo '❌ Fallaron las pruebas'
        }
    }
}
