# docker build -t heroes-app . --no-cache
# docker container run -p 80:80 heroes-app
# http://localhost:80 

# Descarga e instalas las dependencias de dev
FROM node:19-alpine3.15 as dev-deps
# Habilitar corepack y Yarn
#RUN corepack enable && corepack prepare yarn@stable --activate
WORKDIR /app
COPY package.json package.json
RUN yarn install


# STAGE test
#FROM node:19-alpine3.15 as testing
## Habilitar corepack y Yarn
#RUN corepack enable && corepack prepare yarn@stable --activate
#COPY --from=dev-deps /app/node_modules ./node_modules
#COPY . .
#RUN yarn test

# STAGE build dev (teste + construccion)
FROM node:19-alpine3.15 as build-dev
# Habilitar corepack y Yarn
#RUN corepack enable && corepack prepare yarn@stable --activate
WORKDIR /app
COPY --from=dev-deps /app/node_modules ./node_modules
COPY . .
RUN yarn build

# No hay fase de produccion en esta app

FROM nginx:1.29.0-bookworm-otel as run
EXPOSE 80
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build-dev /app/dist /usr/share/nginx/html
COPY  assets/ /usr/share/nginx/html/assets/
CMD ["nginx","-g","daemon off;"]

