pipeline {
    agent any // Use any available agent for the pipeline

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine' // Use Node.js 18 Alpine Docker image
                    reuseNode true // Reuse the same node for this stage
                }
            }
            steps {
                sh '''
                    ls -la # List files in the current directory
                    node --version # Check Node.js version
                    npm --version # Check npm version
                    npm ci # Install dependencies using clean install
                    npm run build # Build the project
                    ls -la # List files after build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine' // Use Node.js 18 Alpine Docker image
                    reuseNode true // Reuse the same node for this stage
                }
            }

            steps {
                sh '''
                    test -f build/index.html # Verify build output exists
                    npm test # Run tests
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml' // Publish test results (commented out)
        }
    }
}
