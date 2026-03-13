# Guía de Comandos de Git

Este documento agrupa y explica los comandos esenciales de Git para la gestión de versiones, junto con buenas prácticas y comandos útiles adicionales.

## Índice
- [Fundamentos de Git](#fundamentos-de-git)
- [Configuración Inicial](#configuración-inicial)
- [Comandos Básicos del Flujo de Trabajo Local](#comandos-básicos-del-flujo-de-trabajo-local)
  - [Inicializar y Clonar](#inicializar-y-clonar)
  - [Seguimiento de Cambios (Status, Add, Commit)](#seguimiento-de-cambios-status-add-commit)
  - [Eliminar Archivos (Rm)](#eliminar-archivos-rm)
- [Análisis y Comparación de Cambios](#análisis-y-comparación-de-cambios)
- [Navegación y Visualización del Historial](#navegación-y-visualización-del-historial)
- [Deshacer Cambios](#deshacer-cambios)
  - [Git Reset](#git-reset)
  - [Git Revert](#git-revert)
  - [Git Commit --amend](#git-commit---amend)
- [Ramas (Branches) y Fusión (Merge)](#ramas-branches-y-fusión-merge)
  - [Gestión de Ramas (Branch, Checkout)](#gestión-de-ramas-branch-checkout)
  - [Fusionar Ramas (Merge)](#fusionar-ramas-merge)
  - [Rebase](#rebase)
- [Trabajo con Repositorios Remotos](#trabajo-con-repositorios-remotos)
  - [Gestión de Remotos (Remote)](#gestión-de-remotos-remote)
  - [Subir y Bajar Cambios (Push, Pull)](#subir-y-bajar-cambios-push-pull)
  - [Colaboración: Forks y Pull Requests](#colaboración-forks-y-pull-requests)
- [Comandos para Casos Especiales](#comandos-para-casos-especiales)
  - [Tags (Etiquetas)](#tags-etiquetas)
  - [Git Stash (Guardado Temporal)](#git-stash-guardado-temporal)
  - [Git Clean (Limpiar Archivos no Trackeados)](#git-clean-limpiar-archivos-no-trackeados)
  - [Git Cherry-pick (Seleccionar Commits)](#git-cherry-pick-seleccionar-commits)
  - [Git Reflog (El Registro de Referencias)](#git-reflog-el-registro-de-referencias)
- [Búsqueda y Diagnóstico](#búsqueda-y-diagnóstico)
  - [Buscar en Archivos (Grep)](#buscar-en-archivos-grep)
  - [Buscar en Commits (Log -S)](#buscar-en-commits-log--s)
  - [Estadísticas y Autoría (Shortlog, Blame)](#estadísticas-y-autoría-shortlog-blame)
- [Alias y Personalización](#alias-y-personalización)
- [Comandos Útiles Adicionales](#comandos-útiles-adicionales)
- [Historial de Comandos del Sistema](#historial-de-comandos-del-sistema)

---

## Fundamentos de Git
Git es un sistema de control de versiones que funciona como una base de datos de cambios. Guarda información sobre las modificaciones dentro de un directorio (repositorio). Lo hace por archivo completo en el caso de binarios, o por línea/fragmento en el caso de texto plano.

Los archivos pasan por varios estados antes de ser registrados definitivamente:

- **Working Directory:** El directorio donde trabajas y haces modificaciones.
- **Staging Area (Index):** Un área intermedia donde se preparan (agregan) los cambios que formarán parte del próximo commit.
- **Git Repository:** La base de datos donde se almacenan los commits (las versiones guardadas).

El siguiente diagrama ilustra este flujo:
[WORKING DIRECTORY]
│
│ (Modificas archivos)
│
▼
[STAGING AREA]
│
│ (git add)
│
▼
[GIT REPOSITORY]
│
│ (git commit)
│
▼
[REPOSITORIO REMOTO] (Opcional, con git push)

---

## Configuración Inicial
Antes de empezar a hacer commits, es necesario identificar al usuario que realiza los cambios.

- `git config --list`
    - Muestra toda la configuración actual de Git.
- `git config --list --show-origin`
    - Muestra la configuración y la ubicación de los archivos donde están definidas.
- `git config --global user.name "Tu Nombre"`
    - Configura el nombre de usuario de forma global para todos los repositorios.
- `git config --global user.email "tu@email.com"`
    - Configura el correo electrónico de forma global.
- `git config --global core.editor "code --wait"`
    - Comando útil para configurar Visual Studio Code como el editor por defecto para los mensajes de commit.

---

## Comandos Básicos del Flujo de Trabajo Local

### Inicializar y Clonar
- `git init`
    - Inicializa un nuevo repositorio de Git en el directorio actual. Crea la carpeta oculta `.git` donde se almacenará toda la base de datos de versiones.
- `git clone <url-del-repositorio>`
    - Crea una copia local de un repositorio remoto existente.

### Seguimiento de Cambios (Status, Add, Commit)
- `git status`
    - Muestra el estado actual del working directory y el staging area. Indica qué archivos han sido modificados, cuáles están en staging y cuáles no están siendo trackeados (untracked).
- `git add <nombre-archivo>`
    - Añade un archivo específico al staging area.
- `git add .`
    - Añade **todos** los archivos nuevos y modificados del directorio actual (y subdirectorios) al staging area.
- `git commit -m "Mensaje descriptivo de los cambios"`
    - Toma todos los archivos que están en el staging area y los guarda en el repositorio como un nuevo commit (una nueva versión). El mensaje debe ser claro y conciso.
- `git commit -am "Mensaje descriptivo"`
    - Es un atajo que combina `git add .` (solo para archivos ya trackeados) y `git commit -m`. Añade y commitea los cambios de archivos que Git ya está siguiendo, omitiendo el paso de `git add` manual. **No funciona para archivos nuevos (untracked).**

### Eliminar Archivos (Rm)
- `git rm <archivo>`
    - Elimina el archivo tanto del working directory como del staging area, y prepara esta eliminación para el próximo commit. El historial del archivo no se borra, solo su versión actual.
- `git rm --cached <archivo>`
    - Elimina el archivo del staging area, pero lo mantiene en el working directory. El archivo deja de ser trackeado por Git (pasa a estado `untracked`). Es útil cuando agregaste un archivo por error (como archivos de configuración locales).
- `git rm --force <archivo>`
    - Elimina el archivo de Git (staging y working directory) y del disco duro. Úsalo con precaución, aunque el historial del archivo permanece en Git.

---

## Análisis y Comparación de Cambios
- `git diff`
    - Compara los cambios en el working directory que **aún no** han sido agregados al staging area.
- `git diff --staged` (o `git diff --cached`)
    - Compara los cambios que ya están en el staging area (preparados para el próximo commit).
- `git diff <id-commit-1> <id-commit-2>`
    - Muestra las diferencias entre dos commits específicos.
- `git show`
    - Muestra los detalles del último commit: metadatos y los cambios (diff) que introdujo.
- `git show <archivo>`
    - Muestra los cambios de un archivo específico en el último commit.
- `git show <id-commit>`
    - Muestra los detalles de un commit en particular.

---

## Navegación y Visualización del Historial
- `git log`
    - Muestra el historial de commits en el repositorio actual.
- `git log --oneline`
    - Muestra el historial de forma compacta, con cada commit en una sola línea (ID corto y mensaje).
- `git log --graph`
    - Muestra el historial como un gráfico de ramas y merges.
- `git log -S"<palabra>"`
    - Busca en el historial los commits que introdujeron o eliminaron una palabra o fragmento de código específico. (Explicado más adelante).

---

## Deshacer Cambios
Es crucial entender la diferencia entre `reset` y `revert`. `reset` modifica el historial (peligroso en repositorios compartidos), mientras que `revert` crea un nuevo commit que deshace los cambios, manteniendo el historial intacto.

### Git Reset
`git reset` mueve el puntero `HEAD` y la rama actual a un commit anterior, "borrando" los commits posteriores del historial. El efecto en el working directory y staging depende del modo usado.
- `git reset --soft <id-commit>`
    - Mueve `HEAD` al commit especificado, pero deja todos los cambios de los commits "borrados" en el staging area (preparados para un nuevo commit).
- `git reset --mixed <id-commit>` (opción por defecto si no se especifica)
    - Mueve `HEAD` al commit especificado y limpia el staging area. Los cambios de los commits "borrados" se quedan en el working directory (como modificaciones sin trackear).
- `git reset --hard <id-commit>`
    - **PELIGROSO.** Mueve `HEAD` al commit especificado y borra **todo** (staging area y working directory). Los cambios posteriores a ese commit se pierden en el directorio local.
- `git reset HEAD <archivo>`
    - Saca un archivo del staging area (`unstage`). Es lo contrario de `git add`. Los cambios en el archivo se mantienen en el working directory.

### Git Revert
- `git revert <id-commit>`
    - Crea un **nuevo commit** que deshace todos los cambios introducidos por el commit especificado. Es la forma segura de deshacer cambios en repositorios compartidos, ya que no reescribe la historia.

### Git Commit --amend
- `git commit --amend`
    - Corrige el commit más reciente. Puede usarse para:
        1.  Añadir archivos que olvidaste incluir en el último commit (si ya los agregaste con `git add`).
        2.  Modificar el mensaje del último commit.
    - Básicamente, reemplaza el último commit por uno nuevo.

---

## Ramas (Branches) y Fusión (Merge)

### Gestión de Ramas (Branch, Checkout)
- `git branch`
    - Muestra la lista de ramas locales. La rama actual está marcada con un asterisco (`*`).
- `git branch <nombre-de-la-nueva-rama>`
    - Crea una nueva rama a partir del commit actual en el que te encuentras.
- `git checkout <nombre-de-la-rama>`
    - Cambia a la rama especificada. Actualiza el working directory para reflejar el estado de esa rama.
- `git checkout -b <nombre-de-la-nueva-rama>`
    - Crea una nueva rama y se cambia a ella en un solo paso.
- `git branch -d <nombre-de-la-rama>`
    - Elimina una rama que ya ha sido fusionada (merge). Usa `-D` para forzar el borrado aunque no se haya fusionado.
- `git branch -r`
    - Muestra todas las ramas remotas.
- `git branch -a`
    - Muestra todas las ramas, tanto locales como remotas.

### Fusionar Ramas (Merge)
- `git merge <nombre-de-la-rama-a-incorporar>`
    - Integra los cambios de la rama especificada (`<nombre-de-la-rama-a-incorporar>`) en la rama en la que te encuentras actualmente (por ejemplo, `main`).
    - **Proceso típico:**
        1.  `git checkout main` (te posicionas en la rama que recibirá los cambios).
        2.  `git merge cabecera` (fusionas la rama `cabecera` en `main`).
    - Si hay conflictos, Git los marcará en los archivos. Deberás resolverlos manualmente (o con ayuda de tu editor), luego hacer `git add` y finalmente `git commit` para completar el merge.

### Rebase
- `git rebase <nombre-de-la-rama-base>`
    - Reaplica los commits de la rama actual sobre la punta de otra rama. Reescribe la historia, creando commits nuevos. Se usa para mantener un historial lineal y limpio, especialmente en ramas locales.
    - **Uso común para actualizar una rama de característica con los cambios de `main`:**
        1.  `git checkout feature`
        2.  `git rebase main` (los commits de `feature` se aplican ahora después de los de `main`).
        3.  Luego, desde `main`, hacer un merge que será fast-forward: `git checkout main` y `git merge feature`.
    - **Advertencia:** No hagas `rebase` en ramas que otros desarrolladores estén usando. Es una operación que reescribe la historia y puede causar confusión.
- `git rebase --continue`
    - Después de resolver conflictos durante un rebase, continúa con el proceso.
- `git rebase --abort`
    - Aborta la operación de rebase en curso y vuelve al estado inicial.

---

## Trabajo con Repositorios Remotos

### Gestión de Remotos (Remote)
- `git remote add origin <url-del-repositorio>`
    - Vincula tu repositorio local con un repositorio remoto. Por convención, el remoto principal se llama `origin`.
    - Es recomendable usar la URL SSH para una comunicación más segura, sin necesidad de introducir usuario y contraseña cada vez.
- `git remote -v`
    - Muestra las URLs de los repositorios remotos configurados (para fetch y push).
- `git remote remove <nombre-del-remoto>` (ej. `origin`)
    - Elimina la asociación con un repositorio remoto.

### Subir y Bajar Cambios (Push, Pull)
- `git pull <nombre-remoto> <nombre-rama>` (ej. `git pull origin main`)
    - Descarga los cambios del repositorio remoto y los fusiona (`merge`) en la rama actual. Es equivalente a `git fetch` seguido de `git merge`.
- `git push <nombre-remoto> <nombre-rama>` (ej. `git push origin main`)
    - Envía los commits de tu rama local al repositorio remoto.
- `git push origin --tags`
    - Envía todas las etiquetas (tags) locales al repositorio remoto.

### Colaboración: Forks y Pull Requests
- **Fork:** No es un comando de Git, sino una acción en la interfaz de GitHub/GitLab. Crea una copia de un repositorio ajeno en tu cuenta.
- `git remote add upstream <url-del-repositorio-original>`
    - Después de clonar tu fork, añades el repositorio original como un remoto adicional (normalmente llamado `upstream`) para poder sincronizarte con sus cambios.
- `git pull upstream main`
    - Desde tu fork local, descargas y fusionas los cambios del repositorio original (`upstream/main`) para mantener tu copia actualizada.
- `git push origin main`
    - Subes los cambios (incluyendo los del `upstream` ya fusionados) a tu fork en GitHub.
- **Pull Request (PR):** Es una petición en GitHub/GitLab para que el dueño del repositorio original revise y fusione los cambios de tu fork.

---

## Comandos para Casos Especiales

### Tags (Etiquetas)
Los tags se usan para marcar puntos importantes en la historia, como versiones de lanzamiento (v1.0.0, v2.1.3).

- `git tag`
    - Lista todos los tags existentes.
- `git tag -a v1.0.0 -m "Mensaje de la versión" <id-commit>`
    - Crea un tag anotado (`-a`) en un commit específico.
- `git tag -d <nombre-del-tag>`
    - Elimina un tag de tu repositorio local.
- `git push origin :refs/tags/<nombre-del-tag>`
    - Elimina un tag del repositorio remoto.

### Git Stash (Guardado Temporal)
Guarda temporalmente los cambios no commiteados para limpiar el working directory, permitiéndote cambiar de rama o hacer otras operaciones.

- `git stash`
    - Guarda los cambios actuales (modificados y en staging) en una pila (stack) y revierte el working directory al último commit.
- `git stash list`
    - Muestra la lista de stashes guardados.
- `git stash pop`
    - Aplica los cambios del stash más reciente y lo elimina de la pila.
- `git stash apply`
    - Aplica los cambios del stash más reciente, pero lo mantiene en la pila.
- `git stash drop`
    - Elimina el stash más reciente.
- `git stash branch <nombre-rama>`
    - Crea una nueva rama a partir del commit donde se creó el stash y aplica los cambios del stash en ella. Útil si los cambios del stash generan conflictos en la rama actual.

### Git Clean (Limpiar Archivos no Trackeados)
- `git clean --dry-run`
    - Muestra una lista de los archivos no trackeados que serían eliminados, sin borrarlos realmente.
- `git clean -f`
    - Elimina forzosamente los archivos no trackeados.

### Git Cherry-pick (Seleccionar Commits)
- `git cherry-pick <id-commit>`
    - Aplica los cambios de un commit específico en la rama actual, creando un nuevo commit (con un ID diferente). Es útil para traer una corrección específica de una rama a otra sin fusionar toda la rama.

### Git Reflog (El Registro de Referencias)
- `git reflog`
    - Muestra un historial de dónde han apuntado `HEAD` y las ramas en tu repositorio local. Incluye commits, checkouts, resets, merges, etc. Es el "botón de deshacer" definitivo para operaciones locales.
- `git reset --hard HEAD@{<n>}` o `git reset --hard <id-commit>`
    - Después de encontrar el estado deseado en el `reflog`, puedes usar `git reset` para volver a ese punto, recuperando commits o ramas que creías perdidos.

---

## Búsqueda y Diagnóstico

### Buscar en Archivos (Grep)
- `git grep "<palabra>"`
    - Busca una palabra o expresión regular dentro de los archivos trackeados por Git.
- `git grep -n "<palabra>"`
    - Muestra también el número de línea donde aparece la coincidencia.
- `git grep -c "<palabra>"`
    - Cuenta el número de veces que aparece la palabra en cada archivo.

### Buscar en Commits (Log -S)
- `git log -S"<palabra>"`
    - Busca en el historial de commits aquellos que añadieron o eliminaron una cadena de texto específica en el código. (Es la "opción de la sopa de letras" o "pickaxe").

### Estadísticas y Autoría (Shortlog, Blame)
- `git shortlog -sn`
    - Muestra un resumen de la actividad, agrupando los commits por autor y ordenados por número de commits.
- `git shortlog -sn --all`
    - Incluye commits que están en ramas no visibles o que han sido eliminados.
- `git shortlog -sn --all --no-merges`
    - Muestra la misma información, pero excluyendo los commits de fusión (merge).
- `git blame <archivo>`
    - Muestra el archivo línea por línea, indicando el autor, el commit y la fecha de la última modificación de cada línea. Muy útil para saber quién introdujo un cambio específico.
- `git blame <archivo> -L <inicio>,<fin>`
    - Limita la salida de `git blame` a un rango de líneas específico (ej. `-L 35,50`).

---

## Alias y Personalización
Los alias permiten crear comandos personalizados para abreviar secuencias largas.

- `git config --global alias.<nombre-alias> "<comando git>"`
    - Crea un alias global. Ejemplo: `git config --global alias.tree "log --graph --oneline --all"`. Luego puedes usar `git tree`.
- Alias de shell (Bash/Zsh):
    - Se pueden crear en el archivo de configuración de la terminal (`.bashrc`, `.zshrc`). Ejemplo:
      `alias see-road="git log --graph --abbrev-commit --decorate --date=relative --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"`
      Este alias proporciona una vista muy detallada y colorida del historial de Git.

---

## Comandos Útiles Adicionales
- `git archive`
    - Crea un archivo comprimido (`.zip` o `.tar`) de los archivos de un commit o tag específico. Útil para distribuir una versión sin el historial de Git.
- `git bisect`
    - Herramienta avanzada para hacer una búsqueda binaria en el historial y encontrar el commit exacto que introdujo un bug.
- `git submodule`
    - Permite incluir y gestionar otros repositorios de Git dentro del tuyo propio.

---

## Historial de Comandos del Sistema
- `history`
    - Este no es un comando de Git, sino de la terminal (Linux/macOS/Git Bash). Muestra el historial de todos los comandos ejecutados recientemente en la terminal.
- `!<número>`
    - Vuelve a ejecutar el comando que aparece en esa posición en la lista de `history`. Ejemplo: `!23` ejecutará el comando número 23 del historial.
