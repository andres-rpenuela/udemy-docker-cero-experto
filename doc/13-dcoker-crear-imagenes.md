## 🐳📁 Imagen con Docker

Crear una **imagen con Docker** implica escribir un archivo llamado `Dockerfile` con instrucciones sobre cómo construir esa imagen. Aquí te dejo una guía paso a paso para hacerlo:


---

## 🐳 Crear una Imagen con Docker

### 📁 1. Estructura de Proyecto

```bash
mi-proyecto/
├── Dockerfile
├── index.js (o app.py, index.html, etc.)
└── package.json (si aplica)
```

---

### 📝 2. Crear el `Dockerfile`

#### 🔹 Ejemplo para Node.js

```dockerfile
# Imagen base
FROM node:18

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar archivos y dependencias
COPY package*.json ./
RUN npm install

# Copiar el resto de la app
COPY . .

# Exponer puerto
EXPOSE 3000

# Comando de inicio
CMD ["node", "index.js"]
```

#### 🔹 Ejemplo para HTML estático con Nginx

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

---

### 🔧 3. Construir la imagen

```bash
docker build -t nombre-de-la-imagen .
```

* `-t`: nombre de la imagen
* `.`: indica el contexto (carpeta actual)

---

### ▶️ 4. Ejecutar un contenedor

```bash
docker run -p 3000:3000 nombre-de-la-imagen
```

* `-p`: mapea puertos (`host:contenedor`)

---

### 🧠 5. Comandos útiles

```bash
docker images             # Ver imágenes locales
docker ps -a              # Ver contenedores (activos e inactivos)
docker stop <ID>          # Detener contenedor
docker rm <ID>            # Eliminar contenedor
docker rmi <imagen>       # Eliminar imagen
```

---

### 📌 Notas

* Asegúrate de tener el `Dockerfile` en la raíz del proyecto.
* Evita copiar archivos innecesarios usando `.dockerignore`.

---

¿Quieres que te genere estos apuntes en un archivo `.md` descargable?
