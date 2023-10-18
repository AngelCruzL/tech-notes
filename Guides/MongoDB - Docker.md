---
tags: guide mongodb docker
---
----

Esta guía permite crear una instancia de [[MongoDB]] dentro de un contenedor de [[Docker]], ya sea mediante la manera tradicional o utilizando un fichero `docker-compose`

# Utilizar una imagen de MongoDB

Para utilizar la base de datos dentro de un espacio aislado como puede ser un contenedor podemos hacer uso de Docker. Lo primero es descargar la [imagen oficial de mongo](https://hub.docker.com/_/mongo) con el siguiente comando:

```sh
docker pull mongo
```

Después ejecutamos el contenedor con el siguiente comando:

```sh
docker run --name <container name> -d -p 27017:27017 --rm mongo
```
![[Utilities/images/docker/Untitled.png]]

Tras esto podemos ver que se ejecuto correctamente el comando, pues nos devuelve un id perteneciente al contenedor. Para poder utilizar el interprete de comandos bash de este contenedor tenemos que ejecutar el siguiente comando:

```sh
docker exec -it <container name> mongo
```
![[Utilities/images/docker/Untitled-2.png]]

Esto nos da acceso a bash dentro del contenedor. A partir de aquí se debe realizar el mismo proceso que si tuviesemos el servicio en la máquina host.

# Dockerizar el servicio de MongoDB

Para ello necesitamos crear un archivo llamado `docker-compose.yml` con el siguiente contenido:
```yml
version: '3'

services:
  db:
    image: mongo:5
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGODB_DATABASE: database-name
    volumes:
      - ./mongo:/data/db
```

Esto utiliza la imagen de `mongo 5` y crea un volumen dentro de la carpeta `mongo` para la persistencia de los datos.