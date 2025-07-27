# Docker Hub images
[Postgres](https://hub.docker.com/_/postgres)

[pgAdmin](https://hub.docker.com/r/dpage/pgadmin4)

## 1. Crear un volumen para almacenar la información de la base de datos
docker **COMANDO CREAR** postgres-db

## 2. DB Postgres + pgAdmin
Crea un fichero docker-compose, que contega dos servicios uno, la base de datos de postgres y otro pgAdmin

Vincula los datos de la base de datos al volumen `postgres-db`

# 3. Ingresar a la web con las credenciales de superman
http://localhost:8080/

# 4. Intentar crear la conexión a la base de datos
1. Click en Servers
2. Click en Register > Server
3. Colocar el nombre de: "SuperHeroesDB"  (el nombre no importa)
4. Ir a la pestaña de connection
5. Colocar el hostname "postgres-db" (el mismo nombre que le dimos al contenedor)
6. Username es "postgres" y el password: 123456
7. Probar la conexión

### 6. Volumen externo
Vincula los datos de la base de datos al volumen creado en el step 1. en lugar de que lo cree al ejcutar el compose.

## 11. Saltar de felicidad
<img src="https://media.giphy.com/media/5GoVLqeAOo6PK/giphy.gif" alt="happy" />