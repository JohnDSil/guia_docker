# P4. Persistencia en Docker

Por defecto, los contenedores son efímeros e inmutables: cuando se destruye un contenedor, todos los datos escritos en su capa de ejecución desaparecen.

## 4.1 Los datos en los contenedores
Para que los datos (bases de datos, archivos subidos, configuraciones) sobrevivan a la destrucción del contenedor se utilizan mecanismos de persistencia fuera del sistema de capas (*copy-on-write*).

## 4.2 Volúmenes frente a Bind Mounts

| Característica | Named Volumes (Volúmenes) | Bind Mounts |
| :--- | :--- | :--- |
| **Ubicación en host** | `/var/lib/docker/volumes/` (gestionado por Docker) | Cualquier ruta arbitraria del host (ej. `/home/user/app`) |
| **Uso habitual** | Bases de datos y almacenamiento de producción | Código fuente en desarrollo y archivos de configuración |
| **Aislamiento** | Alto (gestionado por el daemon) | Depende de los permisos de usuario del host |

```
    [ Bind Mount ]                     [ Named Volume ]
+---------------------+             +---------------------+
| /home/asir/sitio-web|             | /var/lib/docker/... |
+----------+----------+             +----------+----------+
           | (Montaje)                         | (Montaje)
           +-------------> [ CONTENEDOR ] <----+
```

## 4.3 Gestionando volúmenes y obteniendo información
```bash
docker volume create db_datos        # Crear un volumen
docker volume ls                     # Listar volúmenes
docker volume inspect db_datos       # Ver ruta física exacta en el host
docker volume rm db_datos            # Borrar un volumen
docker volume prune                  # Eliminar volúmenes no asociados a contenedores
```

## 4.4 Asociando almacenamiento a los contenedores
### Usando Volúmenes Gestionados (`-v volumen:ruta_contenedor`):
```bash
docker run -d --name db-mariadb -v db_datos:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=asir mariadb:latest
```

### Usando Bind Mounts (`-v ruta_host:ruta_contenedor`):
```bash
docker run -d --name web-php -p 80:80 -v /home/usuario/html:/var/www/html php:8.3-apache
```

## 4.5 Herramientas Docker en Visual Studio Code
La extensión **Docker** de Microsoft para VS Code permite:
- Visualizar contenedores, imágenes y volúmenes desde un panel lateral.
- Conectar una terminal directamente al interior de un contenedor en ejecución con un clic (*Attach Shell*).
- Editar ficheros de configuración con autocompletado para `Dockerfile` y `docker-compose.yml`.

## 4.6 Uso práctico de volúmenes y bind mounts
- **Volumen:** Recomendado para `/var/lib/mysql` o `/var/lib/postgresql` donde el rendimiento de I/O gestionado por Docker es prioritario.
- **Bind mount:** Recomendado para montar un fichero `apache2.conf` externo sobre el contenedor sin necesidad de reconstruir la imagen.

## 4.7 Bind mounts para desarrollo y depuración
Al vincular el directorio del proyecto local con `/var/www/html`, cualquier cambio en los scripts PHP locales se refleja de inmediato en el navegador al recargar la página, acelerando las pruebas de clase sin necesidad de reiniciar el contenedor.
