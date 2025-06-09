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
                    npm test # Run unit tests using npm
                '''
            }
        }

        stage('E2E Tests') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy' // Use Playwright Docker image for end-to-end tests
                    reuseNode true // Reuse the same node for this stage
                }
            }

            steps {
                sh '''
                    npm install serve                       # Install serve locally to serve the build directory
                    node_modules/.bin/serve -s build &     # Start serving the build directory in the background
                    sleep 10                               # Wait for the server to start
                    npx playwright test --reporter=html    # Run Playwright end-to-end tests with HTML reporter
                '''
            }
        }
    }

    post {
        always {
            // Publish test results and HTML reports after the pipeline execution
            junit 'jest-results/junit.xml' // Publish JUnit test results
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}
