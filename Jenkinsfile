pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
    }
    stages {
        stage('Run linter') {
            steps {
                echo "Running linter..." 
            }
        }
        stage('Run SAST scan') {
            steps {
                echo "Running SAST scan..."
            }
        }
        stage('Test Cloudformation Stack Build') {
            steps {
                sh 'ls -la .'
                sh 'ls -la pipeline/cloudformation'
                sh '/usr/bin/aws cloudformation create-stack --stack-name workshop-app-test --template-body file://$(pwd)/pipeline/cloudformation/ecs-app-template.yml --parameters file://$(pwd)/pipeline/cloudformation/ecs-app-params.json --capabilities CAPABILITY_NAMED_IAM --region us-east-1'
            }
        }
        stage('Run DAST scan') {
            steps {
                echo "Running DAST scan..."
            }
        }

    }
}
