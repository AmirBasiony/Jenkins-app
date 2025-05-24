pipeline {
    agent any
    stages {
        stage('Build') {
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
        stage ('Test') {
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

    // post {
    //     always {
    //         // archiveArtifacts artifacts: 'build/test-results.xml', fingerprint: true
    //         junit 'build/test-results.xml'
    //     }
    // }
}