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
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    docker run --rm \
                    -e SONAR_HOST_URL=http://host.docker.internal:9000 \
                    -e SONAR_TOKEN=$SONAR_TOKEN \
                    -v "$PWD:/usr/src" \
                    -w /usr/src \
                    sonarsource/sonar-scanner-cli
                    '''
                }
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
