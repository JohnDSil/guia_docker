# P9. Integración y despliegue continuo (CI/CD) con Jenkins y Docker

## 9.1 Conceptos importantes
- **Integración Continua (CI):** Práctica en la que cada cambio en el código fuente activa automáticamente la compilación, pruebas y generación de la imagen Docker.
- **Despliegue Continuo (CD):** Proceso automatizado que toma la imagen generada y probada y la despliega en el servidor de producción.
- **Jenkins en Contenedor:** Servidor de automatización que puede ejecutarse dentro de Docker y compilar contenedores a través del socket de Docker del host (*Docker-outside-of-Docker*).

## 9.2 Ejemplo práctico de Pipeline (`Jenkinsfile`)

### 1. Despliegue de Jenkins con acceso al socket de Docker:
```bash
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

### 2. Estructura de un Pipeline declarativo (`Jenkinsfile`):
```groovy
pipeline {
    agent any

    stages {
        stage('Descarga de Código') {
            steps {
                git branch: 'main', url: '[https://github.com/usuario/mi-app.git](https://github.com/usuario/mi-app.git)'
            }
        }

        stage('Construcción de Imagen Docker') {
            steps {
                sh 'docker build -t mi-app:${BUILD_NUMBER} .'
            }
        }

        stage('Ejecución de Pruebas') {
            steps {
                sh 'docker run --rm mi-app:${BUILD_NUMBER} php ./tests/test.php'
            }
        }

        stage('Despliegue Automatizado') {
            steps {
                sh '''
                docker stop app-produccion || true
                docker rm app-produccion || true
                docker run -d --name app-produccion -p 80:80 mi-app:${BUILD_NUMBER}
                '''
            }
        }
    }
}
```
