pipeline {
    agent any

    parameters {
        string(name: 'TERRAFORM_ACTION', defaultValue: 'apply', description: 'Terraform action (e.g., apply, destroy)')
    }

    environment {
        AWS_ACCESS_KEY_ID = credentials('sylviewn_AWS_ACCESS_ID')
        AWS_SECRET_ACCESS_KEY = credentials('sylviewn_AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sylviewn/infra-jenkins.git'
            }
        }

        stage("terraform init") {
            steps {
                sh "terraform init"
            }
        }

        stage("plan") {
            steps {
                sh "terraform plan"
            }
        }

        stage("Action") {
            steps {
                  sh "terraform ${params.TERRAFORM_ACTION} --auto-approve"
            }
        }

        stage("Deploy to EKS") {
            steps {
                sh "aws eks update-kubeconfig --name eks_cluster"
                sh "kubectl delete -f deployment.yml"
            }
        }
    }
}



   
    
