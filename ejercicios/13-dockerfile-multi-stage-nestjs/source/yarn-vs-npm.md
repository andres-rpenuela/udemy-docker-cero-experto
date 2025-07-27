## 🧶 **Yarn** – *Gestor de paquetes (Node.js)*

* Alternativa a `npm` creada por Facebook.
* Se usa en **desarrollo de aplicaciones JavaScript/Node.js**.
* Gestiona dependencias (bibliotecas/librerías) de tu proyecto.

```bash
# Instalacion global ( npm i -g yarn )
npm install --global yarn
yarn --version
```
🔧 **Ejemplo de uso:**

```bash
yarn add express
```

---

## 📦 **npm** – *Gestor de paquetes oficial de Node.js*

* Viene preinstalado con Node.js.
* Hace lo mismo que Yarn: **instalar y administrar dependencias**.
* Muy usado en el ecosistema JavaScript.

🔧 **Ejemplo de uso:**

```bash
npm install express
```

📌 **Yarn vs npm:** Ambos hacen lo mismo, pero con diferencias en velocidad, manejo de caché, etc.

---

## 🌐 **Nginx** – *Servidor web y proxy inverso*

* **NO es un gestor de paquetes**, es un **servidor de alto rendimiento**.
* Se usa para:

  * Servir archivos estáticos (HTML, CSS, JS).
  * Actuar como **proxy inverso** entre el navegador y tu backend (Node.js, Python, etc.).
  * Balanceo de carga, compresión, SSL, y más.
* Común en **producción** (por ejemplo, para servir el frontend build de React o Angular).

🔧 **Ejemplo de uso:**

```nginx
server {
    listen 80;
    server_name midominio.com;
    
    location / {
        root /usr/share/nginx/html;
        index index.html;
    }

    location /api/ {
        proxy_pass http://localhost:3000/;
    }
}
```

---

## 🧠 ¿Cómo se relacionan?

* Usas **npm** o **Yarn** para construir tu aplicación frontend o backend.
* Luego usas **Nginx** para **servir el frontend** (como archivos estáticos) y/o hacer de **puente con tu backend** (Node.js, etc.).

---

## 🧪 Ejemplo de flujo completo:

1. **Desarrollo (con npm o yarn):**

   ```bash
   yarn install
   yarn build   # o npm run build
   ```

2. **Producción (con Nginx):**

   * Copias la carpeta `build/` o `dist/` a `/usr/share/nginx/html`.
   * Configuras Nginx para servir la app.

---

# Anexo: Instalacines Globales
Cuando ejecutas:

```bash
npm install --global <paquete>
```

o

```bash
npm i -g <paquete>
```

el paquete se instala **globalmente en tu sistema**, lo que significa que **estará disponible como comando desde cualquier parte del terminal**.

---

## 📍 ¿Dónde se guarda exactamente?

Depende del sistema operativo y la configuración de tu entorno, pero aquí están las rutas por defecto más comunes:

### 🐧 Linux / 🐧 WSL

* **Paquetes:**
  `/usr/local/lib/node_modules/`
* **Binarios (comandos):**
  `/usr/local/bin/`

### 🍏 macOS

* **Paquetes:**
  `/usr/local/lib/node_modules/`
* **Binarios:**
  `/usr/local/bin/`

> ⚠️ Si estás usando **Node.js instalado con `nvm` (Node Version Manager)**, las rutas serán distintas, algo como:

* `/home/tu_usuario/.nvm/versions/node/vXX.X.X/lib/node_modules/`
* `/home/tu_usuario/.nvm/versions/node/vXX.X.X/bin/`

### 🪟 Windows

* **Paquetes:**
  `C:\Users\<tu_usuario>\AppData\Roaming\npm\node_modules\`
* **Binarios:**
  `C:\Users\<tu_usuario>\AppData\Roaming\npm\`

---

## 🔍 ¿Cómo saber la ruta exacta?

Puedes ejecutar estos comandos:

* 📁 **Ver ruta de instalación global**:

  ```bash
  npm root -g
  ```

* 🔧 **Ver ruta de los binarios globales**:

  ```bash
  npm bin -g
  ```

* 🔎 **Ver toda la configuración actual de npm**:

  ```bash
  npm config list
  ```

---

## 📌 ¿Cuándo usar `--global`?

Usa la opción `--global` solo para herramientas que se deben ejecutar como comandos de terminal, por ejemplo:

* `npm install -g typescript`
* `npm install -g nodemon`
* `npm install -g yarn`

---

¿Quieres que revisemos si tienes algún conflicto en tu entorno por tener varias versiones de `npm` o `node`?
