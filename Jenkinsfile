pipeline {
    agent any
    stages {
        stage('Build-without-docker') {
            steps {
                sh '''
                    echo 'Building without docker based agent pipeline ...'
                    ls -al
                    touch container-no.txt
                '''
            }
        }
        stage('Build-with-docker') {
            agent {
                docker {
                    image 'node:18-alpine' // Use a lightweight Node.js image
                    args '-u root' // Run as root user
                    reuseNode true // Reuse the current node,used to allow a shared workspace btw stages
                }
            }
            steps {
                sh '''
                    echo 'Building with docker based agent pipeline ...'
                    ls -al
                    touch container-yes.txt
                '''
            }
        }
    }
}