pipeline {
    agent any

    parameters {
        string(name: 'AWS_DEFAULT_REGION', defaultValue: 'us-east-1', description: 'AWS Region')
    }

    stages {
        stage('CloudFormation Deploy') {
            steps {
                echo 'Deploying CloudFormation stack...'
                sh 'aws cloudformation deploy --template-file oracle-rds.yaml --stack-name oracle-rds'
            }
        }
    }
}