pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID = 'your-subscription-id' // Replace with your actual subscription ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jnaditya108/python-project-jnkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Package App') {
            steps {
                sh 'zip -r app.zip .'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: 'azure-service-principal')]) {
                    script {
                        sh '''
                            az login --service-principal \
                                --username "$AZURE_CLIENT_ID" \
                                --password "$AZURE_CLIENT_SECRET" \
                                --tenant "$AZURE_TENANT_ID"

                            az account set --subscription "$AZURE_SUBSCRIPTION_ID"

                            az webapp up \
                                --name myPythonApp \
                                --resource-group myResourceGroup \
                                --runtime "PYTHON:3.9" \
                                --src-path . \
                                --location "East US"
                        '''
                    }
                }
            }
        }
    }
}
