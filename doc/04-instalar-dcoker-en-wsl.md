# Manual WSL, Carpetas Compartidas y Docker en Ubuntu sobre WSL 2

---

## 1. Instalar Ubuntu en WSL

1. Abre PowerShell como Administrador y ejecuta:
   ```powershell
   wsl --install -d Ubuntu
   ```
2. Comprueba las distros instaladas y su versión:
   ```powershell
   wsl -l -v
   ```
   ```text
     NAME              STATE           VERSION
   * docker-desktop    Stopped         2
     Ubuntu            Stopped         2
   ```
3. Inicia Ubuntu:
   ```powershell
   wsl -d Ubuntu
   ```
4. Dentro de Ubuntu, actualiza tu sistema:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

---

## 2. Carpetas Compartidas entre Windows y WSL

### 2.1 Usar carpetas de Windows desde WSL

1. Crea la carpeta en Windows, por ejemplo:
   ```
   C:\Users\andre\Documents\Angular-Docker-Cero-Experto\doc\Compartida
   ```
2. En WSL, todas las unidades Windows están en `/mnt`. Accede así:
   ```bash
   ls /mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto/doc/Compartida
   ```
3. Para crear subcarpetas:
   ```bash
   mkdir -p /mnt/c/Users/andre/Documents/Angular-Docker-Cero-Experto/doc/Compartida/proyectos
   ```
4. Ya puedes leer/guardar archivos desde Windows y WSL en esa ruta.

---

## 3. Instalar Docker en Ubuntu (WSL 2)

### 3.1 Prerrequisitos

- Ubuntu 22.04, 24.04 o último no‑LTS.
- Conexión a Internet.
- (Si no usas GNOME) instala terminal:
  ```bash
  sudo apt install gnome-terminal
  ```

---

### 3.2 Método recomendado: repositorio oficial

#### Paso 1: configurar repositorio de Docker

```bash
# Dependencias
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release

# Clave GPG de Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Añadir repositorio
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
   https://download.docker.com/linux/ubuntu \
   $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

#### Paso 2: instalar Docker Engine y CLI

```bash
sudo apt-get install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin
```

#### Paso 3: verificar instalación

```bash
sudo docker run hello-world
```

---

### 3.3 Método alternativo: paquete DEB local

1. Descarga `docker-desktop-amd64.deb` en `~/Descargas`.
2. En esa carpeta:
   ```bash
   cd ~/Descargas
   sudo apt-get update
   sudo apt-get install -y ./docker-desktop-amd64.deb
   ```
3. `apt` instalará también las dependencias necesarias.

---

## 4. Permisos y Daemon

### 4.1 Usar Docker sin `sudo`

```bash
sudo usermod -aG docker $USER
```
Cierra y vuelve a abrir sesión en WSL para aplicar.

### 4.2 Iniciar el daemon

- **Con Docker Desktop** (Windows): activa “Use the WSL 2 based engine” en Settings → General.
- **Sin Docker Desktop**: en Ubuntu:
  ```bash
  sudo dockerd
  ```

---

## 5. Comandos Útiles de WSL

| Comando                         | Descripción                                 |
|---------------------------------|---------------------------------------------|
| `wsl --install -d Ubuntu`       | Instala WSL y Ubuntu                        |
| `wsl -l -v`                     | Lista distros y versiones WSL               |
| `wsl -d Ubuntu`                 | Inicia la distribución Ubuntu               |
| `wsl --set-default <Distro>`    | Cambia distro predeterminada                |
| `wsl --update`                  | Actualiza WSL y su kernel                   |
| `wsl --unregister <Distro>`     | Elimina una distro                          |

---

Con esto tendrás un entorno Ubuntu en WSL 2, carpetas compartidas y Docker listo para tu flujo de trabajo.