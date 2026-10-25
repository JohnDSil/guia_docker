# P6. Construyendo mis propias imágenes

## 6.1 Desde un contenedor en ejecución (`docker commit`)
Aunque no es la práctica recomendada para producción, es posible guardar el estado de un contenedor modificado en una nueva imagen:

```bash
# Modificar contenedor
docker exec -it mi-servidor apt update

# Guardar cambios en una nueva imagen local
docker commit -m "Instaladas dependencias base" mi-servidor mi-imagen-personalizada:v1
```

## 6.2 El fichero `Dockerfile`
Es un fichero declarativo que automatiza la construcción reproducible de imágenes mediante directivas paso a paso:

### Directivas clave:
- `FROM`: Define la imagen base.
- `WORKDIR`: Establece el directorio de trabajo dentro del contenedor.
- `COPY`: Copia ficheros locales del host hacia el sistema de archivos del contenedor.
- `RUN`: Ejecuta comandos durante la fase de construcción (como `apt install`).
- `EXPOSE`: Documenta el puerto de escucha previsto para el servicio.
- `CMD`: Comando por defecto que se ejecutará cuando el contenedor arranque.

### Ejemplo de `Dockerfile` para un entorno LAMP/PHP:
```dockerfile
FROM php:8.3-apache

# Actualizar e instalar extensiones de PHP para MariaDB
RUN apt-get update && apt-get install -y \
    mariadb-client \
    && docker-php-ext-install pdo pdo_mysql

# Copiar el código de la práctica a la raíz del servidor web
COPY ./src /var/www/html/

# Exponer el puerto estándar HTTP
EXPOSE 80

# El comando de arranque ya viene definido en la imagen base de Apache
```

### Compilar la imagen:
```bash
docker build -t mi-app-php:1.0 .
```

## 6.3 Conectando GitHub con Docker Hub
Docker Hub permite configurar *Automated Builds*:
- Vincula un repositorio de GitHub con una cuenta de Docker Hub.
- Cada vez que se hace un `git push` a la rama `main`, los servidores de Docker Hub descargan el código, construyen la imagen automáticamente y la dejan disponible para su descarga con `docker pull`.
*(Nota: Actualmente requiere suscripciones compatibles en Docker Hub para automatizaciones privadas).*
