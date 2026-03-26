pipeline {
  agent any

  environment {
    AZURE_SUBSCRIPTION_ID = 'SP2026.T81.INFO.540.Qiwei_Wang'
    AZURE_TENANT_ID = '1a2b4ffc-361d-4bd6-8575-0a07799fc378'
    RESOURCE_GROUP = 'jenkins-get-started-rg'
    WEBAPP_NAME = 'jenkins-node-app-wang'
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
