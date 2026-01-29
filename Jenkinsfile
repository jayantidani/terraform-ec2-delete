pipeline {
    agent any

    parameters {
        string(name: 'INSTANCE_ID', description: 'Enter EC2 Instance ID to delete')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/jayantidani/terraform-ec2-delete.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Import Instance') {
            steps {
                sh """
                terraform import aws_instance.target ${INSTANCE_ID}
                """
            }
        }

        stage('Plan Destroy') {
            steps {
                sh 'terraform plan -destroy -target=aws_instance.target'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: "⚠️ Do you really want to delete EC2 instance ${INSTANCE_ID}?"
            }
        }

        stage('Destroy Instance') {
            steps {
                sh 'terraform destroy -target=aws_instance.target -auto-approve'
            }
        }
    }
}
