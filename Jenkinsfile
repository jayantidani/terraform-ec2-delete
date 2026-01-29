pipeline {
    agent any

    parameters {
        string(name: 'INSTANCE_ID', defaultValue: '', description: 'Enter EC2 Instance ID to delete')
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
                sh '''
                echo "Checking Terraform state..."
                if terraform state list | grep -q aws_instance.target; then
                    echo "Instance already imported. Skipping import."
                else
                    echo "Importing instance..."
                    terraform import aws_instance.target ${INSTANCE_ID}
                fi
                '''
            }
        }

        stage('Plan Destroy') {
            steps {
                sh 'terraform plan -destroy'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: "Do you really want to DELETE EC2 instance ${INSTANCE_ID}?"
            }
        }

        stage('Destroy Instance') {
            steps {
                sh 'terraform destroy -auto-approve'
            }
        }
    }
}
