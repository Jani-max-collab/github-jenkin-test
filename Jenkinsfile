pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Repository connected successfully'
            }
        }

        stage('Build Angular') {
            steps {
                bat '''
                npm install
                npx ng build
                '''
            }
        }

    }
}