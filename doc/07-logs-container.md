# Mostar los de un contendero

[Docker hub](https://hub.docker.com/_/mariadb)

```bash
docker pull mariadb:jammy

$ docker run \
  --name some-mariadb \
  -dp 3308:3306 \
  -e  MARIADB_RANDOM_ROOT_PASSWORD=1 \
  mariadb:jammy
aa09848eabeb65166f004a4b094073835ed10711823337c820d0afe2917486f6

# Ver los logs
docker container logs aa09 
...
2025-07-10 16:07:53+00:00 [Note] [Entrypoint]: Temporary server started.
2025-07-10 16:07:55+00:00 [Note] [Entrypoint]: GENERATED ROOT PASSWORD: XeAQ$fU(ZttLcaSq,|M.l=t2mk)fHdOG
2025-07-10 16:07:55+00:º00 [Note] [Entrypoint]: Securing system users (equivalent to running mysql_secure_installation)

# Se ancla y visualiza los logs
docker container logs --follow aa09
2025-07-10 16:22:50 0 [Note] mariadbd: Event Scheduler: Loaded 0 events
2025-07-10 16:22:50 0 [Note] mariadbd: ready for connections.
Version: '11.3.2-MariaDB-1:11.3.2+maria~ubu2204'  socket: '/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution
2025-07-10 16:23:04 3 [Warning] Access denied for user 'root'@'172.17.0.1' (using password: NO)
```