pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/tirthmodi2904/hrms-devops-demo.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('sonarserver') {
                        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                            sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=hrms-devops-demo \
                            -Dsonar.sources=. \
                            -Dsonar.login=$SONAR_TOKEN
                            """
                        }
                    }
                }
            }
        }

        stage('SonarQube Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Artifact') {
            steps {
                sh 'zip -r hrms-build.zip *.html assets || true'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexuslogin',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh """
                    curl -v -u $NEXUS_USER:$NEXUS_PASS \
                    --upload-file hrms-build.zip \
                    http://172.31.16.65:8081/repository/hrms-repo/hrms-build.zip
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'HRMS CI + Nexus Upload SUCCESS!'
        }
        failure {
            echo 'Pipeline FAILED!'
        }
    }
}
