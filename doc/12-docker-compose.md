Claro, aquí tienes una explicación clara y paso a paso de qué es `Docker Compose`, y de **para qué sirve cada parte** del archivo que compartiste:

---

## 🐳 ¿Qué es **Docker Compose**?

**Docker Compose** es una herramienta que te permite definir y ejecutar **aplicaciones Docker multicontenedor** desde un único archivo YAML (`docker-compose.yml`). Con este archivo, puedes lanzar toda tu aplicación con:

```bash
docker compose up
```

Ideal para entornos de desarrollo, pruebas o incluso producción ligera.

---

## 🧩 Análisis del archivo `docker-compose.yml`

```yaml
version: '3'
```

### 🔸 `version: '3'`

Indica la **versión del esquema de Compose** que estás usando.

* La versión `3` es compatible con Docker **Swarm** y es comúnmente usada.

---

### 🔸 `services`

Define los **contenedores** de tu aplicación. En tu ejemplo hay dos servicios:

#### 1. Servicio `db`

```yaml
  db:
    container_name: postgres-database
    image: postgres:15.1
    volumes:
      - postgres-db:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=123456
```

✅ **Explicación:**

| Clave            | Significado                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| `db`             | Es el nombre del servicio, y puede ser cualuier valor                  |
| `container_name` | Nombre personalizado del contenedor (por defecto Compose genera uno)   |
| `image`          | Imagen base que se usará (en este caso `postgres:15.1`)                |
| `volumes`        | Monta un volumen persistente para los datos de Postgres                |
| `environment`    | Variables de entorno (como la contraseña del superusuario de Postgres) |

---

#### 2. Servicio `pgAdmin`

```yaml
  pgAdmin:
    depends_on:
      - db
    image: dpage/pgadmin4:6.17
    ports:
      - "8080:80"
    environment:
      - PGADMIN_DEFAULT_PASSWORD=123456
      - PGADMIN_DEFAULT_EMAIL=superman@google.com
```

✅ **Explicación:**

| Clave         | Significado                                                     |
| ------------- | --------------------------------------------------------------- |
| `depends_on`  | Indica que este servicio debe esperar a que `db` esté iniciado  |
| `image`       | Usa la imagen oficial de `pgAdmin`                              |
| `ports`       | Expone el puerto `80` del contenedor en el `8080` de tu máquina |
| `environment` | Variables para configurar el login por defecto de `pgAdmin`     |

> 🔐 Te permite acceder vía navegador a `http://localhost:8080` usando el email y contraseña definidos.

---

### 🔸 `volumes` (debería estar al final del archivo)

Es **necesaria** si usas `postgres-db` como volumen. Agrega esto al final, una de las siguientes opciones

1. Crea un volumen nuevo volumen automáticamente 
```yaml
volumes:
  postgres-db:
```

#### 🔍 ¿Qué hace?
* Crea un volumen llamado postgres-db, si no existe aún.
* Es interno al proyecto por defecto (no es compartido con otros proyectos o contenedores).
* Docker lo gestiona automáticamente: lo crea, lo monta, y lo elimina (si usas docker compose down -v).

2. Usar un volumen externo (existente)

```yaml
volumes:
  postgres-db:
    external: true
```
#### 🔍 ¿Qué hace?
* No crea el volumen, simplemente asume que ya existe.
* Docker usará ese volumen externo, por ejemplo, uno creado previamente con:

```bash
docker volume create postgres-db
```
* 🟠 Si el volumen no existe → lanzará un error:
```bash
ERROR: Volume postgres-db declared as external, but could not be found.
```
-   🟢 Cuándo usarlo:
    -   Si quieres compartir el volumen entre varios docker-compose o contenedores.
    -   Si el volumen fue creado de forma manual o por otro script.

---

## ✅ ¿Para qué sirve todo esto?

Con este archivo puedes lanzar una base de datos `PostgreSQL` y su cliente gráfico `pgAdmin` con **un solo comando** ejecutado en el direcotrio donde se ecuentra el fichero de configuarion:

```bash
cd /mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto/doc/ejercicios/05-postgres-pgadmin

# Ejecutar docker compose en segundo plano
docker compose up -d
# si no se quiere ejeuctar en diferido 
docker compose up
```

Y todo se configurará automáticamente: volúmenes, contraseñas, puertos y dependencias.

Para limpiar el docker compose:

```bash
docker compose down

# Eliminara voumnes, contenedores, redes ...
WARN[0000] /mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto/doc/ejercicios/05-postgres-pgadmin/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 3/3
 ✔ Container 05-postgres-pgadmin-pgAdmin-1  Removed                         0.0s 
 ✔ Container postgres-database              Removed                         0.0s 
 ✔ Network 05-postgres-pgadmin_default      Removed                         0.3s 

# Eliminar el volume
$ docker volume rm 05-postgres-pgadmin_postgres-db
05-postgres-pgadmin_postgres-db
---

# Anexo:


## 📦 Diferencia entre *named volume* y *bind mount*

| Tipo             | Sintaxis                           | ¿Quién lo gestiona? | Persistencia | Comportamiento inicial                           |
| ---------------- | ---------------------------------- | ------------------- | ------------ | ------------------------------------------------ |
| **Named volume** | `postgres-db:/ruta/del/contenedor` | Docker              | ✅ Sí         | Docker crea un volumen vacío o usa uno existente |
| **Bind mount**   | `./data:/ruta/del/contenedor`      | Tú (host)           | ✅ Sí         | Usa los archivos del *host* (tu PC)              |

---

## 🧪 Ejemplo con **bind mount**

Modifica la sección `volumes` del servicio `db`:

```yaml
services:
  db:
    ...
    volumes:
      - ./postgres-data:/var/lib/postgresql/data
```

Aquí:

* `./postgres-data` es una carpeta local en tu PC (en el mismo directorio del `.yml`)
* `/var/lib/postgresql/data` es el lugar donde Postgres guarda los datos dentro del contenedor

---

## ⚠️ ¿Qué cambia con bind mounts?

1. **Control total del host:** puedes ver, editar y respaldar los archivos directamente desde tu sistema (ej. VS Code, Finder, etc.)
2. **Reemplaza el contenido del contenedor:** si `./postgres-data` está vacía, el contenedor sobreescribirá sus datos con una carpeta vacía (y Postgres puede fallar si faltan archivos).
3. **Más frágil ante errores del host:** si borras o corrompes esa carpeta, pierdes los datos.

---

## ✅ Cuándo usar cada uno + Resumen

| Usar **Named Volume** cuando...                  | Usar **Bind Mount** cuando...                                  |
| ------------------------------------------------ | -------------------------------------------------------------- |
| Quieres simplicidad y seguridad de datos         | Quieres inspeccionar o modificar archivos desde el host        |
| No necesitas acceder directamente a los archivos | Estás desarrollando y necesitas ver los cambios en tiempo real |
| Quieres que Docker administre el volumen         | Quieres tener control manual sobre la ruta                     |


---
## Cunado usar la sección `volumen`

**Si usas un *bind mount*, no necesitas declarar nada en la sección `volumes:` al final del `docker-compose.yml`.**

---

## 🔍 ¿Por qué?

La sección `volumes:` al final del archivo **solo se usa para declarar *named volumes***, como este ejemplo:

```yaml
volumes:
  postgres-db:
```

Esto es útil cuando haces algo como:

```yaml
services:
  db:
    volumes:
      - postgres-db:/var/lib/postgresql/data
```

Aquí `postgres-db` es un *named volume*, y Docker necesita saber que debe crearlo (si no existe), por eso lo declaras al final.

---

## 🛠️ En cambio, con un **bind mount**, haces algo como:

```yaml
services:
  db:
    volumes:
      - ./postgres-data:/var/lib/postgresql/data
```

* `./postgres-data` es una ruta del sistema host.
* Docker **no necesita declararla**, porque ya existe o se puede crear automáticamente en tu máquina.

---

## ✅ Resumen

| Tipo         | ¿Necesita `volumes:` al final? |
| ------------ | ------------------------------ |
| Named volume | ✅ Sí                           |
| Bind mount   | ❌ No                           |

---

Excelente observación 🔍 — esa es una duda muy común cuando se mezclan **`env_file`**, **variables en `environment:`**, y **sustitución de variables** (`${VAR}`) dentro de un `docker-compose.yml`.

Vamos a desglosarlo con precisión 👇

---

## 🧩 Prioridades y comportamiento real de Docker Compose con las vairables de Entorno

Cuando usas algo como esto:

* Ficheros de entorno general + profiles
```properties
# .env
# Archivo .env
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=appdb
MYSQL_USER=appuser
MYSQL_PASSWORD=apppass

# Selecciona el perfil (dev, test, prod)
PROFILE=dev

# Puerto por defecto (puede cambiar en los perfiles)
MYSQL_PORT=3306
-----
# profiles/dev.env
MYSQL_DATABASE=dev_db
MYSQL_USER=dev_user
MYSQL_PASSWORD=dev_pass
MYSQL_PORT=3307

-----
# profiles/test.env
MYSQL_DATABASE=test_db
MYSQL_USER=test_user
MYSQL_PASSWORD=test_pass
MYSQL_PORT=3308

-----
# profiles/prod.env
MYSQL_DATABASE=prod_db
MYSQL_USER=prod_user
MYSQL_PASSWORD=SuperSecret123
MYSQL_PORT=3306
```

* Fragmento de carga de las variables de entorno:
  
```yaml
env_file:
  - .env
  - ./profiles/${PROFILE}.env
environment:
  - MYSQL_TCP_PORT=${MYSQL_PORT}
```

Docker Compose **fusiona todas las fuentes de variables de entorno**, pero con **prioridad de sobrescritura**.

### 🔢 Orden de precedencia (de menor a mayor prioridad)

1. **Valores definidos en la imagen (Dockerfile → `ENV`)**
   Ejemplo:

   ```dockerfile
   ENV MYSQL_DATABASE=default_db
   ```

2. **Variables definidas en `env_file:`**
   Ejemplo: `.env`, `profiles/dev.env`, etc.

3. **Variables declaradas directamente en `environment:` dentro del servicio**
   (Estas *siempre sobrescriben* las de `env_file`).

4. **Variables del entorno del *host*** (si no se definen en los pasos anteriores).
   Ejemplo: si en tu terminal tienes `export MYSQL_PORT=3307`, también se usa.

---

### 🧠 Qué pasa en el caso

En tu `docker-compose.yml`:

```yaml
env_file:
  - .env
  - ./profiles/${PROFILE}.env
environment:
  - MYSQL_TCP_PORT=${MYSQL_PORT}
```

### 🔹 Cómo se interpretan las variables:

1. Docker Compose primero carga `.env` (que define `PROFILE`, `MYSQL_PORT`, etc.).
2. Luego evalúa `${PROFILE}` → y carga `./profiles/dev.env` (u otro perfil).
3. Luego, en la sección `environment:`, la línea `MYSQL_TCP_PORT=${MYSQL_PORT}`
   se **expande usando la última versión cargada de `${MYSQL_PORT}`** (por perfil).
4. Finalmente, todas las demás variables (`MYSQL_USER`, `MYSQL_PASSWORD`, etc.) se pasan desde los `env_file`.

👉 Resultado final: **todas las variables definidas en los `env_file` estarán disponibles dentro del contenedor**, y **solo las que pongas explícitamente en `environment:` se sobrescriben o complementan**.

---

### ✅ Ejemplo concreto

### .env

```bash
PROFILE=dev
MYSQL_PORT=3306
MYSQL_USER=base_user
```

### profiles/dev.env

```bash
MYSQL_DATABASE=dev_db
MYSQL_USER=dev_user
MYSQL_PASSWORD=dev_pass
MYSQL_PORT=3307
```

### docker-compose.yml

```yaml
env_file:
  - .env
  - ./profiles/${PROFILE}.env
environment:
  - MYSQL_TCP_PORT=${MYSQL_PORT}
```

### Resultado dentro del contenedor:

| Variable       | Valor final                                  |
| -------------- | -------------------------------------------- |
| MYSQL_DATABASE | `dev_db`                                     |
| MYSQL_USER     | `dev_user`                                   |
| MYSQL_PASSWORD | `dev_pass`                                   |
| MYSQL_PORT     | `3307`                                       |
| MYSQL_TCP_PORT | `3307` (definido manualmente en environment) |

---

## ⚠️ Recomendación

No repitas todas las variables en `environment:` —
solo coloca ahí las que necesiten **ser mapeadas dinámicamente** (como `MYSQL_TCP_PORT`),
o las que dependan de **expresiones** (`${PROFILE}`, `${MYSQL_PORT}`, etc.).

Ejemplo ideal:

```yaml
env_file:
  - .env
  - ./profiles/${PROFILE}.env
environment:
  MYSQL_TCP_PORT: ${MYSQL_PORT}
```

---


