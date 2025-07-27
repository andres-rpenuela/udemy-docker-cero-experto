# image base
FROM node:19.2.0-alpine3.17

# directorio de trabajo (cd app)
WORKDIR /app

###
# archivos de la aplacion
#COPY app.js package.json ./
# dependencias 
#RUN npm install

## Reubicacion de capas (mas eficiente)
# archvio de dependencias
COPY package.json ./

# dependencias (se instlará solo si hay cambio en package.json)
RUN npm install

# copia el codigo fuente de la aplicacion
COPY app.js ./


# Cunado se cree la imange y se ejeucte, ejecutar
#CMD ["node", "app.js"]
CMD ["npm", "start"]