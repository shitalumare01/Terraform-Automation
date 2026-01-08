pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['plan', 'apply'],
            description: 'Select the Terraform action to perform'
        )
        string(
            name: 'BRANCH',
            defaultValue: 'main',
            description: 'Enter Git branch name to checkout'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Checking out branch: ${params.BRANCH}"
                }
                checkout scmGit(
                    branches: [[name: "*/${params.BRANCH}"]],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/shitalumare01/Terraform-Automation.git']]
                )
            }
        }

        stage('Terraform Init') {
            steps {
                sh "terraform init -reconfigure"
            }
        }

        stage('Terraform Action') {
            steps {
                script {
                    if (params.ACTION == 'plan') {
                        echo 'Executing Terraform Plan...'
                        sh "terraform plan"
                    } else if (params.ACTION == 'apply') {
                        echo 'Executing Terraform Apply...'
                        sh "terraform apply --auto-approve"
                    } else {
                        error 'Invalid ACTION selected'
                    }
                }
            }
        }
    }
}
