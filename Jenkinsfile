pipeline {
    agent any // Use any available agent for the pipeline

    stages {
        /*
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
        */
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

        stage('E2E Tests') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy' // Use Playwright Docker image for E2E tests 
                    reuseNode true // Reuse the same node for this stage
                }
            }

            steps {
                sh '''
                    npm install serve                       # Install serve globally
                    node_modules/.bin/serve -s build &     # Serve the build directory
                    sleep 10
                    npx playwright test --reporter=html # Run Playwright tests with HTML reporter
                    
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml' // Publish test results (commented out)
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}
