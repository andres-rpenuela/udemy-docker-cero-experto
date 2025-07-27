# Multi Stage Build

El **multi-stage build** en Docker es una técnica que permite crear **imágenes más pequeñas, eficientes y seguras**, dividiendo el proceso de construcción en varias etapas (stages). Cada etapa se basa en una imagen distinta y cumple una función específica (como compilar, testear o preparar archivos), pero **solo la última etapa se usa para generar la imagen final**.

---

### ¿Por qué usar multi-stage build?

1. **Reducir el tamaño de la imagen final**
   No es necesario incluir herramientas de compilación o dependencias de desarrollo en la imagen final.

2. **Mejorar la seguridad**
   Al no incluir herramientas innecesarias, se reduce la superficie de ataque.

3. **Mantener limpio el entorno de producción**
   Solo se copian los archivos estrictamente necesarios a la imagen final.

4. Si un **stage** cambia, la ejecuta y las que le siguen, si no ejecuta la cacheada y pasa al siguiente **stage**
---

### Ejemplo básico

Ejemplo de **multi-stage build con Node.js**, usando una aplicación típica (como una app de React o Express):

---

#### 🚀 Ejemplo: App de React (build frontend en Node.js)

```dockerfile
# Etapa 1: Construcción
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Etapa 2: Servidor web ligero (nginx)
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

##### 🧠 ¿Qué hace esto?

1. **Etapa `builder`**:

   * Usa una imagen con Node.js (`node:18`).
   * Instala las dependencias (`npm install`).
   * Compila la aplicación (`npm run build`).

2. **Etapa final**:

   * Usa una imagen de producción ligera (`nginx:alpine`).
   * Solo copia los archivos estáticos ya compilados (`/app/build`).
   * Sirve esos archivos con Nginx.

---

#### 🛠️ Ejemplo: App de Node.js (API con Express)

```dockerfile
# Etapa 1: Instalación y compilación
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .

# Etapa 2: Imagen final más limpia
FROM node:18-slim
WORKDIR /app
COPY --from=build /app /app
CMD ["node", "index.js"]
```

### ✅ Ventajas:

* No dejas herramientas de desarrollo en producción.
* La imagen final es más pequeña (ideal para despliegue en contenedores o Kubernetes).
* Mayor seguridad y rendimiento.

---

¿Quieres que prepare un `Dockerfile` para un proyecto específico que tengas (como Next.js, NestJS, etc.)?


### Ventajas clave

* Separación de responsabilidades.
* Compatible con herramientas modernas de CI/CD.
* Evita tener que crear imágenes intermedias manualmente.

---

¿Quieres que te muestre un ejemplo para otro lenguaje, como Node.js o Python?
