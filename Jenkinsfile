pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git branch: 'main', url: 'https://github.com/night-ECHO/yo_mama.git'
            }
        }

        stage('Restore Packages') {
            steps {
                echo 'Restoring packages...'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish to Folder') {
            steps {
                echo 'Publishing to ./publish folder...'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }

        stage('Copy to Running Folder') {
            steps {
                echo 'Copying to IIS root (C:\\wwwroot\\myproject)...'
                // Nếu cần stop IIS trước thì uncomment dòng dưới
                // bat 'iisreset /stop'
                bat 'xcopy "%WORKSPACE%\\publish" "C:\\wwwroot\\myproject" /E /Y /I /R'
            }
        }

        stage('Deploy to IIS') {
            steps {
                echo 'Deploying to IIS...'
                powershell '''
                    Import-Module WebAdministration

                    if (-not (Test-Path IIS:\\Sites\\MySite)) {
                        New-Website -Name "MySite" -Port 81 -PhysicalPath "C:\\test1-netcore" -ApplicationPool ".NET v4.5"
                    } else {
                        Write-Output "IIS website 'MySite' already exists."
                    }

                    Start-Website -Name "MySite"
                '''
            }
        }

    } // end stages
}
