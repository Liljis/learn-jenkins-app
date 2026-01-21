pipeline {
    agent any
    stages {
        /*
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
        */
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    echo "Test stage"
                    # [ -f build/index.html ] && echo "File exists" || echo "File does not exist"
                    # sh 'test -f build/index.html'

                    npm test 
                '''
            }
        }

        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
                    reuseNode true
                    // args '-u root:root' -  this will alter the jenkins user to root. which mess with the workspace.
                    // create serve locally
                    // run $npm install serve
                    // copy path from node_modules/.bin/serve
                }
            }
            steps{
                sh '''
                    echo "started serve"
                    npm install -g serve
                    node_modules/.bin/serve -s build &
                    sleep 1
                    npx playwright test
                '''
            }
        }
    }
    
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}