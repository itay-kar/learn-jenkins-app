pipeline {
    agent any

    enviroment{
    NETLIFY_SITE_ID = '48976588-0329-42c5-872d-0f8ebe81a1e2'
    NETLIFY_AUTH_TOKEN = credentials('NetlifyToken')
    }
    
    stages{
        stage('Build'){
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }

        stage('Test'){
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                test -f build/index.html
                npm test 
                '''
            }
        }

        stage('Deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to prudction. site ID: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify status
                '''
            }
        }
    }

    post {
        always{
            junit 'test-results/junit.xml'
    }
}

}