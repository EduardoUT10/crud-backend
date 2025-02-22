pipeline {
    agent any
    tools {
        maven 'Maven' // El nombre debe coincidir con el nombre configurado en Jenkins
    }     
    stages {
        stage('1.Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/EduardoUT10/crud-backend.git'
            }
        }
        stage('2.Maven Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }
        
        stage('3.SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat """
                        sonar-scanner \
                        -Dsonar.projectKey=crud-backend \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=sqa_87c65bcae78ec84ccc2ed0a362f66ae0b7a0438a \
                        -Dsonar.exclusions=**/node_modules/**,**/*.spec.ts \
                        -Dsonar.sourceEncoding=UTF-8
                    """
                }
            }
        }
        stage("4.Quality Gate") {
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
