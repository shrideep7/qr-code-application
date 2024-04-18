pipeline {
    agent any
    
    environment {
        // Define AWS credentials for S3 bucket access
        AWS_ACCESS_KEY_ID = credentials('AKIAVRUVT7F4C6FY7NWV')
        AWS_SECRET_ACCESS_KEY = credentials('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')
        GITLAB_CRED_ID = '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'
        
        // Define S3 bucket name and Terraform workspace name
        // S3_BUCKET = 'your-s3-bucket-name'
        // TF_WORKSPACE = 'your-terraform-workspace'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from the repository
                script {
                    // Set global git configuration to disable SSL verification
                    sh 'git config --global http.sslVerify false'

                    // Checkout code from GitLab repository using credentials
                    checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://gitlab.cdsys.local/jenkins-training/cicd.git', credentialsId: env.GITLAB_CRED_ID]]])
                }

            }
        }

        

        stage('Terraform init') {
            steps {
                script {
                   
                    // Plan and apply Terraform changes
                    sh 'terraform init'

                }
            }
        }


        stage('Terraform plan') {
            steps {
                script {
                   
                    // Plan and apply Terraform changes
                    sh 'terraform plan -out=tfplan'

                }
            }
        }

        stage('Terraform INIT') {
            steps {
                script {
                    sh 'terraform apply -auto-approve tfplan | tee terraform_apply_output.txt'
                    // sh 'api_url=$(grep -oP "(?<=api_url = ).*" terraform_apply_output.txt)'
                    // sh 'sed -i "s|API_GATEWAY_ENDPOINT|$api_url|g" index.html'
                }
            }
        }

  

        stage('Serve HTML') {
            steps {
                // Serve HTML file using Python's SimpleHTTPServer
                sh 'nohup python -m SimpleHTTPServer 5050 > /dev/null 2>&1 &'
            }
        }