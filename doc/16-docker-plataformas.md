# Forzar una plataforma en la consturccion

Para especificar la platarmoa, indicar en el `FROM`, la opcón `--platform=<arquitectura>`

```bash
cd /mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto/doc/ejercicios/10-dockerfile-platform/cron-ticker
```

```dockerfile
# Imagen base
FROM --platform=linux/amd64 node:19.2.0-alpine3.17

# Establece el directorio de trabajo
WORKDIR /app

# Copia solo las dependencias primero (para aprovechar el cache)
COPY package.json ./

# Instala las dependencias
RUN npm install

# Copia el resto del código fuente (no es buena practica)
COPY . .

# Ejecuta los test
RUN npm run test

# Limpair los ficheros / dependencias no necesarias para produccion
RUN rm -rf tests && rm -rf node_modules

# Instalar solo las dependeicas de produccion
RUN npm install --prod

# Comando por defecto al iniciar el contenedor
CMD ["npm", "start"]
```

Construir la imange

```bash
docker build --tag colera/cron-ticker .
```

Listar las imanges y ver los tags, como el `latest` tiende diferente

```bash
docker image ls
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
colera/cron-ticker   latest    2925aae50679   24 minutes ago   245MB
```

Subir a docker push
```bash
docker push colera/cron-ticker
```

--- 

# (TIPS) ❌ Problemas en tu comando:

```bash
docker buildx -f docker-compose-pro.yml  \
--platform linux/amd64, linux/arm64 \
--tag colera/teslo-shop:1.0.0 --push .
```

1. -f es para docker-compose, no para docker buildx
   → En docker buildx, se usa -f para especificar un Dockerfile, no un docker-compose.yml.

2. docker buildx no usa docker-compose directamente.

3. Hay un pequeño detalle con los espacios en --platform:
  👉 no debe hab er coma con espacio (, ), solo coma (linux/amd64,linux/arm64).

Si tienes un Dockerfile (por ejemplo: Dockerfile o Dockerfile.prod), y quieres hacer build multi-arquitectura y subirlo a un registry:

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -f Dockerfile \
  -t colera/teslo-shop:1.0.0 \
  --push .
```
> 🔁 Puedes reemplazar -f Dockerfile con otro archivo si lo necesitas, como -f Dockerfile.prod.

## ❓¿Y si quieres usar docker-compose con buildx?
Eso sí se puede, pero debes usar docker compose build con buildx habilitado, o crear un BuildKit builder con docker buildx create.

Pero si solo estás construyendo una imagen, no necesitas docker-compose aquí.