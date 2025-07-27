# Todos los comandos de nvm (Node Version Manager), incluyendo los que aparecen al ejecutar nvm --help y algunos otros que pueden ser útiles:

## Comandos Básicos

## WINDOWS

[NodeJS](https://nodejs.org/en)

- `nvm list`: Listar instaladas.
- `nvm list available`: Listar disponibles para descargar.
- `nvm uninstall <version>`: Desinstalar.
- `nvm install <version>`: Instalar
- `nvm use <version>`: Usar

## LINUX

- `nvm ls-remote`: Ver todas las versiones remotas
- `nvm ls-remote --lts` : Ver version remotas LTS (soporte largo)
- `nvm install <version>`: Instalar version especifia
- `nvm install --lts`: Ultima version LTS
- `nvm use <version>`: Usar una version
- `nvm alias <alias> <version>`: Crea un alias para una version
- `nvm alias default <version>`: Establecer un alias “default” (versión por defecto al abrir nuevas sesiones)
- `nvm uninstall <version>`: Borra una version
- `nvm unalias <alias>|default`: Elimina alias o el alias "default" 
- `nvm current`: Version activa acutal
- `nvm ls`: Versiones instladas
- `nvm help`: Mostrar ayuda
- `echo $NVM_DIR`: Ruta de NVM

## Otros comandos que hay que verificar si funcionan (algunos no funcionan en Windows sino en Linux)

- Descargar la ultima version de nvm: [https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)
- Instalar `nvm-setup.exe`
- Actualizar nvm `nvm update`
- Listar versiones de Node.js disponibles `nvm ls-remote`
- Instalar una versión específica de Node.js `nvm install <version>`
- Usar una versión específica de Node.js `nvm use <version>`
- Establecer una versión predeterminada de Node.js `nvm alias default <version>`
- Eliminar una versión específica de Node.js `nvm uninstall <version>`
- Listar versiones instaladas de Node.js `nvm ls`
- Mostrar la versión actual de Node.js `nvm current`
- Ver la versión predeterminada establecida de Node.js `nvm alias default`

### Para instalar NVM en Linux, sigue estos pasos:

Pre-requisitos
* Un intérprete de shell Bourne‑compatible (bash, zsh, etc.).
* curl o wget instalado.
* Permisos de usuario normal (no necesitas sudo para instalar NVM).

Abre tu terminal.

1. Instala curl o wget para descargar el script de instalación de NVM:
```bash
$ sudo apt-get install curl. 
```

2. Descarga e instala NVM ejecutando:
```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
# o con wget
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```
> Nota: La versión (v0.40.3) puede variar. Consulta siempre la [página oficial de NVM en GitHub](https://github.com/nvm-sh/nvm) para ver la última versión disponible.

3. Añadir NVM al entorno de tu shell.
> El instalador de NVM añade automáticamente el siguiente bloque al final de tu archivo de configuración de shell (~/.bashrc, ~/.zshrc, etc.):
>```bash
>export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
>[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"   # This loads nvm
>```
>Si no se añadiera, _inclúyelo tú mismo_ al final de tu ~/.bashrc (o el que corresponda): 
>```bash
> # NVM
> export NVM_DIR="$HOME/.nvm"
> [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
> ```
>Con lo anterior valdría es lo minimo, (opcionalmente se puede poner lo siguien si no)
>```bash
> # NVM
>export NVM_DIR="$HOME/.nvm"
>[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
>[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
>```

4. Carga tu perfil para agregar NVM a tu ruta: (source ~/.bashrc, source ~/.profile, .... el que correpond )
```bash
$ source ~/.bashrc 
```

5. Verifica la instalación ejecutando:
```bash
$ command -v nvm
$ nvm --version
```

> Nota: Para deshacer la instalación de NVM que hiciste
> 1. Elimina el directorio de NVM: `rm -rf ~/.nvm`
> 2. Abre con tu editor de texto (por ejemplo nano, vim o VS Code) los archivos de inicialización de tu shell y elimina las líneas relacionadas con NVM. Habitualmente están en alguno de estos:
>   1. ~/.bashrc
>   2. ~/.bash_profile
>   3. ~/.profile
>   4.  ~/.zshrc
>Por ejemplo, en ~/.bashrc busca y elimina bloques semejantes a:
>```bash
> 
> export NVM_DIR="$HOME/.nvm"
> [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"     # This loads nvm
> [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
> Guarda los cambios y cierra el editor.
> ```
> 3. Recarga tu sesión de shell ()
> ````bash
> source ~/.bashrc   # o el fichero que hayas modificado, p.ej. ~/.zshrc
>```` 
>Tras esto, NVM habrá quedado completamente desinstalado de tu entorno. Si quieres comprobarlo, ejecuta:
>
>```bash
>command -v nvm
>```

Estos pasos te permitirán gestionar versiones de Node.js fácilmente en tu sistema Linux.
## Comandos Avanzados

- Establecer una versión específica de npm para una versión de Node.js `nvm install-latest-npm`
- Listar alias establecidos `nvm alias`
- Eliminar un alias específico `nvm unalias <name>`
- Actualizar npm en la versión actual de Node.js `nvm install-latest-npm`
- Mostrar la ruta al ejecutable de Node.js para una versión específica `nvm which <version>`
- Ejecutar un script con una versión específica de Node.js sin cambiar la versión activa `nvm exec <version> <script>`
- Ejecutar un comando con una versión específica de Node.js sin cambiar la versión activa `nvm run <version> <command>`
- Crear un alias para una versión específica de Node.js `nvm alias <name> <version>`
- Deshabilitar nvm temporalmente `nvm deactivate`
- Establecer automáticamente la versión de Node.js basada en el archivo .nvmrc del directorio actual `nvm use`
- Mostrar la ruta al directorio de Node.js de la versión actual `nvm which current`

## Ejemplos de Uso

- Instalar última versión LTS NodeJS `nvm install --lts`. En Windows no es necesario "--", solo `nvm install lts`
- Usar la última versión LTS de Node.js `nvm use --lts`
- Establecer la última versión LTS como predeterminada `nvm alias default lts/*`
- Comprobar la versión de npm instalada con una versión específica de Node.js `nvm use <version> && npm -v`
- Establecer una versión específica de Node.js para un proyecto `echo <version> > .nvmrc`