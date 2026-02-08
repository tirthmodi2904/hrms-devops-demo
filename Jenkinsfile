pipeline {
    agent any

    tools {
        sonarScanner 'SonarScanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/tirthmodi2904/hrms-devops-demo.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=hrms-devops-demo -Dsonar.sources=.'
                }
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
