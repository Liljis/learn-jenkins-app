pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'f7e85c05-8a82-42bc-9a85-35036cdfa34f'
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
                    echo "install netlify cli"
                    npm install netlify-cli
                    echo "success Netlify"
                    node_modules/.bin/netlify --version
                    echo " Deploying to Netlify"
                '''
            }
        }
    }
}