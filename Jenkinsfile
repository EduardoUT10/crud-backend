pipeline {
    agent any
    tools {
        maven 'Maven' // Nombre de la instalación de Maven configurado en Jenkins
        sonarQube 'SonarQubeScanner' // Nombre configurado en Jenkins para SonarQube
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/EduardoUT10/crud-backend.git'
            }
        }
        stage('Maven Build') {
            steps {
                // Aquí usamos 'bat' para ejecutar comandos Maven en un entorno Windows
                bat 'mvn clean install -DskipTests' // 'DskipTests' para evitar ejecutar las pruebas (si lo deseas)
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Asegúrate de que 'SonarQube' coincida con tu configuración en Jenkins
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
   
