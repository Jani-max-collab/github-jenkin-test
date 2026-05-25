pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Repository connected successfully'
            }
        }

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

    }
}