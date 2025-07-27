# Ngix

Nginx (se pronuncia "engine-x") es un servidor web y proxy inverso de alto rendimiento, muy usado en aplicaciones modernas. 

Algunos de sus usos son:

1. Servir contenido web (HTML, CSS, JS, etc.)
2. Actuar como proxy inverso → redirige tráfico hacia tu backend (Node.js, NestJS, etc.)
4. Balancear carga entre múltiples servidores
5. Manejar certificados SSL (HTTPS)


Ventajas
1. Actua como frontal web delante de aplicaciones como NestJS, Express, Django, etc.
2. Puede exponer una API con HTTPS
3. Permite actuar como balanceador en clústeres Docker, Kubernetes, etc.
4. Disponible para ser usado en CDNs o sitios estáticos para servir archivos súper rápido

| Característica | Nginx       | Apache       |
| -------------- | ----------- | ------------ |
| Rendimiento    | 🔥 Muy alto | Bueno        |
| Conexiones     | Asíncronas  | Por hilo     |
| Memoria        | Baja        | Más alta     |
| Configuración  | Sencilla    | Más flexible |


## 📦 ¿Dónde se instala?
Linux (Ubuntu, Debian, etc.): sudo apt install nginx

Docker:
```bash
docker run -d -p 80:80 nginx
Archivos de config: en /etc/nginx/ (Linux)
```

## Uso con Spring

Nginx puede actuar como proxy inverso para aplicaciones hechas con Spring Boot, igual que lo haría con Node.js, Python, etc.

### 🔧 Ejemplo:
Tu app Spring escucha en el puerto 8080, y Nginx escucha en el puerto 80 o 443 (HTTPS). Entonces:

```nginx

server {
  listen 80;
  server_name midominio.com;

  location / {
    proxy_pass http://localhost:8080;
  }
}
```

* 🔁 Con esto, todo lo que llega a http://midominio.com va a tu backend Spring automáticamente.
* ⚠️ Spring Boot ya trae servidor embebido (Tomcat), pero Nginx sigue siendo útil para HTTPS, balanceo, cache, etc.

La arquitectura sería la siguiente

```
my-project/
│
├── app/                 # Código de tu app Spring Boot
│   └── Dockerfile
│
├── nginx/
│   ├── default.conf     # Config de Nginx
│   └── Dockerfile
│
└── docker-compose.yml
```
### Dockerfile para Spring Boot (my-project/app/Dockerfile)
```bash
FROM openjdk:17-jdk-alpine
COPY target/myapp.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
``` 
> Asegúrate de compilar tu app con ./mvnw package antes para tener target/myapp.jar.

### Config Nginx (my-project/nginx/default.conf)
```nginx
server {
    listen 80;

    location / {
        proxy_pass http://spring:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
> 🔁 spring es el nombre del servicio definido en docker-compose.yml.

### Dockerfile para Nginx (my-project/nginx/Dockerfile)

```dockerfile
FROM nginx:alpine
COPY default.conf /etc/nginx/conf.d/default.conf
```

### docker-compose.yml (raíz del proyecto)
```bash
version: '3.8'

services:
  spring:
    build:
      context: ./app
    container_name: spring-app
    ports:
      - "8080:8080"
  
  nginx:
    build:
      context: ./nginx
    container_name: nginx-proxy
    ports:
      - "80:80"
    depends_on:
      - spring
```

### ✅ Comandos para ejecutar
1. Compila tu app:
```bash
./mvnw clean package
```

2. Inicia los contenedores:
```bash
docker-compose up --build
Accede en tu navegador a:
```
```navegador
http://localhost/
```
> Esto carga Nginx en el puerto 80, que hace proxy a tu app Spring Boot en el 8080.