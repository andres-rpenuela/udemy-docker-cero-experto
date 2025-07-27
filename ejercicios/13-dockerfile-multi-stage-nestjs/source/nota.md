Nota Importante
En la siguiente clase haremos uso de una aplicación de Nestjs creada en otro curso que estará en el material adjunto.


Luego de descargarlo se recomienda modificar el archivo package.json del proyecto y eliminar todos los "^" de las dependencias.


Esto es para hacer la instalación de las dependencias con las versiones exactas y evitar algunos errores de incompatibilidad entre ellas que nos han reportado.


En el docker file falto poner: `RUN yarn install --pro --frozen-lockfile` en "FROM node:19-alpine3.15 as prod-deps"



Gracias por compartir tu `package.json`. Aquí hay un **problema crítico de incompatibilidad** entre los paquetes de NestJS que está causando tus errores.

---

## ❌ Problema principal: Mezcla de versiones entre NestJS 8 y 9

Tu proyecto usa **NestJS 8 como base**, pero algunos paquetes están en **versión 9**, lo que rompe compatibilidad.

### 🔥 Ejemplos del conflicto:

| Paquete                      | Versión actual |
| ---------------------------- | -------------- |
| `@nestjs/core`               | `^8.0.0` ✅     |
| `@nestjs/websockets`         | `^9.0.4` ❌     |
| `@nestjs/platform-socket.io` | `^9.0.4` ❌     |
| `@nestjs/typeorm`            | `^9.0.0` ❌     |
| `@nestjs/jwt`                | `^9.0.0` ❌     |
| `@nestjs/passport`           | `^9.0.0` ❌     |

---

## ✅ Solución recomendada: Alinear todo a NestJS v8

Ya que tu proyecto está en NestJS 8 (`@nestjs/core`, `@nestjs/common`, etc.), debes degradar todos los paquetes `@nestjs/*` que están en v9 a v8.

### 🔧 Comando para alinear las versiones:

```bash
yarn add \
  @nestjs/websockets@^8.0.0 \
  @nestjs/platform-socket.io@^8.0.0 \
  @nestjs/typeorm@^8.0.0 \
  @nestjs/jwt@^8.0.0 \
  @nestjs/passport@^8.0.0
```

⚠️ Asegúrate de que **todas** las dependencias `@nestjs/*` están en **v8**.

---

## ⏩ Alternativa opcional: Subir a NestJS 9 o 10

Si prefieres actualizar todo a NestJS 9 o 10 (más moderna, pero con posibles breaking changes), puedes hacer:

```bash
yarn add \
  @nestjs/core@^10 \
  @nestjs/common@^10 \
  @nestjs/websockets@^10 \
  @nestjs/platform-socket.io@^10 \
  @nestjs/jwt@^10 \
  @nestjs/passport@^10 \
  @nestjs/typeorm@^10 \
  @nestjs/platform-express@^10 \
  @nestjs/config@^3 \
  @nestjs/swagger@^7
```

Y también actualizar el CLI y demás devDependencies:

```bash
yarn add -D @nestjs/cli@^10 @nestjs/schematics@^10 @nestjs/testing@^10
```

Pero eso **podría requerir adaptar tu código** (por ejemplo, providers con `useClass`, ajustes en `main.ts`, etc.).

---

## 🧼 Luego de cambiar versiones

1. **Limpia dependencias:**

   ```bash
   rm -rf node_modules yarn.lock
   yarn install
   ```

2. **Verifica que todo esté en NestJS 8 (o 10, si actualizaste)**:

   ```bash
   yarn list --pattern @nestjs
   ```

3. **Corre tu build o start:**

   ```bash
   yarn build
   yarn start
   ```

---

¿Prefieres que te dé el `package.json` ya corregido para NestJS 8 o actualizado a 10?
