pipeline {
    agent {
        docker { 
            // Using 8.0 LTS for maximum compatibility with the Azure sample app
            image 'mcr.microsoft.com/dotnet/sdk:10.0'
            
            // -u root: Grants file write permissions for the workspace
            // -e HOME=/tmp -e DOTNET_CLI_HOME=/tmp: Fixes the Jenkins SDK pathing bug
            args '-u root -e HOME=/tmp -e DOTNET_CLI_HOME=/tmp'    
        }
    }
    stages {
        stage('Checkout') {
            steps {
                // Pulls the code from your Git repository
                checkout scm
            }
        }
        stage('Restore Dependencies') {
            steps {
                // Downloads all required NuGet packages
                sh 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                // Compiles the application in Release mode. 
                // --no-restore saves time since we just did it.
                sh 'dotnet build --configuration Release --no-restore'
            }
        }
        stage('Test') {
            steps {
                // Runs unit tests. It will pass cleanly if no tests exist.
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
        stage('Publish') {
            steps {
                // Packages the application and its dependencies into a folder named 'publish'
                sh 'dotnet publish --configuration Release --no-build --output ./publish'
            }
        }
    }
    post {
        always {
            echo "This is Jenkinsfile"
        }
    }
}
