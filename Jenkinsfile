pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'f7e85c05-8a82-42bc-9a85-35036cdfa34f'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    stages {

        stage('Deploy') {
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