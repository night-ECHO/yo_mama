pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/your-username/yo_mama.git'
            }
        }

        stage('Restore Packages') {
            steps {
                echo 'Restoring packages...'
                bat 'dotnet restore WebApplication1/WebApplication1.csproj'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                bat 'dotnet build WebApplication1/WebApplication1.csproj --configuration Release'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test WebApplication1/WebApplication1.csproj --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing project...'
                bat 'dotnet publish WebApplication1/WebApplication1.csproj --configuration Release --output ./publish'
            }
        }
    }

    post {
        success {
            echo '✅ Build and publish succeeded!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
