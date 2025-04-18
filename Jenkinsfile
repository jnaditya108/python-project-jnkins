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
                bat 'py -m venv venv'                          // Create virtual environment
                bat 'venv\\Scripts\\pip install -r requirements.txt'  // Install dependencies
            }
        }

        stage('Package App') {
            steps {
                powershell 'Compress-Archive -Path * -DestinationPath app.zip -Force'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([
                    string(credentialsId: 'azure-client-id', variable: 'AZURE_CLIENT_ID'),
                    string(credentialsId: 'azure-client-secret', variable: 'AZURE_CLIENT_SECRET'),
                    string(credentialsId: 'azure-tenant-id', variable: 'AZURE_TENANT_ID')
                ]) {
                    script {
                        bat """
                        az login --service-principal ^
                            --username "%AZURE_CLIENT_ID%" ^
                            --password "%AZURE_CLIENT_SECRET%" ^
                            --tenant "%AZURE_TENANT_ID%"

                        az account set --subscription "%AZURE_SUBSCRIPTION_ID%"

                        az webapp up ^
                            --name myPythonApp ^
                            --resource-group myResourceGroup ^
                            --runtime "PYTHON:3.9" ^
                            --src-path . ^
                            --location "East US"
                        """
                    }
                }
            }
        }
    }
}
