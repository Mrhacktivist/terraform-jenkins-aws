pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')  // Jenkins will use this environment variable for AWS credentials
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Pull the Terraform code from GitHub repository
                git 'https://github.com/your-username/terraform-jenkins-aws.git'
            }
        }

        stage('Install Terraform') {
            steps {
                // Check if Terraform is installed and install it if not
                sh '''
                if ! command -v terraform &> /dev/null; then
                    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
                    sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
                    sudo apt update && sudo apt install terraform -y
                fi
                terraform --version
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                // Generate Terraform plan
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                // Apply Terraform plan to provision EC2 instance
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }
}
