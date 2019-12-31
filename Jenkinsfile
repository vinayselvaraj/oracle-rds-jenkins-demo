pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:$PATH"
    }

    parameters {
        string(name: 'AWS_DEFAULT_REGION', defaultValue: 'us-east-1', description: 'AWS Region')
        string(name: 'VPC_ID', description: 'VPC ID')
        string(name: 'SUBNET_IDS', description: 'Subnet IDs')
        string(name: 'RDS_IAM_ROLE', description: 'RDS IAM Role ARN')
    }

    stages {
        stage('CloudFormation Deploy') {
            steps {
                echo 'Deploying CloudFormation stack...'
                sh "aws cloudformation deploy --template-file oracle-rds.yaml --stack-name oracle-rds-demo  --parameter-overrides VpcId=${VPC_ID} DBSubnets=${SUBNET_IDS} RDSS3IAMRoleArn=${RDS_IAM_ROLE} --no-fail-on-empty-changeset"
            }
        }
        stage('Database Deploy') {
            steps {
                echo 'Deploying Database related code and scripts...'

                script {
                    def DB_HOSTNAME = sh (
                        script: "aws cloudformation describe-stacks --stack-name oracle-rds-demo --query \"Stacks[0].Outputs[?OutputKey=='RDSInstanceEndpointAddress'].OutputValue\" --output text",
                        returnStdout: true
                    ).trim()

                    def DB_PORT = sh (
                        script: "aws cloudformation describe-stacks --stack-name oracle-rds-demo --query \"Stacks[0].Outputs[?OutputKey=='RDSInstanceEndpointPort'].OutputValue\" --output text",
                        returnStdout: true
                    ).trim()

                    def SECRET_ARN = sh (
                        script: "aws cloudformation describe-stacks --stack-name oracle-rds-demo --query \"Stacks[0].Outputs[?OutputKey=='DBSecretARN'].OutputValue\" --output text",
                        returnStdout: true
                    ).trim()

                    def DB_PASSWORD = sh (
                        script: "aws secretsmanager get-secret-value --secret-id ${SECRET_ARN} --query \"SecretString\" --output text",
                        returnStdout: true
                    ).trim()

                    echo "${DB_HOSTNAME}:${DB_PORT}"
                    echo "admin / ${DB_PASSWORD}"
                }
            }
        }
    }
}