# P8. Asegurando contenedores (Hardening)

Un contenedor mal configurado puede suponer una vía de escape que comprometa el sistema anfitrión.

## 8.1 Seguridad en las imágenes
- Usar siempre **imágenes oficiales y verificadas** de Docker Hub.
- Emplear imágenes base mínimas como **Alpine** (`alpine:latest`) para reducir la superficie de ataque y el número de paquetes vulnerables.
- Analizar vulnerabilidades con herramientas como **Docker Scout** o **Trivy**:
```bash
docker scout cves php:8.3-apache
```

## 8.2 Seguridad en el proceso de build (Dockerfile)
- **Principio de menor privilegio:** No ejecutar la aplicación como `root`. Usar la instrucción `USER`:
```dockerfile
# Crear un usuario sin privilegios y cambiar a él
RUN groupadd -r appuser && useradd -r -g appuser appuser
USER appuser
```
- Evitar incluir credenciales o tokens en directivas `ENV` dentro del Dockerfile (quedan grabadas en el historial de capas).

## 8.3 Seguridad al arrancar los contenedores
- **Sistema de archivos de solo lectura:**
```bash
docker run -d --read-only nginx
```
- **Limitar consumo de CPU y Memoria (Prevenir DoS local):**
```bash
# Limita a 512MB de RAM y 1 núcleo de procesador
docker run -d --memory="512m" --cpus="1.0" nginx
```
- **Eliminar capacidades del Kernel (*Linux Capabilities*):**
```bash
# Eliminar todas las capacidades y añadir solo la necesaria para abrir puertos
docker run -d --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```
