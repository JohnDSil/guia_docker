# P3. Gestión de imágenes Docker

Una imagen es una plantilla de solo lectura formada por capas superpuestas que contiene el sistema operativo base, librerías y la aplicación empaquetada.

## 3.1 Descarga de imágenes (`pull`)
Para bajar imágenes a la máquina local sin arrancarlas:

```bash
# Descargar la versión más reciente
docker pull debian:latest

# Descargar una versión específica fijada por tag
docker pull php:8.3-apache
```

## 3.2 Borrado de imágenes (`rmi`)
Para eliminar imágenes del almacenamiento local y liberar espacio:

```bash
# Borrar por nombre y tag
docker rmi debian:latest

# Borrar por ID de imagen
docker rmi e9c74fbee6d7

# Eliminar todas las imágenes huérfanas o no utilizadas
docker image prune -a
```

## 3.3 Obteniendo información de las imágenes
- `docker images` : Lista todas las imágenes almacenadas en el host, indicando tamaño, etiqueta e ID.
- `docker history <imagen>` : Muestra las capas que componen la imagen y el comando que creó cada una.
- `docker image inspect <imagen>` : Expone la arquitectura, variables de entorno predefinidas y puertos expuestos.

## 3.4 Más comandos relacionados con imágenes
```bash
# Buscar imágenes en Docker Hub directamente desde la terminal
docker search mariadb

# Asignar un nuevo alias o tag a una imagen local
docker tag nginx:latest mi-registro.local/mi-nginx:v1.0

# Exportar una imagen a un archivo comprimido .tar (para moverla sin internet)
docker save -o mi-imagen.tar php:8.3-apache

# Importar una imagen desde un fichero .tar
docker load -i mi-imagen.tar
```
