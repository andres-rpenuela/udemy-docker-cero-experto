# Guía básica de WSL en Windows

Este manual cubre desde la instalación, configuración de WSL 2, actualización, descarga y cambio de imágenes/distros.

---

## 1. Instalar WSL y Ubuntu

Abre PowerShell como administrador y ejecuta:

```powershell
wsl --install
```

Reinicia el equipo si el sistema lo solicita.

---

## 2. Listar distros instaladas y sus versiones

```powershell
wsl -l -v
```

Esto mostrará una lista con las distros instaladas, su estado y la versión de WSL que usan (1 o 2).

---

## 3. Cambiar una distro a WSL 2

```powershell
wsl --set-version <DistroName> 2
```
> Nota Para ver una lista de distribuciones de Linux disponibles para su descarga a través de la tienda en línea, escriba: wsl --list --online o wsl -l -o.

Ejemplo:

```powershell
wsl --set-version Ubuntu 2
```

---

## 4. Establecer distro predeterminada

```powershell
wsl --set-default <DistroName>
```

Ejemplo:

```powershell
wsl --set-default Debian
```

---

## 5. Abrir consola WSL con una distro específica

```powershell
wsl -d <DistroName>
```

Ejemplo:

```powershell
wsl -d Debian
```

---

## 6. Eliminar una distro

```powershell
wsl --unregister <DistroName>
```

Ejemplo:

```powershell
wsl --unregister Ubuntu
```

---

## 7. Actualizar WSL y el kernel

Para actualizar WSL y el kernel a la última versión disponible, ejecuta:

```powershell
wsl --update
```

---

# Script PowerShell para instalación y configuración básica de WSL

Guarda este script como `setup-wsl.ps1` y ejecútalo en PowerShell como administrador.

```powershell
# Ejecutar como Administrador

# 1. Instalar WSL y Ubuntu (si no está instalado)
wsl --install

# 2. Esperar a que finalice la instalación
Write-Host "WSL instalado, reinicia si es necesario"

# 3. Listar distros y versiones
wsl -l -v

# 4. Cambiar Ubuntu a WSL 2
wsl --set-version Ubuntu 2

# 5. Establecer Ubuntu como distro predeterminada
wsl --set-default Ubuntu

# 6. Actualizar WSL y kernel
wsl --update

Write-Host "WSL configurado y actualizado"
```

---

# Notas adicionales

- Puedes instalar varias distribuciones Linux desde Microsoft Store o con `wsl --install -d <DistroName>`.
- WSL 2 ofrece un kernel Linux completo con mayor compatibilidad.
- Siempre puedes listar tus distros con `wsl -l -v` y gestionarlas según sea necesario.
- Ver distros disponibles remotamente (para instalar) `wsl --list --online`
---
Para verificar las **distribuciones de WSL (Windows Subsystem for Linux)** instaladas localmente y las disponibles de forma remota, puedes usar los siguientes comandos desde PowerShell o CMD:

---

### ✅ **Resumen de comandos útiles**

| Acción                                | Comando                      |
| ------------------------------------- | ---------------------------- |
| Ver distros instaladas                | `wsl --list --verbose`       |
| Ver distros disponibles para instalar | `wsl --list --online`        |
| Instalar una distro                   | `wsl --install -d <Nombre>`  |
| Establecer distro por defecto         | `wsl --set-default <Nombre>` |

---

¿Quieres ayuda para instalar o configurar alguna distro en particular?
