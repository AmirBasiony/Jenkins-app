pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Select environment')
    }
    stages {
        stage('build with params') {
            steps {
                echo "You selected environment: ${params.ENV}"
                sh '''
                    echo "Building for environment: ${params.ENV}"
                    # Simulate build process
                    echo "Build completed for ${params.ENV} environment"
                '''
            }
        }
    }
}
