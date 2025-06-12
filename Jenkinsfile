pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/huysong/gitnetcore.git'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build'
            }
        }
        stage('Test') {
            steps {
                sh 'dotnet test'
            }
        }
    }
}
