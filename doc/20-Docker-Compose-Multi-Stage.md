# Docker compose con multistage DOCKERFILE

Ver ejercicio [13-docerkile-multi-stage-nestjs].

Sobre tu fragmento de `docker-compose.yml`, estás definiendo un servicio llamado `app` que usa un `Dockerfile` multi-stage y montas volúmenes. Vamos a desglosarlo línea por línea, especialmente la parte que preguntas:

---

### 🧩 Fragmento explicado:

```yaml
services:
  db:
    image: postgres:14.3
    restart: always
    ports:
      - "5432:5432"
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
    container_name: ${DB_NAME}
    volumes:
      - postgres-db:/var/lib/postgresql/data
  
  # Contenedor para la aplicacion
  app:
    build:
      # Busca donde esta el dockerfile (por defecto, dondo esta en docker.compose.yml)
      context: .
      # hasta que stage del Dockerfile ejecutar (si no se pone, ejecuta todo)
      target: dev
      dockerfile: Dockerfile
    # Comando a lanzar cuadno se genere la imagen
    #command: yarn start:dev

    volumes:
      volumes:
        # Name volumen
        - .:/app/                 # Comparte tu proyecto al contenedor
        # Volumen anonimo
        - /app/node_modules       # Protege las dependencias del contenedor

    container_name: nest-app
    # Le del fichero .evn
    ports:
      - ${PORT}:${PORT}
    # Carga las variables de entorno definidas en el .env, al contendor
    environment:
      APP_VERSION: ${APP_VERSION}
      STAGE: ${STAGE}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_NAME: ${DB_NAME}
      DB_HOST: ${DB_HOST}
      DB_PORT: ${DB_PORT}
      DB_USERNAME: ${DB_USERNAME}
      PORT: ${PORT}
      HOST_API: ${HOST_API}
      JWT_SECRET: ${JWT_SECRET}

volumes:
  postgres-db:
    external: false
```

---

### 🧠 ¿Qué hace cada línea?

* **`context: .`**
  Le dice a Docker que use el directorio actual (`.`) como contexto de construcción.

* **`target: dev`**
  Especifica que se debe detener la construcción del Dockerfile en el **stage llamado `dev`** (usa `FROM ... AS dev` en tu Dockerfile).

* **`volumes:`**
  Define carpetas compartidas entre tu máquina host y el contenedor.

---

### 🔍 ¿Qué hace esta línea?

```yaml
- /app/node_modules
```

Este **crea un volumen anónimo** en la ruta `/app/node_modules` **dentro del contenedor**, que **anula cualquier archivo que venga del host** para esa ruta.

---

### ❓¿Por qué es útil eso?

Cuando montas `.:/app`, también montas el `node_modules` de tu host. Pero eso puede ser un problema porque:

1. **Dependencias nativas** pueden diferir entre tu host y el contenedor.
2. **El `node_modules` del host sobrescribiría el del contenedor**, lo que puede romper cosas.

Entonces, al montar un volumen vacío sobre `/app/node_modules`, te aseguras de que:

✅ Las dependencias **instaladas dentro del contenedor** no se vean afectadas por lo que hay en tu máquina.

---

### ✅ Resumen práctico:

```yaml
volumes:
  - .:/app/                 # Comparte tu proyecto al contenedor
  - /app/node_modules       # Protege las dependencias del contenedor
```

---
