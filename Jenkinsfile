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
                    image 'node:18-alpine' // Use a lightweight Node.js image
                    reuseNode true // Reuse the current node,used to allow a shared workspace btw stages
                }
            }
            steps {
                echo 'Running tests...'
                sh '''
                    test -f build/test-results.xml || echo "No test results found"
                    npm test
                    ls -al
                '''
            }
        }
    }

    post {
        always {
            // archiveArtifacts artifacts: 'build/test-results.xml', fingerprint: true
            junit 'build/test-results.xml'
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}