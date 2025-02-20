pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/EduardoUT10/crud-backend.git'
            }
        }
        stage('Maven Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                        sonar-scanner \
                        -Dsonar.projectKey=crud-backend \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=${SONAR_TOKEN} \
                        -Dsonar.exclusions=**/node_modules/**,**/*.spec.ts \
                        -Dsonar.sourceEncoding=UTF-8
                    """
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
