pipeline {
    agent any

    environment {
        PUBLISH_DIR = "${WORKSPACE}\\publish"
        DEPLOY_DIR = "C:\\wwwroot\\myproject"
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git url: 'https://github.com/huysong/gitnetcore.git', branch: 'main'
            }
        }

        stage('Restore package') {
            steps {
                echo 'Restoring NuGet packages...'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project (Release)...'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish to folder') {
            steps {
                echo 'Publishing application to folder...'
                bat "dotnet publish -c Release -o \"${env.PUBLISH_DIR}\""
            }
        }

        stage('Copy to deployment folder') {
            steps {
                echo 'Copying published files to deploy directory...'
                // Đảm bảo thư mục đích tồn tại
                bat "if not exist \"${env.DEPLOY_DIR}\" mkdir \"${env.DEPLOY_DIR}\""
                bat "xcopy \"${env.PUBLISH_DIR}\" \"${env.DEPLOY_DIR}\" /E /Y /I /R"
            }
        }

        stage('Deploy to IIS') {
            steps {
                echo 'Deploying to IIS...'
                powershell '''
                    Import-Module WebAdministration

                    $siteName = "MySite"
                    $sitePath = "C:\\wwwroot\\myproject"
                    $sitePort = 40000

                    if (-not (Test-Path "IIS:\\Sites\\$siteName")) {
                        New-Website -Name $siteName -Port $sitePort -PhysicalPath $sitePath -Force
                    }
                    else {
                        Write-Host "Website '$siteName' already exists."
                    }
                '''
            }
        }
    }
}
