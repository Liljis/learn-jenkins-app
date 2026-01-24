pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '36ce618f-0aa8-42cc-bd6c-8740ee0058b8'
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
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo " Deploying to Netlify ${NETLIFY_SITE_ID} "
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build 
                '''
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
    }
}