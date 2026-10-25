# Guía Práctica de Docker y Despliegue de Contenedores

Repositorio con apuntes, comandos y casos de uso prácticos para la administración de contenedores en sistemas Linux (ASIR).

---

## 📑 Índice de Contenidos

| Módulo | Archivo | Contenido Temático |
| :---: | :--- | :--- |
| **P1** | [`01-introduccion-e-instalacion.md`](temario/01-introduccion-e-instalacion.md) | Qué es Docker, instalación en Ubuntu, Hola Mundo y Docker Hub |
| **P2** | [`02-ejecucion-gestion-contenedores.md`](temario/02-ejecucion-gestion-contenedores.md) | Ciclo de vida de contenedores, flags `-it`, puertos (`-p`), variables de entorno (`-e`) y `docker exec` |
| **P3** | [`03-gestion-imagenes.md`](temario/03-gestion-imagenes.md) | Descarga (`pull`), eliminación (`rmi`), inspección y comandos de imágenes |
| **P4** | [`04-persistencia-docker.md`](temario/04-persistencia-docker.md) | Volúmenes administrados, bind mounts, depuración y extensión de Docker en VS Code |
| **P5** | [`05-redes-en-docker.md`](temario/05-redes-en-docker.md) | Drivers de red (bridge, host, none), subredes personalizadas, DNS interno e iptables |
| **P6** | [`06-construyendo-imagenes.md`](temario/06-construyendo-imagenes.md) | `docker commit`, Dockerfile (FROM, RUN, CMD, COPY, EXPOSE) y repositorios remotos |
| **P7** | [`07-multicapa-docker-compose.md`](temario/07-multicapa-docker-compose.md) | Orquestación con `docker-compose.yml`, comandos `up`/`down` y pilas para prácticas |
| **P8** | [`08-asegurando-contenedores.md`](temario/08-asegurando-contenedores.md) | Hardening de imágenes, escaneo de vulnerabilidades, usuarios no-root y límites de recursos |
| **P9** | [`09-ci-cd-jenkins-docker.md`](temario/09-ci-cd-jenkins-docker.md) | Integración y despliegue continuo (CI/CD) con Jenkins y contenedores |

---

## 💻 Entorno de Prácticas
- **Sistema Operativo:** Ubuntu 22.04 / 24.04 LTS
- **Motor:** Docker Community Edition (CE) + Docker Compose Plugin
