pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "night-echo/webapp:latest" // đổi tên theo project của bạn
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/night-ECHO/yo_mama.git' // hoặc repo mới nếu đã đổi
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o out'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}
