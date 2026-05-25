pipeline {
    agent any

    environment {
        SERVER = "192.168.1.200"
        USER = "administrator"

        DIST_PATH = "dist\\github-jenkin-test\\browser"
        WAR_NAME = "angularjenkin.war"

        REMOTE_DEPLOY = "C:\\jenkin"
        REMOTE_BACKUP = "C:\\jenkin\\backup"
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
                withCredentials([
                    usernamePassword(
                        credentialsId: 'sap-server-creds',
                        usernameVariable: 'SSH_USER',
                        passwordVariable: 'SSH_PASS'
                    )
                ]) {

                    bat '''
                    
                    REM Create backup folder if missing
                    ssh %SSH_USER%@%SERVER% ^
                    "if not exist %REMOTE_BACKUP% mkdir %REMOTE_BACKUP%"

                    REM Backup existing WAR if present
                    ssh %SSH_USER%@%SERVER% ^
                    "if exist %REMOTE_DEPLOY%\\%WAR_NAME% powershell ^
                    -command ^
                    \"$ts=Get-Date -Format 'yyyyMMdd_HHmmss'; ^
                    Move-Item '%REMOTE_DEPLOY%\\%WAR_NAME%' ^
                    '%REMOTE_BACKUP%\\angularjenkin_'+$ts+'.war'\""

                    REM Copy new WAR
                    scp %DIST_PATH%\\%WAR_NAME% ^
                    %SSH_USER%@%SERVER%:%REMOTE_DEPLOYY%

                    '''
                }
            }
        }
    }
}