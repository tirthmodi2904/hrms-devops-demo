pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/tirthmodi2904/hrms-devops-demo.git'
            }
        }

        stage('Build Website') {
            steps {
                echo 'Building HRMS Demo Website...'
            }
        }

        stage('Create Artifact') {
            steps {
                sh 'zip -r hrms-build.zip *'
            }
        }

    }
}
