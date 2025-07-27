
# 📦 Docker – Comandos Básicos

> **Referencia oficial:** [Docker Docs – Get Started](https://docs.docker.com/get-started/)

---

## 📌 ¿Qué es una imagen en Docker?

Una **imagen Docker** es un archivo inmutable que contiene todo lo necesario para ejecutar una aplicación: código, librerías, dependencias, configuraciones, etc. Se construye en **capas** y es la base para crear contenedores.

---

## 🔧 Comandos Fundamentales

### ✅ Descargar una imagen

```bash
docker pull <nombre-de-la-imagen>
```

**Ejemplo:**
```bash
docker pull hello-world
```

> 🔹 Descarga la imagen si no está presente localmente.  
> 🔹 `hello-world` es una imagen oficial que simplemente imprime un mensaje para verificar que Docker funciona correctamente.

---

### ✅ Ejecutar un contenedor

```bash
docker container run <imagen>
# o forma corta:
docker run <imagen>
```

**Ejemplo:**
```bash
docker run hello-world
```

> 🔹 Crea un contenedor nuevo desde la imagen y lo ejecuta.  
> 🔹 Si la imagen no existe en local, primero la descarga.  
> 🔹 Cada ejecución crea un nuevo contenedor.

---

### 📋 Ver ayuda de comandos

```bash
docker container --help
docker image --help
```

> Muestra todas las opciones y subcomandos disponibles.

---

### 📦 Ver contenedores

```bash
docker container ls      # Lista los contenedores activos
docker ps                # Equivalente

docker container ls -a   # Lista todos los contenedores (activos e inactivos)
```

---

### 🗑️ Eliminar contenedores

```bash
docker container rm -f <container_id>   # Elimina un contenedor forzoso
docker container rm <container_id>      # Elimina un contenedor parado
docker container prune                  # Elimina todos los contenedores detenidos
```

---

### 🧼 Eliminar imágenes

```bash
docker image ls                 # Ver imágenes descargadas
docker image rm <image_name>   # Eliminar imagen específica
```

---

### 🔄 Detener e iniciar contenedores

```bash
docker container stop <id>     # Detiene un contenedor en ejecución
docker container start <id>    # Inicia uno detenido
```

---

## 🛠️ Comandos comunes organizados

| Comando largo           | Comando corto   | ¿Qué hace?                           |
|-------------------------|-----------------|--------------------------------------|
| `docker container run`  | `docker run`    | Ejecuta un contenedor                |
| `docker container ls`   | `docker ps`     | Lista contenedores activos           |
| `docker container stop` | `docker stop`   | Detiene un contenedor                |
| `docker container rm`   |                 | Elimina uno o varios contenedores    |
| `docker container prune`|                 | Elimina todos los contenedores parados |
| `docker image ls`       | `docker images` | Lista las imágenes disponibles       |
| `docker image rm`       |                 | Elimina una imagen                   |
| `docker volume ls`      |                 | Lista volúmenes                      |

---

## 🧪 Ejemplo práctico: `hello-world`

### 1. Descargar la imagen

```bash
docker pull hello-world
```

Salida típica:
```bash
Using default tag: latest
latest: Pulling from library/hello-world
...
Status: Image is up to date for hello-world:latest
```

---

### 2. Ejecutar un contenedor con la imagen

```bash
docker run hello-world
```

Salida esperada:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

---

## 🌐 Publicar puertos + modo desapegado

Docker puede publicar puertos de contenedores en tu máquina host.

### 📌 Comando:

```bash
docker run -d -p <puerto-local>:<puerto-contenedor> <imagen>
```

**Ejemplo:**
```bash
docker run -d -p 8080:80 docker/welcome-to-docker
```

| Parte                        | Significado                                                                 |
|-----------------------------|------------------------------------------------------------------------------|
| `-d`                        | Modo desapegado: ejecuta el contenedor en segundo plano                     |
| `-p 8080:80`                | Redirige el puerto 80 del contenedor al 8080 del host                       |
| `docker/welcome-to-docker`  | Imagen que muestra una página HTML estática de bienvenida                   |

> Abre tu navegador en `http://localhost:8080/` para ver la página web.

---

### 🔍 Ver contenedores en ejecución

```bash
docker container ls
```

Ejemplo de salida:
```
CONTAINER ID   IMAGE                      STATUS       PORTS
f1ed9c7fd32f   docker/welcome-to-docker   Up 9 minutes 0.0.0.0:8080->80/tcp
```

--- 

### 🔍 Ver contenedores en instaldos

```bash
docker container ls -a
```

Ejemplo de salida:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
---

## 🔁 Iniciar / detener contenedor publicado

```bash
docker container stop <id>   # Detener
docker start <id>            # Reiniciar
```

Puedes usar los primeros dígitos del ID como atajo si no hay conflictos.

---

## 🧼 Eliminar contenedor e imagen

```bash
docker container rm <id>     # Eliminar contenedor detenido
docker image rm <nombre>     # Eliminar la imagen
```

---

## 🧠 Notas adicionales

- Cada vez que corres `docker run`, **se crea un nuevo contenedor**.
- El nombre del contenedor se genera automáticamente (por ejemplo `lucid_liskov`) si no se especifica.
- Las imágenes se pueden usar para lanzar múltiples contenedores independientes.

---

## 📚 Recursos útiles

- 📘 [Documentación oficial de Docker](https://docs.docker.com/get-started/)
- 📦 [Docker Hub: hello-world](https://hub.docker.com/_/hello-world)
- 📦 [Docker Hub: welcome-to-docker](https://hub.docker.com/r/docker/welcome-to-docker)
```
