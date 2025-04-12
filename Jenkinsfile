pipeline {
    agent any

    parameters {
        choice(
            name: 'action',
            choices: ['apply', 'destroy'],
            description: 'Choose whether to apply or destroy the infrastructure'
        )
    }

    environment {
        AWS_ACCESS_KEY_ID = credentials('lil_AWS_Access_key_ID')
        AWS_SECRET_ACCESS_KEY = credentials('lil_AWS_Secret_access_key')
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/lily4499/infra-jenkins.git'
            }
        }

        stage("Terraform Init") {
            steps {
                sh "terraform init"
            }
        }

        stage("Terraform Plan") {
            steps {
                sh "terraform plan"
            }
        }

        stage("Terraform ${params.action}") {
            steps {
                sh "terraform ${params.action} --auto-approve"
            }
        }

        stage("Deploy to EKS") {
            when {
                expression { params.action == 'apply' }
            }
            steps {
                sh "aws eks update-kubeconfig --name eks_cluster"
                sh "kubectl apply -f deployment.yml"
            }
        }
    }
}
