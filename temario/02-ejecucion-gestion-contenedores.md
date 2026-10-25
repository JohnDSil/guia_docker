# P2. Ejecutando y gestionando contenedores Docker

## 2.1 Ejecutando contenedores
El comando básico es `docker run [opciones] imagen [comando]`. Si la imagen no está en local, Docker la descarga automáticamente de Docker Hub.

```bash
# Ejecutar un contenedor de Alpine Linux que imprime un mensaje y finaliza
docker run alpine echo "Contenedor ejecutado con éxito"
```

## 2.2 Los flags `-it` y `-d`
- `-i` (*interactive*): Mantiene abierto el flujo de entrada estándar (STDIN).
- `-t` (*tty*): Asigna una pseudoterminal para poder escribir comandos en la consola.
- `-d` (*detached*): Ejecuta el contenedor en segundo plano (como servicio o daemon).

```bash
# Entrar a una shell interactiva dentro de Ubuntu:
docker run -it ubuntu bash

# Lanzar un servidor Nginx en segundo plano:
docker run -d nginx
```

## 2.3 Ejecución de servicios: Puertos y variables de entorno
- **Publicación de puertos (`-p host:contenedor`):** Redirige el tráfico desde una interfaz del servidor host al puerto interno del contenedor.
- **Variables de entorno (`-e VARIABLE=valor`):** Permiten configurar parámetros de ejecución (claves, bases de datos iniciales, usuarios).

```bash
# Servidor web accesible desde el puerto 8080 del host
docker run -d -p 8080:80 nginx

# Instancia de MariaDB con contraseña de root configurada
docker run -d -p 3306:3306 -e MARIADB_ROOT_PASSWORD=secreta mariadb:latest
```

## 2.4 Asignando nombre a los contenedores
Con el parámetro `--name` se evita que Docker asigne nombres aleatorios difíciles de gestionar:

```bash
docker run -d --name mi-servidor-web -p 80:80 nginx
```

## 2.5 Ejecutar órdenes en los contenedores
La orden `docker exec` permite interactuar con un contenedor que **ya se encuentra en ejecución**:

```bash
# Abrir una terminal bash en un contenedor en marcha
docker exec -it mi-servidor-web bash

# Ejecutar una orden puntual sin abrir shell interactiva
docker exec mi-servidor-web nginx -t
```

## 2.6 Obtención de información de los contenedores
- `docker ps` : Muestra los contenedores en ejecución.
- `docker ps -a` : Muestra todos los contenedores (en ejecución o parados).
- `docker logs <nombre/ID>` : Muestra la salida estándar y errores generados por el contenedor.
- `docker inspect <nombre/ID>` : Devuelve el objeto JSON con toda la configuración interna (IP, volúmenes, estado).

## 2.7 Gestión de contenedores (Ciclo de vida)
```bash
docker stop mi-servidor-web     # Detiene el contenedor de forma segura (SIGTERM)
docker start mi-servidor-web    # Vuelve a iniciar un contenedor detenido
docker restart mi-servidor-web  # Reinicia el contenedor
docker rm mi-servidor-web       # Elimina un contenedor detenido
docker rm -f mi-servidor-web    # Fuerza la eliminación de un contenedor en ejecución
```
