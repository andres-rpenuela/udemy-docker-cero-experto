# Imagen base (con build genera para todas las pataformas que se indique, se ponga o no se ponga --platform=$BUILDPLATFORM)
# docker buildx build --platform linux/amd64,linux/arm64 -t myImagen:lastest --push .

#FROM --platform=$BUILDPLATFORM node:19.2.0-alpine3.17
FROM node:19.2.0-alpine3.17 

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