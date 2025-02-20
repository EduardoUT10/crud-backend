pipeline {
    agent any
    tools {
        // Puedes configurar un instalador de Gradle en Jenkins y asignarlo aquí si tienes uno configurado
        gradle 'Gradle' // Nombre de la instalación de Gradle configurada en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/EduardoUT10/crud-backend.git'
            }
        }
        stage('Gradle Build') {
            steps {
                // Ejecuta el build de Gradle, similar a mvn clean install en Maven
                script {
                    // Si tienes tareas específicas de Gradle puedes incluirlas, por ejemplo "build"
                    sh './gradlew clean build -x test' // Ejecuta el build sin tests, ajusta si necesitas ejecutar pruebas
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeScanner') { // Asegúrate de que 'SonarQube' coincida con tu configuración en Jenkins
                    // Ejecuta el análisis de SonarQube
                    sh './gradlew sonarqube -Dsonar.projectKey=crud-backend -Dsonar.host.url=http://localhost:9000 -Dsonar.login=${SONAR_TOKEN}'
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
