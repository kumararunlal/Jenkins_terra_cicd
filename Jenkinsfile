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
