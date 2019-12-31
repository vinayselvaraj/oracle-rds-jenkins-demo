pipeline {
    agent any

    parameters {
        string(name: 'AWS_DEFAULT_REGION', defaultValue: 'us-east-1', description: 'AWS Region')
        string(name: 'VPC_ID', description: 'VPC ID')
        string(name: 'SUBNET_IDS', description: 'Subnet IDs')
        string(name: 'RDS_IAM_ROLE', description: 'RDS IAM Role ARN')
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
                sh "aws cloudformation deploy --template-file oracle-rds.yaml --stack-name oracle-rds-demo  --parameter-overrides VpcId=${VPC_ID} DBSubnets=${SUBNET_IDS} RDSS3IAMRoleArn=${RDS_IAM_ROLE} --no-fail-on-empty-changeset"
            }
        }
        stage('Database Deploy') {
            steps {
                echo 'Deploying Database related code and scripts...'
            }
        }
    }
}