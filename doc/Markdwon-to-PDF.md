Aquí tienes varias formas **gratuitas** de convertir un archivo **Markdown (`.md`) a PDF**, según tu preferencia (online o local):

---

## 🟢 OPCIÓN 1: Convertir online (sin instalar nada)

### 🌐 [Dillinger](https://dillinger.io/)

1. Abre la web: [https://dillinger.io](https://dillinger.io)
2. Pega o sube tu archivo `.md`
3. Haz clic en `Export as PDF`

---

### 🌐 [markdowntopdf.com](https://markdowntopdf.com/)

1. Súbelo directamente
2. Descarga el PDF generado

---

## 🟢 OPCIÓN 2: Usar VS Code (local, gratuito)

### Requisitos:

* [Visual Studio Code](https://code.visualstudio.com/)
* Extensión: `Markdown PDF`

### Pasos:

1. Abre el archivo `.md` en VS Code
2. Haz clic derecho → `Markdown PDF: Export (pdf)`
3. El archivo `.pdf` aparecerá en el mismo directorio

### Solución (para sistemas Linux)

Debes instalar las dependencias que **Chromium necesita** para ejecutarse. Ejecuta este comando:

```bash
sudo apt update && sudo apt install -y libnss3 libatk1.0-0 libxss1 libasound2 libxshmfence1 libgbm1 libgtk-3-0 libdrm2
```

> Esto instala las bibliotecas más comunes requeridas por Puppeteer/Chromium.

---

## 🟢 OPCIÓN 3: Usar Pandoc (línea de comandos)

### Requisitos:

* Instala [Pandoc](https://pandoc.org/installing.html)
* (Opcional) Instala [LaTeX](https://www.tug.org/texlive/) para mejor formato

### Comando:

```bash
sudo apt install pandoc
pandoc archivo.md -o archivo.pdf
```

> Puedes personalizar mucho el estilo si usas CSS o plantillas.

---

## ✅ ¿Cuál recomiendo?

* Para **una conversión rápida sin instalar nada**: [Dillinger](https://dillinger.io)
* Si usas **VS Code regularmente**: `Markdown PDF`
* Si quieres **automatizar o controlar todo**: `pandoc`

---
