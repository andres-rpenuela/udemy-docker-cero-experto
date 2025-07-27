# Ejercicio - Sin volúmenes montar base de datos

1. Montar la imagen de MariaDB con el tag jammy, publicar en el puerto 3306 del contenedor con el puerto 3306 de nuestro equipo, colocarle el nombre al contenedor de __world-db__ (--name world-db) y definir las siguientes variables de entorno:
    * MARIADB_USER=example-user
    * MARIADB_PASSWORD=user-password
    * MARIADB_ROOT_PASSWORD=root-secret-password
    * MARIADB_DATABASE=world-db

2. Conectarse usando Table Plus a la base de datos con las credenciales del usuario (NO EL ROOT)
3. Conectarse a la base de datos ```world-db```
4. Ejecutar el query de creación de tablas e inserción proporcionado
5. Revisar que efectivamente tengamos la data


## Solucion

```bash
docker pull mariadb:jammy
...
a6321fa47c19: Pull complete 
12077f74b1db: Pull complete 
Digest: sha256:e101f9db31916a5d4d7d594dd0dd092fb23ab4f499f1d7a7425d1afd4162c4bc
Status: Downloaded newer image for mariadb:jammy
docker.io/library/mariadb:jammy
```

```bash 
docker run --name some-mariadb \
  -e MARIADB_USER=example-user \
  -e MARIADB_PASSWORD=user-password \
  -e MARIADB_ROOT_PASSWORD=root-secret-password \
  -e MARIADB_DATABASE=world-db \
  -dp 3307:3306 \
  mariadb:jammy
 24a81732751524b1497ebde9c60bd9c45c762ac4f4fdc4a03f90efe9a051ff75
``` 
Si borramos el contendor y volvmoes a generarlo, se ve que esta vacio la base de datos.