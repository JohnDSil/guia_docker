# P1. Introducción a Docker

## 1.1 ¿Qué es Docker?
Docker es una plataforma de código abierto que permite aislar aplicaciones dentro de unidades ligeras denominadas **contenedores**. 

### Diferencia frente a Máquinas Virtuales (MV)
- **Máquina Virtual:** Virtualiza el hardware completo, requiere un Sistema Operativo invitado (Guest OS) propio y consume gigabytes de RAM y disco por instancia.
- **Contenedor:** Comparte el mismo kernel del sistema anfitrión (Linux cgroups y namespaces), arrancando en cuestión de milisegundos y consumiendo una fracción mínima de recursos.

```
  Máquinas Virtuales                Contenedores Docker
+-------------------------+     +-------------------------+
| App A   | App B         |     | App A   | App B         |
| Bins/Lib| Bins/Lib      |     | Bins/Lib| Bins/Lib      |
| S.O.    | S.O.          |     +-------------------------+
| Invitado| Invitado      |     |     Docker Engine       |
+-------------------------+     +-------------------------+
|       Hypervisor        |     |       Kernel Host       |
+-------------------------+     +-------------------------+
|      Hardware Host      |     |      Hardware Host      |
+-------------------------+     +-------------------------+
```

## 1.2 Instalación de Docker (Ubuntu)
Procedimiento oficial mediante el repositorio APT de Docker:

```bash
# 1. Actualizar el índice de paquetes e instalar utilidades base
sudo apt-get update
sudo apt-get install ca-certificates curl -y

# 2. Crear el directorio de claves con los permisos adecuados
sudo install -m 0755 -d /etc/apt/keyrings

# 3. Descargar la clave GPG oficial de Docker
sudo curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 4. Añadir el repositorio oficial a la lista de fuentes APT
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Instalar los paquetes del motor de Docker y Docker Compose
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Configuración post-instalación (evitar usar sudo para docker):
sudo usermod -aG docker $USER
newgrp docker
```

## 1.3 Hola Mundo
Para verificar que la instalación es funcional y que el daemon responde:

```bash
# Descarga y ejecuta el contenedor de prueba
docker run hello-world

# Comprobar versiones instaladas
docker -v
docker compose version
```

## 1.4 ¿Qué es Docker Hub?
[Docker Hub](https://hub.docker.com) es el registro público oficial en la nube donde desarrolladores y empresas comparten imágenes de contenedores (equivalente a GitHub, pero para imágenes compiladas). Ofrece imágenes oficiales seguras de Debian, Ubuntu, Nginx, Apache, Python, MariaDB, etc.
