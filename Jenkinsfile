pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git 'https://github.com/huysong/gitnetcore.git'
            }
        }
        stage('Restore package'){
            steps{
                echo 'Restore package'
                bat 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                echo 'build project netcore'
                bat 'dotnet build  --configuration Release'
            }
        }
        stage('Test') {
            steps {
                echo 'running test...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
        stage('Publish to folder') {
            steps {
                echo 'publishing'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }
        stage ('Publish') {
	        steps {
		        echo 'public to running folder'
		        // iisreset /stop // dừng IIS nếu cần
		        bat 'xcopy "%WORKSPACE%\\publish" /E /Y /I /R "c:\\wwwroot\\myproject"'
 	        }
        }
        stage('Deploy to IIS') {
            steps {
                powershell '''
        # Tải module IIS
        Import-Module WebAdministration

        # Nếu chưa có website "MySite", thì tạo mới
        if (-not (Test-Path IIS:\\Sites\\MySite)) {
            New-Website -Name "MySite" -Port 40000 -PhysicalPath "c:\\test1-netcore"
        }
        '''
    }
}

    }
}
