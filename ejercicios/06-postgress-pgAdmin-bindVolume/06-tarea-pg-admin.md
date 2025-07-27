# Docker Hub images
[Postgres](https://hub.docker.com/_/postgres)

[pgAdmin](https://hub.docker.com/r/dpage/pgadmin4)

## 1 DB Postgres + pgAdmin
Crea un fichero docker-compose, que contega dos servicios uno, la base de datos de postgres y otro pgAdmin

Vincula los datos de la base de datos al bind volumen (carpeta:`./postgres-data` del host)

> Nota: ' ./' indica el path relativo done se ejecuta `docker compose` (si no existe la carpeta la crea)
# 2. Ingresar a la web con las credenciales de superman
http://localhost:8080/

# 3. Intentar crear la conexión a la base de datos
1. Click en Servers
2. Click en Register > Server
3. Colocar el nombre de: "SuperHeroesDB"  (el nombre no importa)
4. Ir a la pestaña de connection
5. Colocar el hostname "postgres-db" (el mismo nombre que le dimos al contenedor)
6. Username es "postgres" y el password: 123456
7. Probar la conexión

## 4. Saltar de felicidad
<img src="https://media.giphy.com/media/5GoVLqeAOo6PK/giphy.gif" alt="happy" />


---

## Commands Helper

```bash
$ cd /mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto/doc/ejercicios/06-postgress-pgAdmin-bindVolume
..06-postgress-pgAdmin-bindVolume$ docker compose up

# Verifica red y conexión
$ docker network ls
$ docker network inspect nombre-de-la-red

# Diagnóstico desde dentro del contenedor
$ docker exec -it pgadmin ping db
ping: bad address 'db'

$ docker exec -it pgadmin ping postgres_database
PING postgres_database (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=42 time=0.204 ms
```

--- 

## Importante

Si se usa `wsl`, no se recomeinda usar `bind volumen`, porque da problemas de permisos:

```bash
postgres_databasesudo  | chmod: changing permissions of '/var/lib/postgresql/data': Operation not permitted
```

En lugar se puede sar `volumen name`, o que se guarde en el host de wsl

```bash
    volumes:
      # Bind mount para persistencia (con wsl), vuelva en /home/{user}/, no se puede acceder desde windows
      - ~/postgres-data:/var/lib/postgresql/data  
      # Bind mount para persistencia (sin wsl)
#      - ./postgres-data:/var/lib/postgresql/data  
    environment:
```


Ejecucion de docker compose en wsl (usar `~/`)

```bash
root@MSI:/mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto# ls /home/andre/postgres-data/
PG_VERSION    pg_hba.conf    pg_replslot   pg_subtrans  postgresql.auto.conf
base          pg_ident.conf  pg_serial     pg_tblspc    postgresql.conf
global        pg_logical     pg_snapshots  pg_twophase  postmaster.opts
pg_commit_ts  pg_multixact   pg_stat       pg_wal       postmaster.pid
pg_dynshmem   pg_notify      pg_stat_tmp   pg_xact
```

> Nota: Limpir los datos del volumen alamacneados en wsl (como administrador) `# rm -r /home/andre/`