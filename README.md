# Pipeline CI/CD con Jenkins, Docker, SonarQube y Trivy

## *Santiago Morales*

## Descripción

Este proyecto implementa una aplicación web estática desplegada en un contenedor Docker con Nginx. El repositorio incluye un pipeline en Jenkins encargado de automatizar la construcción, validación, análisis de calidad, escaneo de seguridad y despliegue local de la aplicación.

## Objetivo

Diseñar y construir un flujo básico de integración y despliegue continuo que permita validar una aplicación antes de su publicación, incorporando herramientas de calidad y seguridad dentro del proceso.

## Estructura del proyecto

```text
.
├── .dockerignore
├── .gitignore
├── Dockerfile
├── Jenkinsfile
├── README.md
├── index.html
└── sonar-project.properties

## Ejecución local

Construir la imagen Docker:

`docker build -t mi-app:latest .`

Ejecutar el contenedor:

`docker run -d --name mi-app -p 80:80 mi-app:latest`

Verificar que el contenedor esté activo:

`docker ps`

Abrir la aplicación en el navegador:

`http://localhost`

Detener y eliminar el contenedor:

`docker rm -f mi-app`

## Servicios requeridos

Ejecutar Jenkins:

`docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts`

Ejecutar SonarQube:

`docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community`

Instalar Trivy en macOS:

`brew install trivy`