pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build completado"'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t mi-app:latest .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm mi-app:latest nginx -t'
            }
        }

        stage('Static Analysis - SonarQube') {
            steps {
                sh '''
                echo "Ejecutando análisis estático"
                if command -v sonar-scanner >/dev/null 2>&1; then
                sonar-scanner
                else
                echo "sonar-scanner no está instalado en el agente Jenkins"
                echo "Configuración disponible en sonar-project.properties"
                fi
                '''
            }
        }

        stage('Container Security Scan - Trivy') {
            steps {
                sh '''
                docker run --rm \
                -v /var/run/docker.sock:/var/run/docker.sock \
                aquasec/trivy:latest image \
                --severity CRITICAL \
                --exit-code 1 \
                --no-progress \
                mi-app:latest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f mi-app || true'
                sh 'docker run -d -p 80:80 --name mi-app mi-app:latest'
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado correctamente.'
        }
        failure {
            echo 'El pipeline falló.'
        }
        always {
            echo 'Fin de ejecución.'
        }
    }
}
