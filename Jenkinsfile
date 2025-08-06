pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git branch: 'main', url: 'https://github.com/huudqtmu/projectnet.git'
            }
        }

        stage('Restore Packages') {
            steps {
                echo 'Restoring packages'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project (Release mode)'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish to folder') {
            steps {
                echo 'Publishing to ./publish directory'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }

        stage('Copy to running folder') {
            steps {
                echo 'Copying published files to C:\\wwwroot\\myproject'
                bat 'xcopy "%WORKSPACE%\\publish" /E /Y /I /R "C:\\wwwroot\\myproject"'
            }
        }

        stage('Deploy to IIS') {
            steps {
                echo 'Deploying to IIS'
                powershell '''
                    Import-Module WebAdministration
                    if (-not (Test-Path IIS:\\Sites\\MySite)) {
                        New-Website -Name "MySite" -Port 81 -PhysicalPath "C:\\test1-netcore"
                    }
                '''
            }
        }
    }
}
