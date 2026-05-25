pipeline {
    agent any

    environment {
        SERVER = "192.168.1.200"
        USER = "administrator"
        PASS = '$uravel@23'

        DIST_PATH = "dist\\github-jenkin-test\\browser"
        WAR_NAME = "angularjenkin.war"

        REMOTE_DEPLOY = "C:\\jenkin"
        REMOTE_BACKUP = "C:\\jenkin\\backup"

        PLINK = "\"C:\\Program Files\\PuTTY\\plink.exe\""
        PSCP = "\"C:\\Program Files\\PuTTY\\pscp.exe\""
    }

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Angular') {
            steps {
                bat 'npx ng build'
            }
        }

        stage('Create WAR') {
            steps {
                bat '''
                cd %DIST_PATH%
                jar -cvf %WAR_NAME% -C . .
                '''
            }
        }

        stage('Deploy') {
    steps {
        bat '''

        REM Accept host key automatically
        "C:\\Program Files\\PuTTY\\plink.exe" -batch -pw %PASS% -hostkey "ssh-ed25519 255 SHA256:woK2kE6RthRGPKrjpt89/7mezMLugrHf4baqv425cLY" %USER%@%SERVER% ^
        "mkdir C:\\jenkin\\backup 2>nul"

        REM Backup old WAR
"C:\\Program Files\\PuTTY\\plink.exe" -batch -pw %PASS% -hostkey "ssh-ed25519 255 SHA256:woK2kE6RthRGPKrjpt89/7mezMLugrHf4baqv425cLY" %USER%@%SERVER% ^
"powershell -ExecutionPolicy Bypass -Command ""$d=Get-Date -Format 'yyyyMMdd_HHmmss'; if(Test-Path 'C:\\jenkin\\angularjenkin.war'){ Move-Item 'C:\\jenkin\\angularjenkin.war' ('C:\\jenkin\\backup\\angularjenkin_'+$d+'.war') }"""

        REM Upload new WAR
        "C:\\Program Files\\PuTTY\\pscp.exe" -batch -pw %PASS% -hostkey "ssh-ed25519 255 SHA256:woK2kE6RthRGPKrjpt89/7mezMLugrHf4baqv425cLY" ^
        dist\\github-jenkin-test\\browser\\angularjenkin.war ^
        %USER%@%SERVER%:C:/jenkin/

        '''
    }
}
    }
}