pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    npm --version
                    npm install
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
            steps{
                sh '''
                    echo "Test stage"
                    [ -f build/index.html ] && echo "File exists" || echo "File does not exist"
                    npm test
                '''
            }
        }
    }
}