pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Select environment')
    }
    stages {
        stage('build with params') {
            steps {
                echo "You selected environment: ${params.ENV}"
            }
        }
    }
}
