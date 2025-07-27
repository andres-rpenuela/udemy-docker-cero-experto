# Bind Volumes

Bind mounts (o bind volumes) en Docker es esencial para trabajar con archivos entre tu sistema y los contenedores.

---

## 🔗 ¿Qué es un Bind Volume (Bind Mount)?

Un **bind mount** en Docker es una forma de **vincular una carpeta o archivo de tu máquina anfitriona (host)** directamente dentro de un contenedor. Esto significa que **los cambios en los archivos se reflejan en ambos lados**, en tiempo real.

🧠 Es ideal cuando quieres:

* Editar código localmente y verlo correr dentro del contenedor
* Compartir configuraciones
* Montar archivos que ya existen en tu sistema

---

## 📁 Estructura general

```
docker run -v /ruta/en/tu/host:/ruta/en/el/contenedor imagen
```

* **/ruta/en/tu/host** → carpeta o archivo en tu máquina local
* **/ruta/en/el/contenedor** → ruta dentro del contenedor

---

## 📌 Ejemplo

```bash
docker run -v $(pwd)/app:/usr/src/app node
```

* Monta tu carpeta local `./app` dentro del contenedor, en `/usr/src/app`
* Si cambias un archivo en `./app`, **se refleja inmediatamente dentro del contenedor**

> Nota: Cuando se monta Docker a través de wsl (Linux en Windows), las rutas van montadas sobre `/mnt/`
> Ejemplo: 
> ```shell
>$ pwd
>/mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto
>```

---

## 🖼️ Imagen explicativa

Aquí tienes una imagen sencilla para visualizar cómo funciona un **bind volume**:

![Bind Mount Docker Diagram](https://raw.githubusercontent.com/jorgepastorr/docker-binds-volumes/master/bind-mount.png)

> Fuente: GitHub (Jorge Pastor – Docker bind volumes)

---

## 🔍 Diferencia con Volúmenes normales de Docker

| Característica   | Bind Mount               | Docker Volume                 |
| ---------------- | ------------------------ | ----------------------------- |
| Ubicación física | Cualquier parte del host | Gestionado por Docker         |
| Visibilidad      | Control total del host   | Docker lo maneja internamente |
| Portabilidad     | Menor                    | Alta (ideal para backups)     |
| Uso típico       | Desarrollo               | Producción, persistencia      |

---

## 🎯 ¿Cuándo usar Bind Mounts?

✅ Casos típicos:

* Desarrollo de aplicaciones (Node, Python, etc.)
* Compartir logs o archivos de configuración
* Conectarse a servicios externos desde contenedores

❌ No recomendado para:

* Contenedores en producción que requieran aislamiento total
* Ambientes donde se requiera persistencia gestionada (mejor usar `volumes`)

---

