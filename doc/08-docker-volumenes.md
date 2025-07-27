# Volúmenes en Docker

Los volúmenes en Docker permiten mantener persistencia de datos, incluso si se borra el contenedor. Es útil para conservar datos entre reinicios, actualizaciones o recreación de contenedores.

---

## Tipos de volúmenes

### 1. Volúmenes Anónimos

Se crean cuando se usa `-v /ruta/en/el/contenedor` sin especificar el lado del host:

```bash
docker run -v /var/lib/mysql/data IMAGE
```
o

```bash
docker run --name some-mariadb \
  -e MARIADB_USER=example-user \
  -e MARIADB_PASSWORD=user-password \
  -e MARIADB_ROOT_PASSWORD=root-secret-password \
  -e MARIADB_DATABASE=world-db \
  -dp 3307:3306 \
  -v /var/lib/mysql \
  mariadb:jammy
```

Docker interpreta `-v /var/lib/mysql` como un volumen **anónimo** y lo monta dentro del contenedor en esa ruta.

**Consecuencias:**

- 🔄 **No reutilizable fácilmente**: no tiene nombre, así que es difícil volver a usarlo.
- 🗑️ **Fácil de perder**: al ejecutar `docker rm -v <container>`, también se borra.
- 🕵️‍♂️ **Ubicación poco clara**: no sabes dónde está sin usar `docker volume inspect`.

---

### 2. Volúmenes con Nombre (Named Volumes)

Docker los gestiona internamente y tú defines su nombre. Ejemplo:

```bash
# Crear volumen
docker volume create todo-db

# Inspeccionar ubicación
docker volume inspect todo-db

# Usar volumen al iniciar contenedor
docker run -v todo-db:/etc/todos getting-started

# Borrar volumen
docker volume rm todo-db
```

📁 **Ubicación del volumen en el host (Linux):**
```
/var/lib/docker/volumes/todo-db/_data
```
> En esta ruta estara todo el contenido de la carpeta `/etc/todos` que genere el contenedor

---

### 3. Volúmenes Bind Mount (Montaje del sistema de archivos)

Montan una ruta **absoluta** del host y refleja el contenido de la carpeta especificada del contenedor:

```bash
docker run --name some-mariadb \
  -e MARIADB_USER=example-user \
  -e MARIADB_PASSWORD=user-password \
  -e MARIADB_ROOT_PASSWORD=root-secret-password \
  -e MARIADB_DATABASE=world-db \
  -dp 3307:3306 \
  -v $(pwd)/mariadb-data:/var/lib/mysql \
  mariadb:jammy
```

✅ Ventajas:

- Puedes ver/modificar los datos directamente con `ls`, `nano`, etc.
- Muy útil en desarrollo.

---

## Comandos útiles

### Ver todos los volúmenes (incluye anónimos)
```bash
docker volume ls
```

### Ver detalles de un volumen (incluye ubicación)
```bash
docker volume inspect <volume_name>
```

Ejemplo:
```bash
docker volume inspect ec3a8bcb17da5b8...
```

### Borrar todos los volúmenes no usados (anónimos incluidos)
```bash
docker volume prune
```

### Borrar un volumen específico
```bash
docker volume rm <volume_name>
```

---

## Notas adicionales

- Docker borra automáticamente los volúmenes anónimos cuando usas `docker rm -v`.
- Los volúmenes nombrados no se eliminan a menos que lo hagas explícitamente.
- Los bind mounts requieren rutas absolutas.

---

📚 Más información: [Documentación oficial de Docker sobre volúmenes](https://docs.docker.com/storage/volumes/)
