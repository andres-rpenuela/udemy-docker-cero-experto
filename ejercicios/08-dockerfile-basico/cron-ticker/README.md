# Caracterisitcas

- Node version 17.9.1
- npm version 8.11.0

# Crear proeycto Node

```bash
cd .../cron-ticker
npm init
```

## Aplicacion principal

Crear `app.js`, en el proyecto de Node creado y que muestre por consola `Hola Mundo!`


## Ejecutar la aplicaicon

Hay dos formas de hacerlo:

1. Desde la raíz del proyecto, con el comando:
```bash
node app.js
```

2. Modificando el fichero `package.json`, añadiendo el comando `"start": "node app.js"`, dentro de la propiedad `"scripts"`
```bash
{
  "name": "cron-ticker",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node app.js"
  },
  "author": "",
  "license": "ISC"
}
```
  1. Esto pemritirá lanzar el comando `npm start`, para lanzar la aplicación.

### Integrar la dependeica [cron](https://www.npmjs.com/package/node-cron)

Esta dependencia permite programar tareas/procedimientos-

```bash
npm i node-cron
```

Este comando añadirá la `dependeica node-cron` en el `package.json`, y creara la carpeta `node_modules`en la raíz del proyecto

```bash
{
  "name": "cron-ticker",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node app.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "node-cron": "^4.2.1"
  }
}
```

Agrega el siguiente framgento de código en el `app.js`

```js
const cron = require('node-cron');

let times = 0;
cron.schedule('1-59/5 * * * * *', () => {
    times++;
  console.log('running a task every 5 seconds: '+ (times*5));
});

console.log("Inicia")
```

# DockerFile - primeros pasos

1. Crear el fichero `Dockerfile`, en la raíz del proyecto

2. Buscamos la imange de node con la version 19.2, en Dokcer Hub[Node - Docker hub](https://hub.docker.com/_/node), y será la imagen base

```dockerfile
FROM node:19.2.0-alpine3.17
```

> Esta Imagen, ya ya tiene una carpeta preinstalada que se llama /app, /lib, /usr, ...

3. Se establce el directior de trabajo `/app`

```dockerfile
WORKDIR /app
```

4. Volcamos los archivos de la aplicacion `app.js`, `package.json` en el directorio de trabajo de la imange `/app`

```dockerfile
# archivos de la aplacion
COPY app.js package.json ./
```

5. Instalamos las dependencias definidas en `package.json`

```dockerfile
# dependencias
RUN npm install
```

6. Lanzar la aplicacion con `node app.js` o `npm install`, cuando se cree la imagen y se ejecute

```dockerfile
# Cunado se cree la imange y se ejeucte, ejecutar
#CMD ["node", "app.js"]
CMD ["npm", "start"]
```

7. Construir la imagen, ubicado en el direcotrio donde esta dockerfile

```bash
docker build --tag cron-ticker .
```
> Nota: crea un tag en la image `cron-ticker:latest` y `.` indica el path relativo a donde se encuentra el dockerfile (si no habria que poner el path absoluto)

Capas ejecutdas:
```bash
$ docker build --tag cron-ticker .
 => [1/4] FROM docker.io/library/node:19.2.0-alpine3
 => [2/4] WORKDIR /app
 => [3/4] COPY app.js package.json ./
 => [4/4] RUN npm insta                                           
 ```

8. Listar la imagen

```bash
docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
cron-ticker   latest    f30e155a6b7a   3 minutes ago   175MB
```

9. Ejecutar la imagen en un contenedor

```bash
docker container run cron-ticker

> cron-ticker@1.0.0 start
> node app.js

Inicia
running a task every 5 seconds: 5
running a task every 5 seconds: 10
```

# DockerFile - reconstruir image

- Modifica el app.js, para que indique `Inicio app` en lugar de `Inicia`.
- Copia por separado las dependcias al resto de app y que se ejecute la instalacion de las mismas antes de copiar el codigo fuente de la appa

```dockerfile
## Reubicacion de capas (mas eficiente)
# archvio de dependencias
COPY package.json ./

# dependencias (se instlará solo si hay cambio en package.json)
RUN npm install

# copia el codigo fuente de la aplicacion
COPY app.js ./
```
- Reconstruir la imagen

```bash
docker build --tag cron-ticker .                                                                                0.0s
 => [1/5] FROM docker.io/library/node:19.2.0-alpine3.17@sha256:2770c78                                             
 => CACHED [2/5] WORKDIR /app0.0s
 => [3/5] COPY package.json ./  
 => [4/5] RUN npm install
 => [5/5] COPY app.js ./   
```
> Nota: Lo que pone `CACHED`, signfica que no lo ejecuta porque no ha cambiado

- Modifica el app.js, para que indique `Inicio Aplicacion` en lugar de `Inicio app` y reconstruir (_observar, que no se ejecuta las caps  de 2,3 y 4, y por tanto no lanza de nuevo la instalacion de las dependencias..._)

```bash
 docker build --tag cron-ticker .
 => [1/5] FROM docker.io/library/node:19.2.0-alpine3.17@sha256:2770c7
 => CACHED [2/5] WORKDIR /app
 => CACHED [3/5] COPY package.json ./
 => CACHED [4/5] RUN npm install
 => [5/5] COPY app.js ./
```

**Esto implica, que cuando detecta que una capa se ha cambiado, ejecuta la misma y la subsiguientes a él, pero todas las anteriores viene de la caché.**

## Renombrado imanges

```bash
docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
<none>        <none>    57151aae374b   2 minutes ago    175MB
cron-ticker   latest    9ba401c71ccb   6 minutes ago    175MB
<none>        <none>    f30e155a6b7a   19 minutes ago   175MB
```

```bash
$ docker image tag cron-ticker cron-ticker:castor
```
> Si no se especifica, le pone `:latest`, y esto lo que hace es crear una copia con el nombre nuevo.

```bashcron-t
 docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
<none>        <none>    57151aae374b   9 minutes ago    175MB
cron-t        latest    9ba401c71ccb   12 minutes ago   175MB
cron-ticker   castor    9ba401c71ccb   12 minutes ago   175MB
cron-ticker   latest    9ba401c71ccb   12 minutes ago   175MB
<none>        <none>    f30e155a6b7a   25 minutes ago   175MB
```