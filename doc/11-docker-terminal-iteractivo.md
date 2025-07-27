# Docker - Modo iteracitvo 

En Docker, el **modo interactivo** te permite interactuar directamente con el contenedor, ejecutar comandos dentro de él y ver la salida en tiempo real, lo cual es útil para depuración, administración y pruebas.

### Comando para entrar en modo interactivo:

```bash
docker run -it <imagen> <comando>
```

* **`-i`**: Mantiene la sesión abierta (modo interactivo).
* **`-t`**: Asigna un terminal pseudo-TTY para que puedas ver la salida de manera legible (te permite interactuar como si fuera una terminal real).

### Ejemplo con MariaDB

Si deseas iniciar un contenedor de **MariaDB** en modo interactivo, puedes usar el siguiente comando:

```bash
docker run -it --name mariadb-container mariadb:jammy bash
```

* **`--name mariadb-container`**: Asigna un nombre al contenedor (`mariadb-container`).
* **`mariadb:jammy`**: Especifica la imagen de MariaDB que se usará.
* **`bash`**: Ejecuta el intérprete de comandos `bash` dentro del contenedor (esto te permite interactuar con el sistema de archivos del contenedor).

Una vez ejecutado, estarás dentro de un terminal interactivo dentro del contenedor MariaDB, desde donde puedes ejecutar comandos.

### Explicación de la opción `-it`:

* **`-i` (interactive)**: Permite que el contenedor mantenga la entrada de datos desde la terminal (sin esta opción, el contenedor se detendría inmediatamente después de ejecutarse).

* **`-t` (tty)**: Te da un terminal interactivo con formato adecuado para interactuar (sin esto, no obtendrías la interfaz de terminal adecuada).

### **Salir del modo interactivo:**

* Para salir de la terminal interactiva, puedes usar el comando `exit`, lo cual te regresará a tu terminal local.

```bash
exit
```

### Ejemplo adicional: Conectarse a una base de datos MariaDB interactiva

Si quieres acceder a la base de datos **MariaDB** dentro de un contenedor ya en ejecución y ejecutar comandos SQL:

```bash
docker exec -it mariadb-container mysql -u example-user -p
```

* **`exec`**: Ejecuta un comando dentro de un contenedor que ya está corriendo.
* **`-it`**: Igual que antes, para interacción.
* **`mariadb-container`**: Nombre del contenedor.
* **`mysql -u example-user -p`**: Comando para acceder a MariaDB usando el usuario `example-user`.

Este comando abrirá la consola interactiva de **MySQL/MariaDB** para que puedas empezar a ejecutar consultas SQL directamente.

---

### Resumen de los comandos interactivos

* **Para iniciar un contenedor y entrar en modo interactivo**:

  ```bash
  docker run -it <imagen> <comando>
  ```

* **Para ejecutar comandos dentro de un contenedor en ejecución**:

  ```bash
  docker exec -it <contenedor> <comando>
  ```

* **Para salir del contenedor interactivo**:

  ```bash
  exit
  ```