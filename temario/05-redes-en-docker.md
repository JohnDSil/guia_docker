# P5. Redes en Docker

Docker gestiona la comunicación entre contenedores y hacia el exterior mediante interfaces virtuales de red.

## 5.1 Tipos de redes en Docker (Network Drivers)
- **bridge (por defecto):** Crea un switch virtual en el host (`docker0`). Cada contenedor recibe su propia IP privada en esa subred.
- **host:** El contenedor elimina el aislamiento de red y utiliza directamente la interfaz del anfitrión (no se usa `-p`).
- **none:** Deshabilita toda la pila de red del contenedor (aislamiento total).
- **overlay:** Permite comunicación entre contenedores desplegados en hosts físicos distintos (modo Swarm/Kubernetes).

## 5.2 Gestionando redes
Las redes bridge personalizadas creadas por el usuario habilitan **resolución DNS automática por nombre de contenedor** (la red bridge por defecto no lo permite).

```bash
# Crear una red bridge personalizada
docker network create --driver bridge red-practicas

# Listar redes existentes
docker network ls

# Eliminar una red
docker network rm red-practicas
```

## 5.3 Obteniendo información de las redes
```bash
# Inspeccionar subred, puerta de enlace y contenedores conectados
docker network inspect red-practicas
```

## 5.4 Asociando redes a contenedores
```bash
# Arrancar contenedor conectado a la red
docker run -d --name db --network red-practicas -e MARIADB_ROOT_PASSWORD=asir mariadb:latest

# Arrancar aplicación web en la misma red
docker run -d --name web --network red-practicas -p 80:80 php:8.3-apache

# Probar la conectividad por nombre de host (DNS interno de Docker):
docker exec -it web ping db
```

## 5.5 Iptables en contenedores
Docker manipula directamente la tabla `nat` de `iptables` en el host Linux para realizar *Port Forwarding* y aislamiento:
- Al publicar un puerto (`-p 8080:80`), Docker añade una regla de DNAT en la cadena `DOCKER` de iptables.
- Para inspeccionar las reglas generadas por el motor de Docker en el host:
```bash
sudo iptables -t nat -L -n -v | grep DOCKER
```
