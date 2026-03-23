pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID = 'd9e25859-f02c-40e8-8f9d-a27696ab41cc'
        AZURE_TENANT_ID       = '60a402fe-9582-4148-9b8d-590ff2eb8a99'
        RESOURCE_GROUP        = 'jenkins-get-started-rg'
        WEBAPP_NAME           = 'jenkins-node-app-mani'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Azure Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'AzureServicePrincipal',
                    usernameVariable: 'AZURE_CLIENT_ID',
                    passwordVariable: 'AZURE_CLIENT_SECRET'
                )]) {
                    sh '''
                        az login --service-principal \
                          --username $AZURE_CLIENT_ID \
                          --password $AZURE_CLIENT_SECRET \
                          --tenant $AZURE_TENANT_ID
                    '''
                }
            }
        }

        stage('Deploy to Azure') {
            steps {
                sh '''
                    zip -r app.zip .
                    az webapp deploy \
                      --resource-group $RESOURCE_GROUP \
                      --name $WEBAPP_NAME \
                      --src-path app.zip \
                      --type zip
                '''
            }
        }
    }
}
