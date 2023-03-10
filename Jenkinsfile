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
                sh 'cfn-lint $(pwd)/pipeline/cloudformation/*.yml'
            }
        }
        stage('Run SAST scan') {
            steps {
                echo "Running SAST scan..."
                sh 'env | grep -E "JENKINS_HOME|BUILD_ID|GIT_BRANCH|GIT_COMMIT" > /tmp/env'
                sh 'docker pull registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
                sh 'docker run --rm --env-file /tmp/env --mount type=bind,source=$PWD,target=/scan registry.fortidevsec.forticloud.com/fdevsec_sast:latest'
            }
        }
        stage('Test Cloudformation Stack Build') {
            when {
                expression { false }
            }
            steps {
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
