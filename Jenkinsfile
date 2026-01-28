pipeline {
    agent any

    stages {

      stage('Debug workspace') {
         steps {
        sh '''
          echo "=== Workspace ==="
          pwd
          ls -la
        '''
         }
     }

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
                JENKINS_CONTAINER_ID=$(cat /proc/self/cgroup | grep docker | head -1 | cut -d/ -f3)

                docker run --rm \
                  --volumes-from $JENKINS_CONTAINER_ID \
                  -w /var/jenkins_home/workspace/playrightNuevo \
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
