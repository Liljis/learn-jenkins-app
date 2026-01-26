pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'ecb1d0b9-4e64-4695-a436-2df381a7edb1'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    stages {
        stage('Deploy staging') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli node-jq
                    node_modules/.bin/netlify --version
                    echo " Deploying to Netlify ${NETLIFY_SITE_ID} "
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --json > deploy_url.json
                    cat deploy_url.json
                '''
            }
            script{
                env.STAGING_URL = sh(script: "node_modules/.bin/node-jq -r '.deploy_url' deploy_url.json", returnStdout: true)
            }
        }

        stage('Staging E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.58.0-focal'
                    reuseNode true
                }
            }

            environment {
                CI_ENVIRONMENT_URL = "${env.STAGING_URL}"
            }

            steps{
                sh '''
                    npx playwright test --reporter=html
                '''
            }

            post {
                always {
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: false,
                        reportDir: 'playwright-report',
                        reportFiles: 'index.html',
                        reportName: 'Staging E2E Test Report',
                        useWrapperFileDirectory: true
                    ])
                }
            }
        }


        stage('Approval'){
            steps {
                timeout(time:15, unit:'MINUTES') {
                    input message: 'Approve Deployment to Production?', ok: 'Deploy'
                }
            }
        }

        stage('Deploy prod') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo " Deploying to Netlify ${NETLIFY_SITE_ID} "
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }

        stage('Prod E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.58.0-focal'
                    reuseNode true
                }
            }

            environment {
                CI_ENVIRONMENT_URL = 'https://tubular-rugelach-ec3a1b.netlify.app/'
            }

            steps{
                sh '''
                    npx playwright test --reporter=html
                '''
            }

            post {
                always {
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: false,
                        reportDir: 'playwright-report',
                        reportFiles: 'index.html',
                        reportName: 'Prod E2E Test Report'
                    ])
                }
            }
        }
    }
}