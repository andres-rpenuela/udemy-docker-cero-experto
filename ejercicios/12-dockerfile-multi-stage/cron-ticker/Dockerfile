# docker build -t colera/cron-ticker:stages .
# docker push colera/cron-ticker:stages

# docker buildx create --name mybuilder --use
# docker buildx build --platform linux/amd64,linux/arm64 -t colera/cron-ticker:stages --push .

# docker buildx imagetools inspect colera/cron-ticker:stages

# ---------- Stage 1: Instala dependencias ----------
# Sugerencia: añadir --platform=$BUILDPLATFORM si estás usando buildx y plataformas cruzadas
FROM node:19.2.0-alpine3.17 AS dependencies

# Establece el directorio de trabajo
WORKDIR /app

# Copia el archivo de dependencias
COPY package.json ./

# Instala todas las dependencias (dev + prod)
RUN npm install


# ---------- Stage 2: testing (sería como empezr de cero) ----------
FROM node:19.2.0-alpine3.17 as testing

# Establece el directorio de trabajo
WORKDIR /app

# Copia las dependencias de la imagen anterior y las agrega a la image de `testing`
# (bien indicado la ruta absolut o partiendo del WORKDIR)
COPY --from=dependencies /app/node_modules /app/node_modules
#COPY --from=dependencies /app/node_modules ./node_modules

# Copia todo lo que esta el directorio del dockerfile del host y no esta incluido en el .dockeringore
# al directorio de trabajo de la imagen `testing`
COPY . .

# Ejecuta los tests definidos en package.json
RUN npm run test

# ---------- Stage 3: dependencias de producion ----------
FROM node:19.2.0-alpine3.17 AS deps-pro

WORKDIR /app

# Copia solo lo necesario para producción
COPY --from=dependencies /app/package.json /app

# Instala solo dependencias de producción
RUN npm install --prod


# ---------- Stage 4: run ----------
FROM node:19.2.0-alpine3.17 AS runner

WORKDIR /app

# Copia solo lo necesario para producción
COPY --from=deps-pro /app/node_modules ./node_modules
COPY tasks/ ./tasks
COPY app.js ./

# Establece el comando por defecto para ejecutar la app
CMD ["npm", "start"]
