pipeline {
    agent any
    tools {
        sonarQube 'SonarQubeScanner' // Nombre configurado en Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/EduardoUT10/crud-backend.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'sonar-scanner -Dsonar.projectKey=https:crud-backend -Dsonar.sources=src -Dsonar.host.url=http://localhost:9000'
                }
            }
        }
    }
}
