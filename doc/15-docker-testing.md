# Añadir testing + `.dockeringore`

## Caracterisitcas 

- Node version 17.9.1
- npm version 8.11.0

## Crear proeycto Node

```bash
cd .../cron-ticker
npm init
```
## Instalar la depedencias

```bash
npm i node-cron
npm i jest --save-dev
```
> _Nota_: Con la opción `save-dev`, solo se instala en para el perfil de pruebas `dev`

## Añadir el comando "start"

En el `package.json`, del proyecto añadir el comando `start`, para que inicie la app.

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
  },
  "devDependencies": {
    "jest": "^29.7.0"
  }
}
```

## Crea la app

1. Crear la carpeta `tasks`, en la raiz del proyecto

```bash
cd ../cron-ticker
mkdir tasks
```

2. Crea el fichero `sync-db.js`, que se utilizara para sincronizar algún procesamiento, como la conexion a la base de datos.

```js
// sync-db.js
let times = 0;
const syncDb = () => {
    times++;
    console.log('running a task every 5 seconds: '+ (times*5));
    
    return times;
}

module.exports = {
    syncDb
}
```

3. Crear el fichero en la raíz del proyect `app.js` y agrega el siguiente framgento de código usando la funcion `syncDb`:

```bash
cd ../cron-ticker
```

```js
// app.js
const cron = require('node-cron');
const { syncDb } = require('./tasks/sync-db');


cron.schedule('1-59/5 * * * * *', syncDb );

console.log("Inicia")
```

4. Comprueba el funcionamiento, lanzando la aplicacion

```js
npm start

> cron-ticker@1.0.0 start
> node app.js

Inicia
running a task every 5 seconds: 5
running a task every 5 seconds: 10
...
```

## Crea el testing app

1. Crea la carpete `test`, en la raíz del proyecto

```bash
cd ../cron-ticker
mkdir tests
```

2. Dentro de la carpeta `test`, crear la caperta `tasks`, que alojara el archivo `sync-db.test.js`

```bash
cd ../cron-ticker/tests
mkdir tasks
```

```js
// sync-db.test.js
const { syncDb } = require("../../tasks/sync-db")

describe('Pruebas en Sync-DB', () => {
    test('debe de ejecutar el proceso 2 veces', () =>{
        syncDb();
        const times = syncDb();
        console.log('Ejecutado: '+times)

        expect( times ).toBe( 2 );
    });
});
```

3. Modfica el fichero `package.json`, para que lance `jest` cuande se ejeucta los test

```bash
{
  "name": "cron-ticker",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "jest", # <-- poner "jest"
    "start": "node app.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "node-cron": "^4.2.1"
  },
  "devDependencies": {
    "jest": "^29.7.0"
  }
}
```` 

4. Lanzar los test `npm run test`

```bash
npm run test

> cron-ticker@1.0.0 test
> jest


  console.log
    running a task every 5 seconds: 5

      at log (tasks/sync-db.js:5:13)

  console.log
    running a task every 5 seconds: 10

      at log (tasks/sync-db.js:5:13)

  console.log
    Ejecutado: 2

      at Object.log (tests/tasks/sync-db.test.js:7:17)

 PASS  tests/tasks/sync-db.test.js (12.567 s)
  Pruebas en Sync-DB
    ✓ debe de ejecutar el proceso 2 veces (157 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        13.878 s, estimated 23 s
Ran all test suites.
```

## Incorporar pruebas automaticas en la construcion de imagen Docker

1. Crear en la raíz del proyecto el `Dockerfile` y añade la siguiente configuarion

```dockerfile
# Imagen base
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

# Comando por defecto al iniciar el contenedor
CMD ["npm", "start"]
```

2. Monta la imagen

```bash
docker build -t cron-ticker .
```

3. Subir la imange a docker hub

```bash
docker login
docker tag cron-ticker colera/cron-ticker:1.0.0
docker push colera/cron-ticker:1.0.0
```

4. Ejcutar un conecntedor

```bash
docker container run -d colera/cron-ticker:1.0.0
cc057d1d2b7f61e414554c22d4a20fbbd16a2867982154d17d53cdcd4a07acc3
``` 

5. Lanzar un terminal interactivo

```bash
docker exec -it cc05 /bin/sh
/app # pwd
/app
/app # ls -ltr
total 260
drwxrwxrwx    2 root     root          4096 Jul 19 17:02 tasks
-rwxrwxrwx    1 root     root           150 Jul 19 17:04 app.js
drwxrwxrwx    3 root     root          4096 Jul 19 17:09 tests
-rwxrwxrwx    1 root     root           297 Jul 19 17:19 package.json
-rwxrwxrwx    1 root     root        226321 Jul 19 17:20 package-lock.json
drwxrwxrwx    1 root     root         12288 Jul 19 17:20 node_modules
-rwxrwxrwx    1 root     root           399 Jul 19 17:45 Dockerfile
```

> Observese que hay fichersoo  que no son necesarios para que funcione la aplicacion, por lo que se pueden borrar o ignarar


## Dockerignore

Crear en la raíz del proyecto el fichero `.dockerignore`, y añadir lo que se quiere ignorar cuando se ejeucte el `Dockerfile`

```.dockerignore
node_modules/
Dockerfile
package-lock.json
```

Generar de nuevo la imagen

```bash
docker build -t cron-ticker:tigre .
```

Listar las imagnes:
```bash
docker image ls
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
cron-ticker          tigre     0fc4bfe006fe   2 minutes ago    244MB
colera/cron-ticker   1.0.0     66943cdd8099   17 minutes ago   267MB
cron-ticker          latest    66943cdd8099   17 minutes ago   267MB
```
> Observe que el tamaño de la imagen nueva es menor

Subimos la imange nueva a docker hub y la ejecutamos en un contendor, despues abrir una terminal interactiva.

```bash
docker login

docker tag cron-ticker:tigre colera/cron-ticker:tigre

docker push colera/cron-ticker:tigre

docker run -d colera/cron-ticker:tigre 
c01583bacff4c2dcf69794554d8fd293c0bd46a05e7122fc59f9f8af5b07aa37

docker exec -it c0158 /bin/sh
/app # ls -ltra
total 264
drwxrwxrwx    2 root     root          4096 Jul 19 17:02 tasks
-rwxrwxrwx    1 root     root           150 Jul 19 17:04 app.js
drwxrwxrwx    3 root     root          4096 Jul 19 17:09 tests
-rwxrwxrwx    1 root     root           297 Jul 19 17:19 package.json
-rw-r--r--    1 root     root        226321 Jul 19 17:48 package-lock.json
drwxr-xr-x  199 root     root         12288 Jul 19 17:48 node_modules
-rwxrwxrwx    1 root     root            42 Jul 19 18:00 .dockerignore
drwxr-xr-x    1 root     root          4096 Jul 19 18:03 .
drwxr-xr-x    1 root     root          4096 Jul 19 18:09 ..
```
> Observe que ya no esta los ficheros que se añaderon en el ingore, salvo algunos que aparece que son los que se crearon al ejecutar `npm install`, se puede añadir para que se ingore tamibén `.git`, `.dockeringore`, `.gitignore`,... en el propio `.dockerignore`

```.dockerignore
node_modules/

Dockerfile

package-lock.json

.git
.dockerignore
.gitignore
```

### Moficiando `.dockerfile`y volviendo a genrar la imagen
Volviendo a subir la imange  con los cambios den el `.dockerfile`

```bash
docker build -t cron-ticker:tigre .

docker tag cron-ticker:tigre colera/cron-ticker:tigre

# solo para subir la imagen y estamos logout, entonces hacer login
#docker login

docker push colera/cron-ticker:tigre

$ docker image ls
REPOSITORY           TAG       IMAGE ID       CREATED              SIZE
cron-ticker          tigre     28aa13926ed5   About a minute ago   244MB
colera/cron-ticker   tigre     28aa13926ed5   About a minute ago   244MB
colera/cron-ticker   1.0.0     66943cdd8099   28 minutes ago       267MB
cron-ticker          latest    66943cdd8099   28 minutes ago       267MB


docker run -d colera/cron-ticker:tigre 
6bf80ab0890a9147c748bb196ac8b7b2e51bed5457fd8266fc82e768d38ec50f

docker exec -it 6bf8 /bin/sh
/app # ls -ltra
total 260
drwxrwxrwx    2 root     root          4096 Jul 19 17:02 tasks
-rwxrwxrwx    1 root     root           150 Jul 19 17:04 app.js
drwxrwxrwx    3 root     root          4096 Jul 19 17:09 tests
-rwxrwxrwx    1 root     root           297 Jul 19 17:19 package.json
-rw-r--r--    1 root     root        226321 Jul 19 17:48 package-lock.json
drwxr-xr-x  199 root     root         12288 Jul 19 17:48 node_modules
drwxr-xr-x    1 root     root          4096 Jul 19 18:14 .
drwxr-xr-x    1 root     root          4096 Jul 19 18:16 ..
```


### Remover archvios y carpets


```dockerfile
# Imagen base
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
```

Generar la imagen y ejecutar el contendor (_abrir una terminal iteractiva_)

```bash
docker build -t cron-ticker:pantera .

docker tag cron-ticker:pantera colera/cron-ticker:pantera

# solo para subir la imagen y estamos logout, entonces hacer login
#docker login

docker push colera/cron-ticker:pantera

docker image ls
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
colera/cron-ticker   pantera   2925aae50679   19 seconds ago   245MB
cron-ticker          pantera   2925aae50679   19 seconds ago   245MB
colera/cron-ticker   tigre     28aa13926ed5   13 minutes ago   244MB
cron-ticker          tigre     28aa13926ed5   13 minutes ago   244MB
colera/cron-ticker   1.0.0     66943cdd8099   39 minutes ago   267MB
cron-ticker          latest    66943cdd8099   39 minutes ago   267MB

docker run -d colera/cron-ticker:pantera 
e240c49bfd8a5166702a07ee96880e613916cd40fccb50aef2661cba078a7583

docker exec -it e24 /bin/sh
/app # ls -ltra
total 260
drwxrwxrwx    2 root     root          4096 Jul 19 17:02 tasks
-rwxrwxrwx    1 root     root           150 Jul 19 17:04 app.js
-rwxrwxrwx    1 root     root           297 Jul 19 17:19 package.json
-rw-r--r--    1 root     root        226321 Jul 19 18:28 package-lock.json
drwxr-xr-x   12 root     root         12288 Jul 19 18:28 node_modules
drwxr-xr-x    1 root     root          4096 Jul 19 18:28 .
drwxr-xr-x    1 root     root          4096 Jul 19 18:28 ..

/app #  ls node_modules/
@ampproject  @babel       @bcoe        @istanbuljs  @jest        @jridgewell  @sinclair    @sinonjs     @types       node-cron
```
> Nota: Se observa una imge mas limpia a pesar que crecio un mega, al añadir mas capas.