#Set Up Terraform in your Jenkins Docker Container
#modified webhook
#Step 2: Install Terraform on your Docker Container
#find your Jenkins container ID or name and log into the container as root user:
# **docker exec -u root -it <container-id> /bin/bash**
# Change Directory to /usr/bin directory
# If sudo and wget above are not found, they can be installed by running the below commands:
 #apt-get install sudo
 #apt-get install wget
 install terraform in the docker container:
 wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
#pipeline


pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kumararunlal/Jenkins_terra_cicd.git']])
            }
        }
    stage('Terraform Version') {
            steps {
                sh 'terraform --version'
            }
        }
    stage('Terraform Initialization') {
            steps {
                sh 'terraform init'
            }
        }
    stage('Terraform Plan') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: 'IAC_AZURE_SECRET',
                                    subscriptionIdVariable: 'SUBS_ID',
                                    clientIdVariable: 'CLIENT_ID',
                                    clientSecretVariable: 'CLIENT_SECRET',
                                    tenantIdVariable: 'TENANT_ID')]) 
                {
                    sh "terraform plan -var 'subscription_id=$SUBS_ID' -var 'tenant_id=$TENANT_ID' -var 'client_id=$CLIENT_ID' -var 'client_secret=$CLIENT_SECRET'"
                }
                
            }
        }
    }
}
