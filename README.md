#Configuración basica Traefik


### Clonar el repo 

```
git clone https://github.com/elcascarudo-dev/traefik-basico.git
```

### Crear red

```
docker network create web
```

### Ingresar al directorio clonado y ejecutar

```
cd traefik-basico

docker-compose up -d
```

----------------------------------------------

## Que tiene que tener nuestro contenedor para que funciones

En el docker-compose en los servicio que se desea utilizar traefik como proxy reverso deber tener la siguiente configuración

```yaml
version: '3'

networks:
  web:
    external: true
  internal:
    external: false

services:
  servicio1:
    #Configuración del servicio
    [....]
    labels:
      - "traefik.http.routers.servicio1.rule=Host(`url.com`)"
      - "traefik.docker.network=web"
      #Puerto de expocición interna del contenedor
      - "traefik.port=80"
    networks:
      - internal
      - web

  servicio2:
    # Configuración del servicio que no se quiere que lo tome Traefik
    [...]
    networks:
      - internal
    labels:
      - traefik.enable=false
```

Para que traefik entienda a que servicio se debe enlazar las URLs en la siguiente etiqueta se debe tener el mismo nombra que el servicio

```
"traefik.http.routers.{nombre_del_servicio}.rule=Host(`url.com`)"
```


#### Acceder al panel de Traefik

http://[ip_servicor_docker]:8072