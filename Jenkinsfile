pipeline {
    agent any
    stages {
        stage('Build-with-docker') {
            agent {
                docker {
                    image 'node:18-alpine' // Use a lightweight Node.js image
                    reuseNode true // Reuse the current node,used to allow a shared workspace btw stages
                }
            }
            steps {
                sh '''
                    ls -al
                    npm ci
                    npm run build
                    ls -al
                '''
            }
        }
    }
}