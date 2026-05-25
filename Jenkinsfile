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

        REM Create backup folder if not exists
        "C:\\Program Files\\PuTTY\\plink.exe" -batch -pw %PASS% %USER%@%SERVER% ^
        "if not exist C:\\jenkin\\backup mkdir C:\\jenkin\\backup"

        REM Move old WAR to backup with timestamp
        "C:\\Program Files\\PuTTY\\plink.exe" -batch -pw %PASS% %USER%@%SERVER% ^
        "if exist C:\\jenkin\\angularjenkin.war powershell -command ^
        \"$ts=Get-Date -Format 'yyyyMMdd_HHmmss'; ^
        Move-Item 'C:\\jenkin\\angularjenkin.war' ^
        'C:\\jenkin\\backup\\angularjenkin_'+$ts+'.war'\""

        REM Copy new WAR
        "C:\\Program Files\\PuTTY\\pscp.exe" -batch -pw %PASS% ^
        dist\\github-jenkin-test\\browser\\angularjenkin.war ^
        %USER%@%SERVER%:C:/jenkin/

        '''
    }
}
    }
}