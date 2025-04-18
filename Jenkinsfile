pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID = '054f2ec0-512e-4743-b678-f72b4ac7435f' // Replace with your actual subscription ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jnaditya108/python-project-jnkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Package App') {
            steps {
                bat 'zip -r app.zip .'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: 'azure-service-principal')]) {
                    script {
                        bat '''
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
