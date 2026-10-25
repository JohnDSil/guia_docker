# P7. Aplicaciones multicapa con docker-compose

Docker Compose es una herramienta para definir y ejecutar aplicaciones Docker de múltiples contenedores mediante un fichero YAML.

## 7.1 Instalación de docker-compose
En versiones modernas se distribuye como plugin nativo (`docker compose`):
```bash
sudo apt-get install docker-compose-plugin -y
docker compose version
```

## 7.2 El archivo `docker-compose.yml`
Estructura declarativa que define servicios, redes y volúmenes:

```yaml
version: '3.8'

services:
  servidor-web:
    image: php:8.3-apache
    container_name: web_asir
    ports:
      - "8080:80"
    volumes:
      - ./sitio:/var/www/html
    networks:
      - red-interna
    depends_on:
      - base-datos

  base-datos:
    image: mariadb:10.11
    container_name: db_asir
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: rootpassword
      MARIADB_DATABASE: app_db
    volumes:
      - datos-db:/var/lib/mysql
    networks:
      - red-interna

networks:
  red-interna:
    driver: bridge

volumes:
  datos-db:
```

## 7.3 La orden `docker compose`
Comandos de gestión del ciclo de vida del stack:

```bash
docker compose up -d       # Descarga, crea redes, volúmenes y arranca los servicios en segundo plano
docker compose ps          # Muestra el estado de los servicios del fichero actual
docker compose logs -f     # Sigue los registros en tiempo real de todos los contenedores
docker compose down        # Detiene y elimina contenedores y redes creadas por el compose
docker compose down -v     # Detiene los contenedores y elimina también los volúmenes nombrados
```

## 7.4 Aplicaciones a la docencia: Entornos de prácticas
Docker Compose permite desplegar pilas completas para prácticas de aula con un único comando:
- Una pila con **Moodle + Base de datos MySQL**.
- Una pila con **Nextcloud + Redis + MariaDB**.
- Permite que cada estudiante trabaje de forma idéntica sin conflictos de versiones en el sistema operativo base.

## 7.5 Publicación de archivos docker-compose en Docker Hub
Los ficheros `docker-compose.yml` pueden compartirse en GitHub o publicarse en Docker Hub como parte de la documentación del proyecto para permitir el despliegue inmediato de la infraestructura completa.

## 7.6 Recursos utilizados
- Documentación oficial de Docker: [docs.docker.com/compose/](https://docs.docker.com/compose/)
- Repositorio oficial de imágenes: [hub.docker.com](https://hub.docker.com)
