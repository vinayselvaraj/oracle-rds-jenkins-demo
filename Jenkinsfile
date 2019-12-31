pipeline {
    agent any

    parameters {
        string(name: 'AWS_DEFAULT_REGION', defaultValue: 'us-east-1', description: 'AWS Region')
        string(name: 'VPC_ID', description: 'VPC ID')
        string(name: 'SUBNET_IDS', description: 'Subnet IDs')
    }

    stages {
        stage('Validate') {
            steps {
                echo 'Validating CloudFormation template...'
                sh "aws cloudformation validate-template --template-body file://oracle-rds.yaml"
            }
        }
        stage('CloudFormation Deploy') {
            steps {
                echo 'Deploying CloudFormation stack...'
                sh "aws cloudformation deploy --template-file oracle-rds.yaml --stack-name oracle-rds  --parameter-overrides VpcId=${VPC_ID} DBSubnets=${SUBNET_IDS} --no-fail-on-empty-changeset"
            }
        }
    }
}