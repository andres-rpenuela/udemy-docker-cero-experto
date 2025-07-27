# Se recomenda instalar `yarn`de forma global
# Si da problema de dependencias, eliminar `^` de las dependencias de package.json


# Crear una image para varias platormas + Docker compose
# 1. Descargar la imagne de buildx
# 2. Entrar en docker hub y crear un repositorio `colera/teslo-shop`
# 3.1 docker buildx build   --platform linux/amd64,linux/arm64  -f Dockerfile  -t colera/teslo-shop:1.0.0 --push .
# 3.2 docker buildx build   --platform linux/amd64,linux/arm64  -f Dockerfile  -t colera/teslo-shop --push .
# 4. Crear un docker compose que use la imagen subida a docker hub (tarea)
# Nota: crea la imagne con el código dentro

# Descarga e instalas las dependencias de dev
FROM node:19-alpine3.15 as dev-deps
WORKDIR /app
COPY package.json yarn.lock ./
# Ejecutado en el proyecto y evitar hacerlo aquí
# RUN yarn add typescript@^5.0.0 --dev
#RUN yarn add \
#  @nestjs/websockets@^8.0.0 \
#  @nestjs/platform-socket.io@^8.0.0 \
#  @nestjs/typeorm@^8.0.0 \
#  @nestjs/jwt@^8.0.0 \
#  @nestjs/passport@^8.0.0
RUN yarn install


# STAGE DEV: Inicia modo dev (sin comprobar test)
FROM node:19-alpine3.15 as dev
EXPOSE 3000
WORKDIR /app
ENV APP_VERSION=${APP_VERSION}
COPY --from=dev-deps /app/node_modules ./node_modules
# Puede usarse `command` en el docker compose
CMD [ "yarn","start:dev"]

# STAGE BUILDER: Compila y ejeucta los test
FROM node:19-alpine3.15 as builder
WORKDIR /app
COPY --from=dev-deps /app/node_modules ./node_modules
COPY . .
RUN yarn test
RUN yarn build

# Descarga e instalas las dependencias de produccion
FROM node:19-alpine3.15 as prod-deps
WORKDIR /app
COPY package.json ./
# Ejecutado en el proyecto y evitar hacerlo aquí
# RUN yarn add typescript@^5.0.0
#RUN yarn add \
#  @nestjs/websockets@^8.0.0 \
#  @nestjs/platform-socket.io@^8.0.0 \
#  @nestjs/typeorm@^8.0.0 \
#  @nestjs/jwt@^8.0.0 \
#  @nestjs/passport@^8.0.0
RUN yarn install --pro 

# STAGE BUILDER: Compila y ejeucta los test
FROM node:19-alpine3.15 as builder-pro
WORKDIR /app
COPY --from=prod-deps /app/node_modules ./node_modules
COPY . .
# RUN yarn test
RUN yarn build

# STAGE POR: Inicia modo pro (sejeucta el build)
FROM node:19-alpine3.15 as pro
EXPOSE 3000
WORKDIR /app
ENV APP_VERSION=${APP_VERSION}
COPY --from=prod-deps /app/node_modules ./node_modules
COPY --from=builder-pro /app/dist ./dist

CMD [ "node","dist/main.js"]









