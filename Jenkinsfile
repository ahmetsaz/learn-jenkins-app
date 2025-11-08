pipeline {
    agent any
    environment {
        NPM_CONFIG_CACHE = "${WORKSPACE}/.npm"
    }
    stages {
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm cache clean --force
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage("Npm Test") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                sh '''
                    test -f build/index.html
                    npm test 
                '''

            }
        }
    }
    post{
        always{
           junit 'test-results/junit.xml'
        }
    }
}