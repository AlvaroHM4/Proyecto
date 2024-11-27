# Laboratorio: Introducción a CVS (Git)

## Objetivos
- Comprender los conceptos básicos de control de versiones.
- Aprender a utilizar Git para gestionar proyectos.

## Prerrequisitos
- Tener Git instalado en tu sistema.
- Conocimientos básicos de línea de comandos.

## Contenido

El material de lectura y de estudio es el libro [ProGit](https://git-scm.com/book/en/v2). La práctica es un índice y resumen de lo que se puede encontrar en ese libro.

Para el repaso de GIT elemental se plantea utilizar los recursos y los ejercicios propuestos en la Web [AprendeConAlf](https://aprendeconalf.es/docencia/git/)

### 0. Instalación de Git

Para instalar Git en tu sistema, sigue los pasos correspondientes a tu sistema operativo:

#### En Windows
1. Descarga el instalador de Git desde [git-scm.com](https://git-scm.com/download/win).
2. Ejecuta el instalador y sigue las instrucciones en pantalla. Asegúrate de seleccionar las opciones predeterminadas a menos que tengas una razón específica para cambiarlas.
3. Una vez completada la instalación, abre la terminal de Git Bash desde el menú de inicio.

#### En macOS
1. Abre la terminal.
2. Instala Git utilizando Homebrew (si no tienes Homebrew instalado, sigue las instrucciones en [brew.sh](https://brew.sh/)):
    ```bash
    brew install git
    ```
3. Verifica la instalación comprobando la versión de Git:
    ```bash
    git --version
    ```

#### En Linux
1. Abre la terminal.
2. Instala Git utilizando el gestor de paquetes de tu distribución. Por ejemplo, en Debian/Ubuntu:
    ```bash
    sudo apt-get update
    sudo apt-get install git
    ```
   En Fedora:
    ```bash
    sudo dnf install git
    ```
   En Arch Linux:
    ```bash
    sudo pacman -S git
    ```
3. Verifica la instalación comprobando la versión de Git:
    ```bash
    git --version
    ```

Una vez que hayas instalado Git, puedes proceder a la configuración inicial y a los demás pasos del laboratorio.


### 1. Configuración Inicial

Antes de comenzar a trabajar con Git, es importante configurar tu entorno para que los commits que realices estén correctamente identificados. Esta configuración solo necesita hacerse una vez por máquina.

#### Configuración del Nombre de Usuario y Correo Electrónico

Git utiliza tu nombre de usuario y correo electrónico para asociar los commits con tu identidad. Configura estos valores utilizando los siguientes comandos:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tuemail@example.com"
```

Puedes verificar la configuración actual con:

```bash
git config --global --list
```

#### Configuración del Editor de Texto

Git utiliza un editor de texto para que puedas escribir mensajes de commit. Puedes configurar tu editor preferido, por ejemplo, para usar `nano`:

```bash
git config --global core.editor nano
```

Para usar `vim`:

```bash
git config --global core.editor vim
```


#### Configuración de Alias

Los alias en Git te permiten crear comandos abreviados para tareas comunes. Por ejemplo, puedes crear un alias para `git status`:

```bash
git config --global alias.st status
```

Ahora, en lugar de escribir `git status`, puedes simplemente escribir `git st`.

#### Eliminar una configuración global

Para borrar una configuración global de Git, puedes usar el comando en el terminal:

```bash
git config --global --unset <nombre_de_la_configuración>
```

Por ejemplo, si deseas borrar la configuración global del alias podrímos usarlo del siguiente modo:

```bash
git config --global --unset alias.st
```

#### Configuración del Archivo `.gitignore`

El archivo `.gitignore` le dice a Git qué archivos o directorios debe ignorar en un proyecto. Crea un archivo `.gitignore` en el directorio raíz de tu proyecto y añade las rutas de los archivos que deseas ignorar. Por ejemplo:

```
# Ignorar archivos de configuración del sistema operativo
.DS_Store
Thumbs.db

# Ignorar archivos de compilación
/build
/dist

# Ignorar archivos de entorno
.env
```

#### Configuración de Credenciales

Para evitar tener que ingresar tu nombre de usuario y contraseña cada vez que interactúas con un repositorio remoto, puedes almacenar tus credenciales de manera segura. Puedes usar el siguiente comando para habilitar el almacenamiento en caché de credenciales:


```bash
git config --global credential.helper cache
```

Esto almacenará las credenciales en memoria durante 15 minutos (900 segundos) de forma predeterminada. Si deseas cambiar el tiempo de almacenamiento en caché, puedes especificar un tiempo en segundos usando el siguiente comando:

```bash
git config --global credential.helper 'cache --timeout=<segundos>'
```

Por ejemplo, para almacenar las credenciales en caché durante 1 hora (3600 segundos):

```bash
git config --global credential.helper 'cache --timeout=3600'
```




### 2. Creación de un Repositorio

1. Crea un nuevo directorio y navega a él:
    - Abre tu terminal.
    - Usa el comando `mkdir` para crear un nuevo directorio llamado `mi_proyecto` (en la práctica pon el siguiente código de nombre `CE_apellido_nombre`, es decir en mi caso sería: `CE_Caballero_Carlos`).
 
    ```bash
    mkdir CE_Caballero_Carlos
    cd CE_Caballero_Carlos
    ```

2. Inicializa un repositorio Git:
    - Asegúrate de estar en el directorio `CE_Caballero_Carlos`
    - Usa el comando `git init` para inicializar un nuevo repositorio Git en este directorio.

    ```bash
    git init
    ```

    **Explicación**:
    - `git init`: Este comando inicializa un nuevo repositorio Git en el directorio actual. Crea un subdirectorio oculto llamado `.git` que contiene todos los archivos necesarios para el control de versiones.
    
    **Resultado**:
    - Después de ejecutar este comando, tu directorio `CE_Caballero_Gonzalez` se convertirá en un repositorio Git. Puedes verificarlo listando los archivos ocultos con `ls -a` y viendo el directorio `.git`.

    ```bash
    ls -a
    ```

    Deberías ver una salida similar a esta:
    ```
    .  ..  .git
    ```

    Esto confirma que el repositorio Git ha sido inicializado correctamente.


### 3. Estados de un Fichero en Git

En Git, un fichero puede estar en uno de los siguientes estados: 

- **no rastreado (untracked)**.
- **modificado (modified)**.
- **preparado (staged)**.
- **confirmado (committed)**. 
  
A continuación, se explica cada uno de estos estados y cómo se transiciona entre ellos.

![Lifecycle Git](./L2/lifecycle-git.png)

*Figura 1: Ciclo de vida de Git*

#### 1. No Rastreado (Untracked)
Un fichero no rastreado es aquel que existe en el directorio de trabajo pero no está siendo seguido por Git. Esto significa que Git no tiene información sobre este fichero y no lo incluirá en los commits.

**Ejemplo**:
```bash
echo "Nuevo archivo" > nuevo.txt
git status
```

El comando `git status` mostrará que `nuevo.txt` está no rastreado.

#### 2. Modificado (Modified)
Un fichero modificado es aquel que ha sido cambiado en el directorio de trabajo pero no ha sido añadido al área de preparación. Git sabe que el fichero ha cambiado porque lo está rastreando, pero los cambios aún no están listos para ser confirmados.

**Ejemplo**:
```bash
echo "Cambio en el archivo" >> hola.txt
git status
```

El comando `git status` mostrará que `hola.txt` ha sido modificado.

#### 3. Preparado (Staged)
Un fichero preparado es aquel que ha sido añadido al área de preparación. Esto significa que los cambios en este fichero están listos para ser incluidos en el próximo commit.

**Ejemplo**:
```bash
git add hola.txt
git status
```
El comando `git status` mostrará que `hola.txt` está en el área de preparación.

#### 4. Confirmado (Committed)
Un fichero confirmado es aquel cuyos cambios han sido guardados en el repositorio de Git. Esto significa que los cambios están ahora en el historial de commits y pueden ser recuperados en cualquier momento.

**Ejemplo**:
```bash
git commit -m "Actualizar hola.txt"
git status
```
El comando `git status` mostrará que no hay cambios pendientes, ya que todos los cambios han sido confirmados.

### Transiciones entre Estados

1. **De No Rastreado a Preparado**:
    ```bash
    git add nuevo.txt
    ```
    Este comando añade el fichero `nuevo.txt` al área de preparación.

2. **De Modificado a Preparado**:
    ```bash
    git add hola.txt
    ```
    Este comando añade el fichero `hola.txt` modificado al área de preparación.

3. **De Preparado a Confirmado**:
    ```bash
    git commit -m "Mensaje de commit"
    ```
    Este comando confirma los cambios preparados con un mensaje descriptivo.

4. **De Confirmado a Modificado**:
    Simplemente edita el fichero:
    ```bash
    echo "Otro cambio" >> hola.txt
    git status
    ```
    El comando `git status` mostrará que `hola.txt` ha sido modificado nuevamente.

### Resumen de Comandos
- **git status**: Muestra el estado de los ficheros en el repositorio.
- **git add <archivo>**: Añade un fichero al área de preparación.
- **git commit -m "mensaje"**: Confirma los cambios preparados con un mensaje descriptivo.

Estos comandos y estados son fundamentales para trabajar eficientemente con Git y mantener un historial claro y organizado de los cambios en tu proyecto.


### 6. Ramas en Git

La gestión de ramas se basa en el contenido del libro [ProGit]
(https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)

### 7. Breve Paseo por GitHub: Crear mi Primer Repositorio Remoto y Usarlo en Local

Crear cuenta gratuita en [GitHub](https://github.com)

- Navegando vía web, conozca proyectos de repositorios públicos como:
  - Proyecto del [Kernel de Linux](https://github.com/torvalds/linux)
  - Proyecto de [Angular](https://github.com/angular/angular)
  - [WordPress](https://github.com/WordPress/WordPress), etc.

- Compruebe:
  - Ficheros que existen.
  - Ramas que tienen.
  - Tags.
  - Quién contribuye en este proyecto.
  - Estadística de cada uno de los lenguajes de programación usados.
  - Etc.

#### Crear Repositorio `Practica1Git` para practicar

- Enlace del repositorio: [https://github.com/politecnicoDAW-2024/pps-repositorio-demo-git](https://github.com/politecnicoDAW-2024/pps-repositorio-demo-git)

Pasos:
1. Añadir un archivo `README.md` para explicar para qué sirve este proyecto.
2. Analizar y crear el fichero `.gitignore`.
3. Añadir varios ficheros desde el repositorio.
4. Inicializar/Clonar el proyecto en el repositorio local de su ordenador.
   - Comprobar que existe una única rama.
   - Comprobar que los ficheros se han descargado.

#### ¿Qué ha sucedido en el directorio oculto `.git` de cada uno de los proyectos?
Ejecutar el siguiente comando:
```bash
ls -alR
```

**Importante para los alumnos:**

> “Si se utiliza Git de forma normal sin tratar de comprender los mecanismos internos, un usuario nunca tendrá la necesidad de ir a esta carpeta. De hecho, la mayoría de los programadores no saben ni que existe .git”.

Comprobar que se ha añadido información que relaciona un alias (origin) con el repositorio remoto  en el fichero `.git/config`. Usaremos este alias (el mismo en todos los repositorios) para no tener que poner la ruta completa de la URL en cuestión.

1. Crear nuestro primer fichero del proyecto
2. Realizar seguimiento del archivo:
```
git add .
```
3. Hacer un commit:
```
git commit -m "Mensaje del commit"
```
4. Subirlo al repositorio remoto:
```
git push origin main
```

Evitar que nos pidan más el usuario y la contraseña cada vez que hagamos uso del repositorio remoto
Opciones:

Para almacenar las credenciales para siempre:
```
git config --global credential.helper store
```
Para almacenarlas por un tiempo limitado (ej. 1 hora):

```
git config --global credential.helper 'cache --timeout=3600'
```

5. Probar con otro repositorio remoto
Comprobar que existen muchas ramas, commits, tags, archivos, etc., no como en nuestro repositorio “vacío”.

### 8. HASH

#### ¿Qué es?

Verificar que un archivo no es corrupto (descarga, git, etc.). También se usa para la autenticación de usuarios sin tener que almacenar su contraseña en texto plano. Existen diferentes mecanismos como:

- MD4
- MD5
- SHA1
- SHA256
- SHA512
- CRC

##### Ejemplo con `sha1sum`

Para su comprensión, probemos con `sha1sum` (40 caracteres), que guarda:

```bash
echo -n "Git" | sha1sum
echo -n "Pepe" | sha1sum
echo -n "Este es un código de Carlos Caballero que ha encontrado un fallo en ese fichero, y lo va a arreglar" | sha1sum
```

En la actualidad se usan otros tipos de hash, pero para su comprensión es suficiente.

##### ¿Cómo lo usa Git?
Git utiliza pares clave/valor por cada fichero (contenido y su hash) para detectar rápidamente cuando un fichero ha sido modificado por un programador.

##### Riesgo de Colisión
El riesgo de colisión es improbable, incluso en proyectos muy grandes.


Aquí tienes el texto en formato Markdown, complementado con más ejemplos y explicaciones sobre el comando `reset` en Git:

### 9. Restaurando Ficheros del Repositorio (Reset)

#### ¿Qué es `HEAD`?

En Git, `HEAD` es el alias del commit que apunta a la rama seleccionada actualmente en el repositorio. `HEAD` tiene un hash como cualquier commit, pero debido a que se usa frecuentemente, se le asigna este alias para referirse al último commit de la rama activa.

#### ¿Por qué es importante?

Es importante entender qué es `HEAD` para evitar borrar o modificar cosas por accidente. Usar correctamente los comandos de Git que interactúan con `HEAD` nos permite manejar de forma segura las restauraciones o cambios en el historial.

#### Subir varios ficheros al repositorio

Antes de restaurar ficheros, es necesario entender los siguientes pasos comunes en Git:

1. **Añadir archivos al área de preparación (staging):**
   ```bash
   git add archivo.txt
   ```

2. **Hacer commit de los cambios en el repositorio:**
   ```bash
   git commit -m "Mensaje del commit"
   ```

Una vez que hemos subido varios ficheros al repositorio, podemos decidir volver a versiones anteriores, ya sea de commits completos o de ficheros individuales.

#### Restaurar commits o ficheros con `git reset`

##### Resetear al área de staging (index)

El comando `git reset` se usa para mover el `HEAD` a un commit anterior o para restaurar un fichero en específico. Se puede usar de diferentes formas:

- Para resetear el repositorio al estado de un commit anterior en el área de staging:
   ```bash
   git reset [commit]
   ```
   Esto no modifica los archivos en el directorio de trabajo, solo mueve el estado de los archivos en staging (index) al commit seleccionado.

- Para resetear un archivo específico al estado del commit anterior en staging:
   ```bash
   git reset archivo.txt
   ```

##### Resetear el directorio de trabajo (con `--hard`)

Si se quiere restaurar no solo el staging, sino también el directorio de trabajo, se utiliza la opción `--hard`:

- Para resetear todo el proyecto a un commit anterior, incluyendo el directorio de trabajo:
   ```bash
   git reset [commit] --hard
   ```

- Para resetear un archivo específico directamente en el directorio de trabajo:
   ```bash
   git reset --hard archivo.txt
   ```

##### Ejemplo completo

Supongamos que hemos subido varios ficheros al repositorio y queremos volver a una versión anterior de nuestro proyecto:

1. **Añadir archivos:**
   ```bash
   git add index.php style.css
   ```

2. **Hacer commit:**
   ```bash
   git commit -m "Añadido archivo index.php y estilo"
   ```

3. **Resetear al commit anterior, pero manteniendo los cambios en el área de staging:**
   ```bash
   git reset [commit]
   ```

4. **Resetear completamente el proyecto a un commit anterior, eliminando todos los cambios no guardados:**
   ```bash
   git reset [commit] --hard
   ```

##### Diferencias entre `git reset` y `git revert`

Mientras que `git reset` cambia el historial del repositorio, `git revert` es una forma más segura de deshacer cambios, ya que crea un nuevo commit que deshace las modificaciones de un commit anterior sin cambiar el historial existente. Ejemplo de `git revert`:

```bash
git revert [commit]
```

Esto es útil en situaciones en las que quieres deshacer un commit, pero mantener el historial de commits intacto. No obstante, es peligroso si queremos borrar credenciales. En ese caso es mejor usar `git reset`.

##### Conclusión

El comando `git reset` es muy poderoso y puede ser peligroso si no se usa con cuidado. Es importante saber cuándo usarlo y cómo afecta el estado del repositorio y del directorio de trabajo. Recuerda siempre hacer un respaldo de tu trabajo antes de realizar acciones destructivas como `git reset --hard`.


Aquí está el contenido convertido a Markdown y ampliado, dividido en partes para mejor comprensión:

---

### 10. Observando Nuestro Repositorio con Git: `status`, `log`, `show`, `blame`, `diff`

Para comprender mejor los comandos que vamos a utilizar, puede ser útil trabajar con un proyecto de tamaño considerable, por ejemplo, un proyecto de código abierto descargado de Internet. Con un proyecto más grande, es más fácil observar cambios, ramificaciones y commits en diferentes momentos del desarrollo.

#### 1. `git status`: Observando el Estado de los Archivos

El comando `git status` permite ver los archivos que están en la zona de **staging** o aquellos que han sido modificados pero aún no añadidos al staging.

```bash
git status
```

Este comando te ofrece información como:

- Archivos modificados.
- Archivos que están listos para ser confirmados (en el staging).
- Archivos no seguidos por Git.
  
Es especialmente útil para decidir qué archivos deben ser confirmados o descartados antes de realizar un commit.

#### 2. `git log`: Explorando el Historial de Commits

El comando `git log` muestra el historial de commits en la rama actual. Los commits se muestran en orden descendente, del más reciente al más antiguo, y proporciona detalles como:

- **Título** del commit.
- **Hash** del commit (importante para trabajar con identificaciones precisas).
- **Autor** y **fecha** del commit.

##### Ejemplos Útiles de `git log`

- Ver los últimos 10 commits:

  ```bash
  git log -10
  ```

- Mostrar los últimos 10 commits con los cambios realizados en cada uno:

  ```bash
  git log -10 -p
  ```

  Esto te permite ver las diferencias específicas (líneas añadidas, eliminadas, etc.) en cada commit.

- Ver una versión condensada del historial, mostrando solo los títulos de los commits y una representación gráfica de las ramificaciones del proyecto:

  ```bash
  git log --oneline --graph --decorate -10
  ```

  La opción `--graph` es útil para visualizar las fusiones (*merges*) y bifurcaciones del proyecto.

- Mostrar estadísticas detalladas sobre los cambios en un commit (líneas añadidas, eliminadas, archivos modificados, etc.):

  ```bash
  git log -1 --stat
  ```

- Filtrar commits entre fechas específicas:

  ```bash
  git log --oneline --before="2019-12-12" --after="2019-12-01"
  ```

- Ver el historial de cambios para un archivo específico (ej. `index.php`):

  ```bash
  git log -3 index.php
  ```

##### Exploración del Historial en Varias Ramas

Es fundamental usar la opción `--graph` para observar cómo han evolucionado los commits en diferentes ramas:

```bash
git log --graph --all
```

Esto te permite ver la cronología de los commits en todas las ramas del proyecto.


#### 3. `git show`: Explorando un Commit o Archivo Específico

El comando `git show` te permite ver los detalles de un commit específico o de un archivo modificado en un commit. Es útil cuando quieres ver exactamente qué cambió en un commit determinado.

- Ver los cambios de un commit completo:

  ```bash
  git show [hash del commit]
  ```

- Ver los cambios de un archivo en particular en un commit:

  ```bash
  git show [hash del commit]:[archivo]
  ```

#### 4. `git diff`: Comparando Diferencias entre Estados

El comando `git diff` se usa para comparar diferencias entre distintos estados del proyecto, como el **directorio de trabajo**, el **índice** (staging), y los **commits** en el repositorio local.

##### Comparaciones Comunes con `git diff`

- Ver diferencias entre el **directorio de trabajo** y el **staging** (archivos modificados pero no añadidos a staging):

  ```bash
  git diff
  ```

- Ver diferencias entre el **directorio de trabajo** y `HEAD` (la última versión en el repositorio):

  ```bash
  git diff HEAD
  ```

- Comparar el **staging** con el último commit (`HEAD`):

  ```bash
  git diff --cached
  ```

  Esto es útil cuando has añadido cambios al staging (`git add .`) y quieres ver la diferencia con el último commit.

- Comparar dos commits específicos:

  ```bash
  git diff [commit1] [commit2]
  ```

- Comparar dos ramas:

  ```bash
  git diff [rama1] [rama2]
  ```

- Comparar un archivo en el **directorio de trabajo** con su versión en `HEAD`:

  ```bash
  git diff -r HEAD archivo.txt
  ```

#### 5. `git blame`: Identificando Autores y Fechas de Cambios

El comando `git blame` te permite ver quién cambió una línea específica en un archivo y cuándo lo hizo. Es muy útil para rastrear la historia de un archivo y entender la razón detrás de ciertos cambios.

- Ver quién ha modificado un archivo y en qué líneas:

  ```bash
  git blame archivo.txt
  ```

Aquí tienes el contenido convertido a Markdown y ampliado:

---

### 11. Uso de Tags en Git


Los **tags** son extremadamente útiles para:

- **Versionado del software**: Te permiten identificar fácilmente las versiones del software, lo cual es esencial para el desarrollo continuo, la entrega de versiones estables, y el mantenimiento de la compatibilidad.
- **Lanzamientos de versiones**: En muchos proyectos, los tags se utilizan para marcar los puntos en los que una nueva versión del software está lista para ser lanzada.
- **Facilitar la navegación**: En lugar de recordar un hash largo y complejo, los tags proporcionan una manera sencilla de navegar y referenciar puntos clave en el historial de un proyecto.


#### ¿Qué es un Tag?

Un **tag** en Git es un puntero inmutable a un commit específico. Puedes imaginarlo como una "etiqueta" que se le asigna a un commit. Una vez creado, ese tag no cambia, lo que te permite hacer referencia a un commit específico por su nombre en lugar de por su hash, lo cual es más conveniente, especialmente cuando trabajas con versiones liberadas de software.

Los tags son ampliamente usados para **versionar** programas y librerías siguiendo un esquema de versiones **semánticas** en el formato `x.y.z`, donde:

- **x**: Indica una versión mayor. Se usa cuando hay cambios significativos en el proyecto que pueden romper la compatibilidad con versiones anteriores. Normalmente, estos cambios ocurren en la rama `master` o `main`.
- **y**: Indica una versión menor. Se utiliza cuando se añade nueva funcionalidad sin romper la compatibilidad con versiones anteriores.
- **z**: Indica una versión de parches o correcciones de bugs.

#### Ejemplo de Versionado Semántico:

- `1.0.0`: Primera versión estable.
- `1.1.0`: Se ha añadido una nueva funcionalidad.
- `1.1.1`: Se ha corregido un bug, pero no se añade funcionalidad nueva.

#### Crear un Tag

Puedes crear un tag en cualquier momento para identificar una versión específica del código. Existen dos formas comunes de hacerlo:

##### 1. Tag en el Commit Actual

Para crear un **tag** en el commit actual (el último commit que has realizado en la rama en la que estás), puedes usar el siguiente comando:

```bash
git tag 2.0.0
```

En este caso, `2.0.0` es el nombre del tag que estamos creando. Este tag representará el estado actual del repositorio.

##### 2. Tag en un Commit Específico

Si deseas poner un **tag** en un commit anterior, puedes hacerlo especificando el hash del commit:

```bash
git tag 2.1.0 [hash del commit]
```

Esto es útil cuando quieres etiquetar un commit antiguo, por ejemplo, cuando olvidaste poner un tag en una versión anterior del proyecto.


##### Listar Tags

Para ver todos los **tags** que has creado en el repositorio, puedes usar el siguiente comando:

```bash
git tag -l
```

Este comando lista todos los tags en el repositorio, de una manera similar a cómo `git log` muestra los commits, pero en este caso solo te mostrará los tags con información más limitada.


##### Subir Tags al Repositorio Remoto

Una vez que has creado tags en tu repositorio local, puedes subirlos al repositorio remoto (por ejemplo, GitHub). Hay dos formas de hacerlo:

###### 1. Subir Todos los Tags

Si quieres enviar todos los tags que has creado al repositorio remoto, puedes usar este comando:

```bash
git push origin --tags
```

Este comando enviará todos los tags locales que aún no estén en el repositorio remoto. Es especialmente útil cuando creas múltiples tags de una sola vez.

###### 2. Subir un Tag Específico

Si prefieres subir solo un tag en particular, puedes hacerlo especificando el nombre del tag:

```bash
git push origin <nombre_tag>
```

Por ejemplo, para subir el tag `2.0.0`, puedes usar:

```bash
git push origin 2.0.0
```


#### Ejemplo Práctico

##### Crear y Subir un Tag:

1. **Realizas un commit importante**, por ejemplo, finalizas una nueva funcionalidad:

    ```bash
    git commit -m "Añadida nueva funcionalidad X"
    ```

2. **Creas un tag** que identifique este commit como una nueva versión (por ejemplo, la versión 1.2.0):

    ```bash
    git tag 1.2.0
    ```

3. **Subes el tag al repositorio remoto**:

    ```bash
    git push origin 1.2.0
    ```

Ahora, en GitHub (o cualquier otro servidor Git), podrás ver que tu repositorio tiene un tag `1.2.0`, indicando esa versión específica del proyecto.

##### ¿Cómo se ven los Tags en GitHub?

En GitHub, los **tags** se pueden ver en la pestaña "Tags" o "Releases" del repositorio. Cuando se hace clic en un tag, GitHub muestra el estado del código en ese punto, permitiendo que otros puedan descargar la versión específica del proyecto en la que ese tag fue creado.

Además, GitHub permite **crear lanzamientos** (releases) asociados con tags, lo que es ideal para proyectos de software en los que se liberan versiones formales del código. Al crear un **release**, puedes asociar un changelog, notas de la versión, y hasta archivos binarios relacionados.


### 12. Git Hooks

Git Hooks son scripts que Git ejecuta automáticamente antes o después de ciertos eventos, como commits, pushes y merges. Estos hooks permiten automatizar tareas y personalizar el flujo de trabajo de Git. Los hooks se configuran a nivel de repositorio y se encuentran en el directorio `.git/hooks`.

#### Tipos de Git Hooks

Existen dos tipos principales de hooks en Git:

1. **Hooks del Lado del Cliente (Client-Side Hooks)**:
    - **pre-commit**: Se ejecuta antes de que se realice un commit. Se utiliza para verificar el código (por ejemplo, ejecutar linters o pruebas).
    - **prepare-commit-msg**: Se ejecuta antes de que el mensaje de commit sea editado. Se puede usar para modificar el mensaje de commit predeterminado.
    - **commit-msg**: Se ejecuta después de que el mensaje de commit ha sido editado. Se utiliza para validar el mensaje de commit.
    - **post-commit**: Se ejecuta después de que se ha realizado un commit. Se puede usar para notificaciones o tareas de registro.

2. **Hooks del Lado del Servidor (Server-Side Hooks)**:
    - **pre-receive**: Se ejecuta antes de que se acepte un push en el servidor. Se utiliza para validar los cambios entrantes.
    - **update**: Similar a `pre-receive`, pero se ejecuta una vez por cada rama que se está actualizando.
    - **post-receive**: Se ejecuta después de que se ha aceptado un push. Se puede usar para desplegar código o notificar a otros sistemas.

#### Configuración de Git Hooks

Para configurar un hook, simplemente crea un archivo en el directorio `.git/hooks` con el nombre del hook y hazlo ejecutable. Aquí tienes un ejemplo de cómo configurar un hook `pre-commit` que ejecuta un linter antes de cada commit:

1. Navega al directorio de hooks:
    ```bash
    cd .git/hooks
    ```

2. Crea un archivo llamado `pre-commit`:
    ```bash
    touch pre-commit
    ```

3. Haz que el archivo sea ejecutable:
    ```bash
    chmod +x pre-commit
    ```

4. Edita el archivo `pre-commit` para que ejecute el linter:
```bash
#!/bin/sh
# Ejecutar linter
eslint . || {
    echo "Linter errors, aborting commit."
    exit 1
}
```

#### Ejemplo de Hook `pre-commit`

Aquí tienes un ejemplo más detallado de un hook `pre-commit` que verifica si hay archivos con errores de sintaxis antes de permitir un commit:

```bash
#!/bin/sh
# Hook pre-commit para verificar errores de sintaxis

# Verificar archivos Python
for file in $(git diff --cached --name-only | grep '\.py$'); do
    python -m py_compile $file
    if [ $? -ne 0 ]; then
        echo "Errores de sintaxis en $file. Abortando commit."
        exit 1
    fi
done

# Verificar archivos JavaScript
for file in $(git diff --cached --name-only | grep '\.js$'); do
    eslint $file
    if [ $? -ne 0 ]; then
        echo "Errores de linter en $file. Abortando commit."
        exit 1
    fi
done

echo "Todos los archivos están correctos. Procediendo con el commit."
```

### Husky && CommitLint

Husky y CommitLint son herramientas que ayudan a mantener la calidad del código y la consistencia en los mensajes de commit. Husky permite ejecutar scripts de Git Hooks de manera fácil y configurable, mientras que CommitLint valida los mensajes de commit según reglas definidas.

#### Instalación de Husky y CommitLint

Primero, necesitas instalar Husky y CommitLint en tu proyecto. Asegúrate de tener Node.js y npm instalados.

1. Instala Husky:
    ```bash
    npm install husky --save-dev
    ```

2. Instala CommitLint y su configuración convencional:
    ```bash
    npm install @commitlint/{config-conventional,cli} --save-dev
    ```

#### Configuración de Husky

1. Añade un script de Husky en tu `package.json` para inicializar Husky:
    ```json
    {
      "scripts": {
        "prepare": "husky install"
      }
    }
    ```

2. Ejecuta el script para inicializar Husky:
    ```bash
    npm run prepare
    ```

3. Configura Husky para usar un hook `commit-msg` que ejecute CommitLint:
    ```bash
    npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
    ```

#### Configuración de CommitLint

1. Crea un archivo de configuración para CommitLint llamado `commitlint.config.js` en la raíz de tu proyecto:
    ```javascript
    // commitlint.config.js
    module.exports = {
      extends: ['@commitlint/config-conventional'],
      rules: {
        'type-enum': [
          2,
          'always',
          [
            'build',
            'chore',
            'ci',
            'docs',
            'feat',
            'fix',
            'perf',
            'refactor',
            'revert',
            'style',
            'test'
          ]
        ],
        'subject-case': [0, 'never', ['sentence-case', 'start-case', 'pascal-case', 'upper-case']]
      }
    };
    ```

#### Ejemplo de Mensaje de Commit

CommitLint validará los mensajes de commit según las reglas definidas. Un mensaje de commit válido podría ser:

```bash
feat(entorno): añadir nueva funcionalidad de autenticación
```

Si intentas hacer un commit con un mensaje que no sigue las reglas, CommitLint lo rechazará.

## Securización de Git/GitHub
`git-secrets` es una herramienta desarrollada por AWS Labs que ayuda a prevenir la inclusión accidental de secretos (como claves de API, contraseñas y otros datos sensibles) en los repositorios de Git. Funciona escaneando los commits, las ramas y los hooks de Git en busca de patrones que coincidan con secretos conocidos y bloquea los commits que contienen estos secretos.

### Características principales de `git-secrets`:

- **Prevención de commits**: Bloquea los commits que contienen secretos conocidos.
- **Hooks de Git**: Se integra con los hooks de Git para escanear automáticamente los commits antes de que se realicen.
- **Patrones personalizados**: Permite definir patrones personalizados para buscar secretos específicos.
- **Compatibilidad**: Funciona en sistemas Unix y Windows.

### Instalación
Para instalar `git-secrets`, puedes usar `brew` en macOS o clonar el repositorio y ejecutar el script de instalación en otros sistemas.

#### macOS (usando Homebrew):
```bash
brew install git-secrets
```

#### Linux/Windows:
```bash
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
sudo make install
```

### Uso
1. **Inicializar `git-secrets` en un repositorio**:
    ```bash
    git secrets --install
    ```

2. **Añadir patrones de AWS** (opcional, pero recomendado si usas servicios de AWS):
    ```bash
    git secrets --register-aws
    ```

3. **Añadir patrones personalizados**:
    ```bash
    git secrets --add '<tu_patron>'
    ```

4. **Escanear el repositorio en busca de secretos**:
    ```bash
    git secrets --scan
    ```

### Ejemplo de uso
1. Inicializa `git-secrets` en tu repositorio:
    ```bash
    git secrets --install
    ```

2. Registra patrones de AWS:
    ```bash
    git secrets --register-aws
    ```

3. Añade un patrón personalizado para buscar una clave API específica:
    ```bash
    git secrets --add 'AKIA[0-9A-Z]{16}'
    ```

4. Escanea el repositorio en busca de secretos:
    ```bash
    git secrets --scan
    ```

Con `git-secrets`, puedes asegurarte de que no se filtren datos sensibles en tu repositorio, mejorando así la seguridad de tu código y tus datos.


### Ejemplo de git-secrets

Aquí tienes un ejemplo completo de un script que configura `git-secrets` e integra sus verificaciones con los hooks de Git:

### Script de Configuración

```bash
#!/bin/bash

# Inicializa git-secrets en el repositorio
git secrets --install

# Registra patrones de AWS
git secrets --register-aws

# Añade un patrón personalizado para buscar una clave API específica
git secrets --add 'AKIA[0-9A-Z]{16}'

# Añade un patrón personalizado

 para

 buscar contraseñas comunes
git secrets --add 'password\s*=\s*["\'][^"\']*["\']'

# Configura los hooks de pre-commit, commit-msg y pre-push para usar git-secrets
git secrets --install -f -t pre-commit
git secrets --install -f -t commit-msg
git secrets --install -f -t pre-push

echo "git-secrets ha sido configurado con éxito y los hooks han sido instalados."
```

### Explicación del Script

1. **Inicializa `git-secrets` en el repositorio**:
    ```bash
    git secrets --install
    ```

2. **Registra patrones de AWS**:
    ```bash
    git secrets --register-aws
    ```

3. **Añade patrones personalizados**:
    - Para buscar una clave API específica:
        ```bash
        git secrets --add 'AKIA[0-9A-Z]{16}'
        ```
    - Para buscar contraseñas comunes:
        ```bash
        git secrets --add 'password\s*=\s*["\'][^"\']*["\']'
        ```

4. **Configura los hooks de Git**:
    - Instala el hook de `pre-commit`:
        ```bash
        git secrets --install -f -t pre-commit
        ```
    - Instala el hook de `commit-msg`:
        ```bash
        git secrets --install -f -t commit-msg
        ```
    - Instala el hook de `pre-push`:
        ```bash
        git secrets --install -f -t pre-push
        ```

5. **Mensaje de éxito**:
    ```bash
    echo "git-secrets ha sido configurado con éxito y los hooks han sido instalados."
    ```

### Uso del Script

Guarda el script en un archivo, por ejemplo `configurar-git-secrets.sh`, y dale permisos de ejecución:

```bash
chmod +x configurar-git-secrets.sh
```

Luego, ejecuta el script desde la raíz de tu repositorio:

```bash
./configurar-git-secrets.sh
```

Este script configurará `git-secrets` en tu repositorio, registrará patrones de AWS y personalizados, y configurará los hooks de Git para que `git-secrets` verifique automáticamente los commits, mensajes de commit y pushes.

### trufflehog: Asegurando secretos

En esta sección trabajaremos con la herramienta [trufflehog](https://github.com/trufflesecurity/trufflehog)-


#### 1.**Introducción a Trufflehog**

##### ¿Qué es Trufflehog?

Trufflehog es una herramienta de código abierto diseñada para buscar secretos expuestos, como claves de API, tokens de autenticación y contraseñas, en repositorios de código, sistemas de control de versiones y otras fuentes de datos. Esta herramienta fue creada para ayudar a los desarrolladores y equipos de seguridad a identificar de manera rápida y eficiente posibles fugas de información sensible dentro de su código o historial de commits.

Trufflehog funciona escaneando archivos y repositorios en busca de patrones comunes de secretos y puede analizar no solo el contenido actual, sino también el historial completo de un repositorio, buscando cambios pasados que puedan haber expuesto información confidencial en algún momento.

##### ¿Por qué usar Trufflehog?

- **Detección automática de secretos**: En proyectos grandes, es fácil que se filtren accidentalmente secretos en el código. Trufflehog ayuda a identificar esas fugas de forma automática y rápida.
- **Análisis profundo del historial**: No solo revisa el código actual, sino también el historial de commits, lo que permite encontrar secretos que pudieron haber sido expuestos en versiones anteriores y que podrían ser vulnerables.
- **Integración en pipelines CI/CD**: Al integrarse en pipelines de integración continua, Trufflehog permite detectar y prevenir la inclusión de secretos desde las primeras fases del ciclo de desarrollo.
- **Ahorro de tiempo**: En lugar de revisar manualmente miles de líneas de código, Trufflehog automatiza el proceso, permitiendo a los equipos centrarse en la solución de problemas y mejoras de seguridad.
- **Mitigación de riesgos**: Identificar y eliminar secretos del código reduce significativamente el riesgo de que esos secretos sean explotados por atacantes, especialmente en repositorios públicos.

Trufflehog es particularmente útil en entornos de desarrollo colaborativo donde múltiples desarrolladores trabajan en un mismo proyecto, asegurando que ningún secreto quede expuesto en el código fuente.



#### 2. **Instalación de trufflehog**
 
 Para instalar la herramienta consultemos la documentación oficial donde se plantean diferentes modos de instalarla.

 [https://github.com/trufflesecurity/trufflehog?tab=readme-ov-file#floppy_disk-installation](https://github.com/trufflesecurity/trufflehog?tab=readme-ov-file#floppy_disk-installation)

#### 3. **Configuración de trufflehog**

  - **Configuración básica**
    
    Una vez que trufflehog está instalado, puedes comenzar a usarlo con configuraciones básicas para escanear tus repositorios en busca de secretos. Aquí hay algunos ejemplos de cómo configurar y ejecutar trufflehog:

    1. **Escaneo de un repositorio local**:
   
        ```bash
        trufflehog filesystem --path /ruta/a/tu/repositorio
        ```
        Este comando escaneará el repositorio local ubicado en `/ruta/a/tu/repositorio` en busca de posibles secretos.

    1. **Escaneo de un repositorio remoto**:
        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git
        ```

        Este comando escaneará el repositorio remoto especificado en la URL en busca de posibles secretos.

    2. **Especificar patrones de búsqueda**:
        Puedes especificar patrones de búsqueda personalizados utilizando el parámetro `--rules`:

        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git --rules /ruta/a/tus/reglas.json
        ```

        El archivo `reglas.json` debe contener las expresiones regulares que deseas utilizar para buscar secretos.

 - **Configuración avanzada**
  
    Para configuraciones más avanzadas, trufflehog ofrece varias opciones y parámetros que puedes ajustar según tus necesidades:

    1. **Especificar profundidad de escaneo**:
        Puedes limitar la profundidad del escaneo en el historial de commits utilizando el parámetro `--depth`:
        
        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git --depth 50
        ```
        
        Este comando escaneará solo los últimos 50 commits del repositorio.

    2. **Excluir archivos o directorios específicos**:
            
        Puedes excluir ciertos archivos o directorios del escaneo utilizando el parámetro `--exclude-paths`:
            
        ```bash
        trufflehog filesystem --path /ruta/a/tu/repositorio --exclude-paths "node_modules,tests"
        ```
        
        Este comando excluirá los directorios `node_modules` y `tests` del escaneo.

     3. **Salida en formato JSON**:
        Puedes obtener los resultados del escaneo en formato JSON utilizando el parámetro `--json`:
        
        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git --json > resultados.json
        ```
        
        Este comando guardará los resultados del escaneo en un archivo llamado `resultados.json`.

        1. **Uso de credenciales para repositorios privados**:
            Si necesitas escanear un repositorio privado, puedes agregar la clave SSH a tu cuenta de GitHub.


#### 4. **Integración de trufflehog con Git Hooks**


##### 1. Configuración de Hooks Pre-Commit

La configuración de un hook **pre-commit** en Git es útil para asegurar que ningún secreto sea añadido al código antes de que se haga un commit. A continuación, se muestra cómo configurar Trufflehog como un hook pre-commit:

1. **Crear el archivo del hook pre-commit:**
   En el directorio de tu proyecto, crea o edita el archivo `pre-commit` en la carpeta `.git/hooks/`:

   ```bash
   touch .git/hooks/pre-commit
   chmod +x .git/hooks/pre-commit
   ```

2. Agregar el script de Trufflehog: Añade el siguiente script al archivo pre-commit para que Trufflehog escanee los cambios antes de permitir el commit:

```bash
#!/bin/bash

echo "Running Trufflehog pre-commit hook..."

# Escanea los cambios que están por ser comiteados
git diff --cached --name-only | xargs trufflehog filesystem

# Captura el código de salida de Trufflehog
RESULT=$?

if [ $RESULT -ne 0 ]; then
    echo "Trufflehog found potential secrets. Commit aborted."
    exit 1
fi

echo "No secrets found. Proceeding with commit."
exit 0
```

ste script escanea los archivos modificados que están listos para el commit (`--cached`) y, si Trufflehog encuentra algún secreto, aborta el proceso del commit.

##### 2. Configuración de Hooks Pre-Push

El hook pre-push en Git es útil para asegurarse de que no se envían secretos al repositorio remoto. Este hook se ejecuta antes de que se realice un push, permitiendo escanear el código que está a punto de ser subido.

1. Crear el archivo del hook pre-push: De forma similar, en el directorio de tu proyecto, crea o edita el archivo `pre-push` en la carpeta `.git/hooks/`:

```bash
touch .git/hooks/pre-push
chmod +x .git/hooks/pre-push
```

2. Agregar el script de Trufflehog: Inserta el siguiente script en el archivo pre-push para ejecutar Trufflehog sobre los commits que se van a hacer push:

```bash
#!/bin/bash

echo "Running Trufflehog pre-push hook..."

# Obtener el rango de commits que se van a hacer push
LOCAL=$(git rev-parse @)
REMOTE=$(git rev-parse @{u})

if [ "$LOCAL" != "$REMOTE" ]; then
    # Escanear el rango de commits que se van a hacer push
    git log $REMOTE..$LOCAL --name-only | xargs trufflehog filesystem

    # Captura el código de salida de Trufflehog
    RESULT=$?

    if [ $RESULT -ne 0 ]; then
        echo "Trufflehog found potential secrets. Push aborted."
        exit 1
    fi
fi

echo "No secrets found. Proceeding with push."
exit 0
```

Este script obtiene el rango de commits que se van a hacer push y ejecuta Trufflehog sobre esos cambios. Si se detectan secretos, el push será abortado.


#### 5. **Buenas prácticas y recomendaciones**
    - Gestión de falsos positivos
    - Actualización y mantenimiento de trufflehog




# Prácticas a Entregar

Todas las prácticas deben desarrollarse creando capturas de pantalla, ficheros auxiliares (scripts, código, etc.), y debe crearse una documentación explicando qué se ha realizado.

Esta documentación se realizará utilizando Markdown o equivalente y se entregará a través de **GitHub Classroom** del módulo **Puesta en Producción Segura**.

## Práctica 2.1. Configuración de GIT (0.50 puntos)

1. Realiza la práctica guiada para configurar GIT en tu entorno de trabajo.

## Práctica 2.2. Trabajando con GIT básico (0.5 puntos)

Haciendo uso de algún proyecto “real”  descargado desde el repositorio GitHub (consultar con el profesor antes de tomar un repositorio, puesto que debe ser de un proyecto real de gran envergadura), demostrar que sabéis usar los comandos útiles de Git para observar el repositorio de dicho proyecto (`git log`, `git diff`, `git blame` y `git show`). 

Por cada uno de estos 4 comandos, hay que evidenciar **4 capturas** (junto a su explicación, ejemplo y todo lo que se desee aportar) de diferentes usos (opciones) del mismo comando. Junto a cada captura, hay que explicar con vuestras palabras lo que se quiere **probar**, **demostrar** o **buscar** con ese comando/opción. 


-  GIT LOG

   - **Captura 1**  
      - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

   - **Captura 2**  
      - Explicación de uso y captura.

   - **Captura 3**  
      - Explicación de uso y captura.

   - **Captura 4**  
      - Explicación de uso y captura.

- GIT DIFF
    - **Captura 1**  
      - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

  - **Captura 2**  
      - Explicación de uso y captura.

  - **Captura 3**  
      - Explicación de uso y captura.

  - **Captura 4**  
      - Explicación de uso y captura.

- GIT SHOW
    - **Captura 1**  
      - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

  - **Captura 2**  
      - Explicación de uso y captura.

  - **Captura 3**  
      - Explicación de uso y captura.

  - **Captura 4**  
      - Explicación de uso y captura.
- GIT BLAME
  - **Captura 1**  
    - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

  - **Captura 2**  
      - Explicación de uso y captura.

  - **Captura 3**  
      - Explicación de uso y captura.

  - **Captura 4**  
      - Explicación de uso y captura.

Aquí tienes el texto convertido a Markdown:

## Práctica 2.3. Ejercicio Práctico: Uso de Comandos de Git y Gitflow (1.5 puntos)

### Objetivo

Demuestre con ejemplos que sabe diferenciar los siguientes conceptos:
- **Resolver conflictos**
- **Fast-forward**
- **Squash**

A partir de la figura que aparece a continuación (además, la rama `master` debe mantenerse en un repositorio GitHub privado), tenéis que hacer uso de **TODOS** los comandos y las opciones más útiles, para construir el siguiente proyecto siguiendo **Gitflow** (no usar extensiones de Gitflow, únicamente los comandos explicados en clase).

![Ejercicio](./L2/exercise-git-flow.png)

> **Recomendación**: Antes de comenzar, leer el [siguiente enlace](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow) para hacerse una idea clara de lo que se debe hacer.


### Requisitos

- **Comandos anteriores**: No es necesario evidenciar (captura, comando y explicación) los comandos del ejercicio anterior (número 1), aunque **deben ser usados** para evidenciar los comandos de este ejercicio.
- La **primera vez** que aparezca un nuevo comando, explicar claramente qué realiza.
- El alumno tiene libertad para crear el guion que crea oportuno para explicar/evidenciar esta práctica (tipo de ficheros a usar -JavaScript, PHP, etc.-, número de ficheros, etc.).
- **Evidenciar el uso del fichero `.gitignore`** con algunos archivos/directorios.
- Los **commits** deben llamarse (C1-XXxx, C2-XXxx), los **tags** (v0.1XXxx, etc.) y las **ramas** (masterXXxx, developXXxx, feature1XXxx, etc.).
- Hacer uso ocasionalmente de la pestaña **Insights > Network** en GitHub para visualizar la evolución de las ramas.
- En **una o dos ocasiones**, crear un fichero vía web y actualizar el repositorio local.
- Simular que las ramas **feature** son realizadas por otro programador (**Programador Nº 2** para simularlo correctamente).
- Al generar un **tag** y subirlo al repositorio remoto, compruebe que se ha creado correctamente (fichero .zip, etc.).


### Secuencia del Proyecto

Una posible secuencia del proyecto podría ser la siguiente (obtenido del libro _Git, Controle la gestión de sus versiones - Samuel Dazón_):

1. **C1 (master)**: Primer commit del proyecto. Este commit añade la base del proyecto en el repositorio. _(Tag: versión v1.0.0)._
2. **C2 (develop)**: Creación de la rama `develop`. Después de este commit, las dos ramas principales están creadas.
3. **C3 (graph_employee)**: Creación de una rama para añadir gráficos en la página de los trabajadores (creado por Programador Nº 2).
4. **C4 (hotfix-negative-time)**: Creación de una rama para gestionar el caso en que un trabajador introduzca un valor negativo en una tarea.
5. **C5 (master)**: Integración del parche en la rama `master`. _(Tag: versión v1.0.1)._
6. **C6 (develop)**: Integración del parche en la rama `develop`.
7. **C7 (task_type)**: Creación de una rama para añadir el tipo de tareas.
8. **C8 (graph_employee)**: Último commit de la rama `graph_employee`.
9. **C9 (develop)**: Integración de la rama `graph_employee` en `develop`. Se elimina la rama `graph_employee`.
10. **C10 (task_type)**: Último commit de la rama `task_type`.
11. **C11 (develop)**: Integración de la rama `task_type` en `develop`.
12. **C12 (release-V1.1)**: Creación de la rama para la versión 1.1.
13. **C13 (export_csv)**: Creación de una rama para agregar una funcionalidad de exportar a CSV las tareas de un empleado durante un periodo.
14. **C14 (release-V1.1)**: Parche para corregir un error en el gráfico cuando el trabajador no tiene tareas.
15. **C15 (master)**: Integración de la versión 1.1 en la rama `master`. _(Tag: versión v1.1)._
16. **C16 (develop)**: Integración del parche de la versión 1.1 en la rama `develop`.
17. **C17 (export_csv)**: Último commit de la rama `export_csv`.
18. **C18 (develop)**: Integración de la rama `export_csv` en `develop`.


### Instrucciones Adicionales

1. **Muestre el estado final de las ramas creadas** con el software gratuito **SourceTree**.
2. **Muestre el estado final de las ramas creadas** con capturas del repositorio GitHub.
3. **Aspectos a evitar**: 
    - Capturas poco claras.
    - No seguir las normas de presentación.
    - Explicación desordenada (use sangrías, tabulaciones, numeración, viñetas, etc.).
    - Faltan comandos.

- **Claridad en la presentación**. Uso de sangrías, capturas concluyentes, con marcas adecuadas y concretas.
- **Explicación y uso del fichero `.gitignore`**.
- **Uso y explicación de comandos**. Adecuado uso y explicación (una vez) de los comandos: `clone`, `init`, `push`, `pull`, `commit`, `merge`, `checkout`, `branch`, `status`, `tag`.
- **Uso de `--graph`** para ver la evolución en cada commit realizado.
- **Capturas del repositorio**. Mínimo dos capturas del repositorio final/parcial de la evolución de las ramas mediante **SourceTree**.
- **Capturas en GitHub** . Mínimo dos capturas del repositorio final/parcial de la evolución de las ramas.
- **Originalidad en el guion** 


## Práctica 2.4. Configurando hooks de GIT (1 punto)

En esta práctica, aprenderás a configurar y utilizar hooks en GIT para automatizar tareas comunes en tu flujo de trabajo. Los hooks son scripts que GIT ejecuta automáticamente en ciertos puntos del ciclo de vida de los commits y otras acciones de GIT. 

#### Objetivos:
1. Configurar un hook pre-commit para verificar el formato del código antes de permitir un commit.
2. Configurar un hook commit-msg para asegurar que los mensajes de commit sigan un formato específico.
3. Configurar un hook post-merge para ejecutar pruebas automatizadas después de una fusión.

#### Instrucciones:
1. Crea un repositorio GIT nuevo o utiliza uno existente.
2. Navega al directorio `.git/hooks` dentro de tu repositorio.
3. Copia y pega los siguientes scripts en los archivos correspondientes dentro del directorio `.git/hooks`:

    - **pre-commit**:
      ```sh
      #!/bin/sh
      echo "Ejecutando pre-commit hook"
      # Aquí puedes agregar comandos para verificar el formato del código
      exit 0
      ```

    - **commit-msg**:
      ```sh
      #!/bin/sh
      echo "Ejecutando commit-msg hook"
      # Aquí puedes agregar comandos para validar el formato del mensaje de commit
      exit 0
      ```

    - **post-merge**:
      ```sh
      #!/bin/sh
      echo "Ejecutando post-merge hook"
      # Aquí puedes agregar comandos para ejecutar pruebas automatizadas
      exit 0
      ```

4. Asegúrate de que los scripts sean ejecutables:
    ```sh
    chmod +x .git/hooks/pre-commit
    chmod +x .git/hooks/commit-msg
    chmod +x .git/hooks/post-merge
    ```

5. Realiza pruebas para verificar que los hooks funcionan correctamente.

#### Entregables:
- Capturas de pantalla o registros de la terminal que muestren la ejecución de cada hook.
- Los scripts utilizados para cada hook.
- Un breve informe describiendo los pasos realizados y cualquier problema encontrado.


## Práctica 2.5. Configurando hooks usando Husky (1 punto)

En esta práctica, aprenderás a configurar y utilizar Husky para gestionar hooks en GIT de manera más sencilla y eficiente. Husky es una herramienta que facilita la creación y gestión de hooks en GIT, permitiendo automatizar tareas comunes en tu flujo de trabajo.

#### Objetivos:
1. Instalar Husky en un proyecto GIT.
2. Configurar un hook pre-commit para verificar el formato del código antes de permitir un commit.
3. Configurar un hook commit-msg para asegurar que los mensajes de commit sigan un formato específico.
4. Configurar un hook post-merge para ejecutar pruebas automatizadas después de una fusión.

#### Instrucciones:
1. Crea un repositorio GIT nuevo o utiliza uno existente.
2. Asegúrate de tener Node.js y npm instalados en tu sistema.
3. Instala Husky en tu proyecto:
    ```sh
    npm install husky --save-dev
    ```

4. Inicializa Husky en tu proyecto:
    ```sh
    npx husky install
    ```

5. Configura los hooks utilizando Husky:
    - **pre-commit**:
      ```sh
      npx husky add .husky/pre-commit "echo 'Ejecutando pre-commit hook'; # Aquí puedes agregar comandos para verificar el formato del código"
      ```

    - **commit-msg**:
      ```sh
      npx husky add .husky/commit-msg "echo 'Ejecutando commit-msg hook'; # Aquí puedes agregar comandos para validar el formato del mensaje de commit"
      ```

    - **post-merge**:
      ```sh
      npx husky add .husky/post-merge "echo 'Ejecutando post-merge hook'; # Aquí puedes agregar comandos para ejecutar pruebas automatizadas"
      ```

6. Realiza pruebas para verificar que los hooks funcionan correctamente.

#### Entregables:
- Capturas de pantalla o registros de la terminal que muestren la ejecución de cada hook.
- Los scripts utilizados para cada hook.
- Un breve informe describiendo los pasos realizados y cualquier problema encontrado.


## Práctica 2.6. Firmando commits (1.5 puntos)

En esta práctica, aprenderás a firmar tus commits en GIT utilizando GPG (GNU Privacy Guard) para asegurar la autenticidad e integridad de tus cambios. Firmar tus commits añade una capa adicional de seguridad y confianza, permitiendo a otros verificar que los commits realmente provienen de ti. Además, podrás ver la verificación de tus commits firmados en plataformas como GitHub.

#### Objetivos:
1. Generar una clave GPG si no tienes una.
2. Configurar GIT para usar tu clave GPG.
3. Firmar tus commits con GPG.
4. Verificar la firma de tus commits en GitHub.

#### Instrucciones:
1. **Generar una clave GPG**:
    - Si no tienes una clave GPG, genera una nueva clave siguiendo las instrucciones de la [documentación oficial de GPG](https://gnupg.org/documentation/).

2. **Configurar GIT para usar tu clave GPG**:
    - Exporta tu clave pública y añádela a tu cuenta de GitHub siguiendo las instrucciones de la [documentación de GitHub](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account).
    - Configura GIT para usar tu clave GPG:
      ```sh
      git config --global user.signingkey <tu_clave_gpg>
      git config --global commit.gpgSign true
      ```

3. **Firmar tus commits con GPG**:
    - Realiza un commit firmado:
      ```sh
      git commit -S -m "Tu mensaje de commit"
      ```

4. **Verificar la firma de tus commits en GitHub**:
    - Sube tus commits firmados a GitHub y verifica que aparezcan como "Verified" en la interfaz de GitHub.

#### Entregables:
- Capturas de pantalla o registros de la terminal que muestren la generación de la clave GPG y la configuración en GIT.
- Capturas de pantalla de los commits firmados y verificados en GitHub.
- Un breve informe describiendo los pasos realizados y cualquier problema encontrado.


## Práctica 2.7. Securizando GIT: Git-Secrets (1.5 puntos)

En esta práctica, aprenderás a utilizar `git-secrets` para evitar que información sensible, como claves API y contraseñas, se incluya accidentalmente en tus commits de GIT. `git-secrets` es una herramienta que escanea tus commits y previene que información sensible sea añadida al repositorio.

#### Objetivos:
1. Instalar `git-secrets` en tu sistema.
2. Configurar `git-secrets` en un repositorio GIT.
3. Añadir patrones para detectar información sensible.
4. Probar `git-secrets` para asegurarte de que funciona correctamente.

#### Instrucciones:
1. **Instalar `git-secrets`**:
    - Sigue las instrucciones de instalación para tu sistema operativo desde el [repositorio oficial de git-secrets](https://github.com/awslabs/git-secrets#installing-git-secrets).

2. **Configurar `git-secrets` en tu repositorio**:
    - Navega al directorio de tu repositorio GIT y ejecuta:
      ```sh
      git secrets --install
      git secrets --register-aws
      ```

3. **Probar `git-secrets`**:
    - Intenta hacer un commit que contenga información sensible para verificar que `git-secrets` lo detecta y bloquea:
      ```sh
      echo "AWS_SECRET_ACCESS_KEY=example" > test.txt
      git add test.txt
      git commit -m "Añadiendo clave secreta"
      ```

4. **Verificar la configuración**:
    - Asegúrate de que `git-secrets` está correctamente configurado y funcionando en tu repositorio.


5. **Añadir patrones personalizados**:
    - Puedes añadir patrones personalizados para detectar información sensible específica de tu proyecto:
      ```sh
      git secrets --add 'tu_patron_personalizado'
      ```
    - Haz comprobaciones de que se han buscado y detectado tus patrones personalizados.
  


## Práctica 2.8. Securizando GIT: Trufflehog: Local (1.25 puntos)

En esta práctica, aprenderás a utilizar TruffleHog para escanear tu repositorio GIT en busca de información sensible, como claves API y contraseñas, que puedan haberse incluido accidentalmente en tus commits. TruffleHog es una herramienta que busca patrones de información sensible en el historial de commits de tu repositorio.

#### Objetivos:
1. Instalar TruffleHog en tu sistema.
2. Configurar TruffleHog para escanear un repositorio GIT local.
3. Ejecutar TruffleHog para detectar información sensible en el historial de commits.
4. Analizar los resultados del escaneo y tomar medidas para eliminar cualquier información sensible encontrada.

#### Instrucciones:
1. **Instalar TruffleHog**

2. **Configurar TruffleHog para escanear un repositorio GIT local**:
    - Navega al directorio de tu repositorio GIT.

3. **Ejecutar TruffleHog**:
    - Ejecuta TruffleHog para escanear el historial de commits de tu repositorio:
      ```sh
      trufflehog --regex --entropy=True .
      ```

4. **Analizar los resultados del escaneo**:
    - Revisa los resultados proporcionados por TruffleHog para identificar cualquier información sensible.
    - Si se encuentra información sensible, toma las medidas necesarias para eliminarla del historial de commits (por ejemplo, utilizando `git filter-branch` o `BFG Repo-Cleaner`).

5. **Verificar la eliminación de información sensible**:
    - Asegúrate de que la información sensible ha sido eliminada correctamente y que el repositorio está limpio.
6. **Provoca una vulnerabilidad y localizala**
    - Haz un commit con credenciales que deben protegerse.
    - Haz un commit borrando la credencial.
    - Escanea y encuentra la vulnerabilidad.


## Práctica 2.9. Securizando GIT: Trufflehog: Remoto (1.25 puntos)

En esta práctica, aprenderás a utilizar TruffleHog para escanear un repositorio GIT remoto en busca de información sensible.

#### Objetivos:
1. Configurar TruffleHog para escanear un repositorio GIT remoto.
2. Ejecutar TruffleHog para detectar información sensible en el historial de commits del repositorio remoto.
3. Analizar los resultados del escaneo y tomar medidas para eliminar cualquier información sensible encontrada.

#### Instrucciones:
1. **Configurar TruffleHog para escanear un repositorio GIT remoto**:
    - Identifica la URL del repositorio GIT remoto que deseas escanear.

2. **Ejecutar TruffleHog**:
    - Ejecuta TruffleHog para escanear el historial de commits del repositorio remoto:
      ```sh
      trufflehog --regex --entropy=True <URL_del_repositorio_remoto>
      ```

3. **Analizar los resultados del escaneo**:
    - Revisa los resultados proporcionados por TruffleHog para identificar cualquier información sensible.
    - Si se encuentra información sensible, toma las medidas necesarias para eliminarla del historial de commits (por ejemplo, utilizando `git filter-branch` o `BFG Repo-Cleaner`).

4. **Verificar la eliminación de información sensible**:
    - Asegúrate de que la información sensible ha sido eliminada correctamente y que el repositorio está limpio.

## Criterios de Evaluación Asociados

| RA3  | Detecta y corrige vulnerabilidades de aplicaciones web analizando su código fuente y configurando servidores web |
|------|------------------------------------------------------------------------------------------------------------------|
| 3.e  | Se han utilizado algoritmos criptográficos seguros para almacenar las contraseñas de usuario                     |


| RA5  | Implanta sistemas seguros de desplegado de software, utilizando herramientas para la automatización de la construcción de sus elementos |
|------|---------------------------------------------------------------------------------------------------------------------------------------|
| 5.a  | Se han identificado las características, principios y objetivos de la integración del desarrollo y operación del software              |
| 5.b  | Se han implantado sistemas de control de versiones, administrando los roles y los permisos solicitados                                |
| 5.c  | Se han instalado, configurado y verificado sistemas de integración continua, conectándolos con sistemas de control de versiones        |




## Practicas investigación

### Submódulos

¿Qué son los submódulos? ¿Cómo funcionan los submódulos?

De manera práctica debes realizar las siguientes tareas:

- Crea un repositorio con submódulos.
- Utiliza submódulos (clonar, commit, log, status, y reset en submódulos).
  
[Documentación de submódulos](https://git-scm.com/book/en/v2/Git-Tools-Submodules)



### Uso avanzado: trufflehog

Realiza una investigación sobre el uso avanzado de trufflehog, se deben cubrir los siguientes puntos:

  - Gestión de falsos positivos
  - Actualización y mantenimiento de trufflehog

### Protección de credenciales.

Investigue cómo instalar y usar las herramientas:
    - El comando `git filter-branch`.
    - [`BFG Repo-Cleaner`](https://rtyley.github.io/bfg-repo-cleaner/)
    - 
### Propuesta por el estudiante

El estudiante puede proponer una práctica de investigación.# Laboratorio: Introducción a CVS (Git)

## Objetivos
- Comprender los conceptos básicos de control de versiones.
- Aprender a utilizar Git para gestionar proyectos.

## Prerrequisitos
- Tener Git instalado en tu sistema.
- Conocimientos básicos de línea de comandos.

## Contenido

El material de lectura y de estudio es el libro [ProGit](https://git-scm.com/book/en/v2). La práctica es un índice y resumen de lo que se puede encontrar en ese libro.

Para el repaso de GIT elemental se plantea utilizar los recursos y los ejercicios propuestos en la Web [AprendeConAlf](https://aprendeconalf.es/docencia/git/)

### 0. Instalación de Git

Para instalar Git en tu sistema, sigue los pasos correspondientes a tu sistema operativo:

#### En Windows
1. Descarga el instalador de Git desde [git-scm.com](https://git-scm.com/download/win).
2. Ejecuta el instalador y sigue las instrucciones en pantalla. Asegúrate de seleccionar las opciones predeterminadas a menos que tengas una razón específica para cambiarlas.
3. Una vez completada la instalación, abre la terminal de Git Bash desde el menú de inicio.

#### En macOS
1. Abre la terminal.
2. Instala Git utilizando Homebrew (si no tienes Homebrew instalado, sigue las instrucciones en [brew.sh](https://brew.sh/)):
    ```bash
    brew install git
    ```
3. Verifica la instalación comprobando la versión de Git:
    ```bash
    git --version
    ```

#### En Linux
1. Abre la terminal.
2. Instala Git utilizando el gestor de paquetes de tu distribución. Por ejemplo, en Debian/Ubuntu:
    ```bash
    sudo apt-get update
    sudo apt-get install git
    ```
   En Fedora:
    ```bash
    sudo dnf install git
    ```
   En Arch Linux:
    ```bash
    sudo pacman -S git
    ```
3. Verifica la instalación comprobando la versión de Git:
    ```bash
    git --version
    ```

Una vez que hayas instalado Git, puedes proceder a la configuración inicial y a los demás pasos del laboratorio.


### 1. Configuración Inicial

Antes de comenzar a trabajar con Git, es importante configurar tu entorno para que los commits que realices estén correctamente identificados. Esta configuración solo necesita hacerse una vez por máquina.

#### Configuración del Nombre de Usuario y Correo Electrónico

Git utiliza tu nombre de usuario y correo electrónico para asociar los commits con tu identidad. Configura estos valores utilizando los siguientes comandos:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tuemail@example.com"
```

Puedes verificar la configuración actual con:

```bash
git config --global --list
```

#### Configuración del Editor de Texto

Git utiliza un editor de texto para que puedas escribir mensajes de commit. Puedes configurar tu editor preferido, por ejemplo, para usar `nano`:

```bash
git config --global core.editor nano
```

Para usar `vim`:

```bash
git config --global core.editor vim
```


#### Configuración de Alias

Los alias en Git te permiten crear comandos abreviados para tareas comunes. Por ejemplo, puedes crear un alias para `git status`:

```bash
git config --global alias.st status
```

Ahora, en lugar de escribir `git status`, puedes simplemente escribir `git st`.

#### Eliminar una configuración global

Para borrar una configuración global de Git, puedes usar el comando en el terminal:

```bash
git config --global --unset <nombre_de_la_configuración>
```

Por ejemplo, si deseas borrar la configuración global del alias podrímos usarlo del siguiente modo:

```bash
git config --global --unset alias.st
```

#### Configuración del Archivo `.gitignore`

El archivo `.gitignore` le dice a Git qué archivos o directorios debe ignorar en un proyecto. Crea un archivo `.gitignore` en el directorio raíz de tu proyecto y añade las rutas de los archivos que deseas ignorar. Por ejemplo:

```
# Ignorar archivos de configuración del sistema operativo
.DS_Store
Thumbs.db

# Ignorar archivos de compilación
/build
/dist

# Ignorar archivos de entorno
.env
```

#### Configuración de Credenciales

Para evitar tener que ingresar tu nombre de usuario y contraseña cada vez que interactúas con un repositorio remoto, puedes almacenar tus credenciales de manera segura. Puedes usar el siguiente comando para habilitar el almacenamiento en caché de credenciales:


```bash
git config --global credential.helper cache
```

Esto almacenará las credenciales en memoria durante 15 minutos (900 segundos) de forma predeterminada. Si deseas cambiar el tiempo de almacenamiento en caché, puedes especificar un tiempo en segundos usando el siguiente comando:

```bash
git config --global credential.helper 'cache --timeout=<segundos>'
```

Por ejemplo, para almacenar las credenciales en caché durante 1 hora (3600 segundos):

```bash
git config --global credential.helper 'cache --timeout=3600'
```




### 2. Creación de un Repositorio

1. Crea un nuevo directorio y navega a él:
    - Abre tu terminal.
    - Usa el comando `mkdir` para crear un nuevo directorio llamado `mi_proyecto` (en la práctica pon el siguiente código de nombre `CE_apellido_nombre`, es decir en mi caso sería: `CE_Caballero_Carlos`).
 
    ```bash
    mkdir CE_Caballero_Carlos
    cd CE_Caballero_Carlos
    ```

2. Inicializa un repositorio Git:
    - Asegúrate de estar en el directorio `CE_Caballero_Carlos`
    - Usa el comando `git init` para inicializar un nuevo repositorio Git en este directorio.

    ```bash
    git init
    ```

    **Explicación**:
    - `git init`: Este comando inicializa un nuevo repositorio Git en el directorio actual. Crea un subdirectorio oculto llamado `.git` que contiene todos los archivos necesarios para el control de versiones.
    
    **Resultado**:
    - Después de ejecutar este comando, tu directorio `CE_Caballero_Gonzalez` se convertirá en un repositorio Git. Puedes verificarlo listando los archivos ocultos con `ls -a` y viendo el directorio `.git`.

    ```bash
    ls -a
    ```

    Deberías ver una salida similar a esta:
    ```
    .  ..  .git
    ```

    Esto confirma que el repositorio Git ha sido inicializado correctamente.


### 3. Estados de un Fichero en Git

En Git, un fichero puede estar en uno de los siguientes estados: 

- **no rastreado (untracked)**.
- **modificado (modified)**.
- **preparado (staged)**.
- **confirmado (committed)**. 
  
A continuación, se explica cada uno de estos estados y cómo se transiciona entre ellos.

![Lifecycle Git](./L2/lifecycle-git.png)

*Figura 1: Ciclo de vida de Git*

#### 1. No Rastreado (Untracked)
Un fichero no rastreado es aquel que existe en el directorio de trabajo pero no está siendo seguido por Git. Esto significa que Git no tiene información sobre este fichero y no lo incluirá en los commits.

**Ejemplo**:
```bash
echo "Nuevo archivo" > nuevo.txt
git status
```

El comando `git status` mostrará que `nuevo.txt` está no rastreado.

#### 2. Modificado (Modified)
Un fichero modificado es aquel que ha sido cambiado en el directorio de trabajo pero no ha sido añadido al área de preparación. Git sabe que el fichero ha cambiado porque lo está rastreando, pero los cambios aún no están listos para ser confirmados.

**Ejemplo**:
```bash
echo "Cambio en el archivo" >> hola.txt
git status
```

El comando `git status` mostrará que `hola.txt` ha sido modificado.

#### 3. Preparado (Staged)
Un fichero preparado es aquel que ha sido añadido al área de preparación. Esto significa que los cambios en este fichero están listos para ser incluidos en el próximo commit.

**Ejemplo**:
```bash
git add hola.txt
git status
```
El comando `git status` mostrará que `hola.txt` está en el área de preparación.

#### 4. Confirmado (Committed)
Un fichero confirmado es aquel cuyos cambios han sido guardados en el repositorio de Git. Esto significa que los cambios están ahora en el historial de commits y pueden ser recuperados en cualquier momento.

**Ejemplo**:
```bash
git commit -m "Actualizar hola.txt"
git status
```
El comando `git status` mostrará que no hay cambios pendientes, ya que todos los cambios han sido confirmados.

### Transiciones entre Estados

1. **De No Rastreado a Preparado**:
    ```bash
    git add nuevo.txt
    ```
    Este comando añade el fichero `nuevo.txt` al área de preparación.

2. **De Modificado a Preparado**:
    ```bash
    git add hola.txt
    ```
    Este comando añade el fichero `hola.txt` modificado al área de preparación.

3. **De Preparado a Confirmado**:
    ```bash
    git commit -m "Mensaje de commit"
    ```
    Este comando confirma los cambios preparados con un mensaje descriptivo.

4. **De Confirmado a Modificado**:
    Simplemente edita el fichero:
    ```bash
    echo "Otro cambio" >> hola.txt
    git status
    ```
    El comando `git status` mostrará que `hola.txt` ha sido modificado nuevamente.

### Resumen de Comandos
- **git status**: Muestra el estado de los ficheros en el repositorio.
- **git add <archivo>**: Añade un fichero al área de preparación.
- **git commit -m "mensaje"**: Confirma los cambios preparados con un mensaje descriptivo.

Estos comandos y estados son fundamentales para trabajar eficientemente con Git y mantener un historial claro y organizado de los cambios en tu proyecto.


### 6. Ramas en Git

La gestión de ramas se basa en el contenido del libro [ProGit]
(https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)

### 7. Breve Paseo por GitHub: Crear mi Primer Repositorio Remoto y Usarlo en Local

Crear cuenta gratuita en [GitHub](https://github.com)

- Navegando vía web, conozca proyectos de repositorios públicos como:
  - Proyecto del [Kernel de Linux](https://github.com/torvalds/linux)
  - Proyecto de [Angular](https://github.com/angular/angular)
  - [WordPress](https://github.com/WordPress/WordPress), etc.

- Compruebe:
  - Ficheros que existen.
  - Ramas que tienen.
  - Tags.
  - Quién contribuye en este proyecto.
  - Estadística de cada uno de los lenguajes de programación usados.
  - Etc.

#### Crear Repositorio `Practica1Git` para practicar

- Enlace del repositorio: [https://github.com/politecnicoDAW-2024/pps-repositorio-demo-git](https://github.com/politecnicoDAW-2024/pps-repositorio-demo-git)

Pasos:
1. Añadir un archivo `README.md` para explicar para qué sirve este proyecto.
2. Analizar y crear el fichero `.gitignore`.
3. Añadir varios ficheros desde el repositorio.
4. Inicializar/Clonar el proyecto en el repositorio local de su ordenador.
   - Comprobar que existe una única rama.
   - Comprobar que los ficheros se han descargado.

#### ¿Qué ha sucedido en el directorio oculto `.git` de cada uno de los proyectos?
Ejecutar el siguiente comando:
```bash
ls -alR
```

**Importante para los alumnos:**

> “Si se utiliza Git de forma normal sin tratar de comprender los mecanismos internos, un usuario nunca tendrá la necesidad de ir a esta carpeta. De hecho, la mayoría de los programadores no saben ni que existe .git”.

Comprobar que se ha añadido información que relaciona un alias (origin) con el repositorio remoto  en el fichero `.git/config`. Usaremos este alias (el mismo en todos los repositorios) para no tener que poner la ruta completa de la URL en cuestión.

1. Crear nuestro primer fichero del proyecto
2. Realizar seguimiento del archivo:
```
git add .
```
3. Hacer un commit:
```
git commit -m "Mensaje del commit"
```
4. Subirlo al repositorio remoto:
```
git push origin main
```

Evitar que nos pidan más el usuario y la contraseña cada vez que hagamos uso del repositorio remoto
Opciones:

Para almacenar las credenciales para siempre:
```
git config --global credential.helper store
```
Para almacenarlas por un tiempo limitado (ej. 1 hora):

```
git config --global credential.helper 'cache --timeout=3600'
```

5. Probar con otro repositorio remoto
Comprobar que existen muchas ramas, commits, tags, archivos, etc., no como en nuestro repositorio “vacío”.

### 8. HASH

#### ¿Qué es?

Verificar que un archivo no es corrupto (descarga, git, etc.). También se usa para la autenticación de usuarios sin tener que almacenar su contraseña en texto plano. Existen diferentes mecanismos como:

- MD4
- MD5
- SHA1
- SHA256
- SHA512
- CRC

##### Ejemplo con `sha1sum`

Para su comprensión, probemos con `sha1sum` (40 caracteres), que guarda:

```bash
echo -n "Git" | sha1sum
echo -n "Pepe" | sha1sum
echo -n "Este es un código de Carlos Caballero que ha encontrado un fallo en ese fichero, y lo va a arreglar" | sha1sum
```

En la actualidad se usan otros tipos de hash, pero para su comprensión es suficiente.

##### ¿Cómo lo usa Git?
Git utiliza pares clave/valor por cada fichero (contenido y su hash) para detectar rápidamente cuando un fichero ha sido modificado por un programador.

##### Riesgo de Colisión
El riesgo de colisión es improbable, incluso en proyectos muy grandes.


Aquí tienes el texto en formato Markdown, complementado con más ejemplos y explicaciones sobre el comando `reset` en Git:

### 9. Restaurando Ficheros del Repositorio (Reset)

#### ¿Qué es `HEAD`?

En Git, `HEAD` es el alias del commit que apunta a la rama seleccionada actualmente en el repositorio. `HEAD` tiene un hash como cualquier commit, pero debido a que se usa frecuentemente, se le asigna este alias para referirse al último commit de la rama activa.

#### ¿Por qué es importante?

Es importante entender qué es `HEAD` para evitar borrar o modificar cosas por accidente. Usar correctamente los comandos de Git que interactúan con `HEAD` nos permite manejar de forma segura las restauraciones o cambios en el historial.

#### Subir varios ficheros al repositorio

Antes de restaurar ficheros, es necesario entender los siguientes pasos comunes en Git:

1. **Añadir archivos al área de preparación (staging):**
   ```bash
   git add archivo.txt
   ```

2. **Hacer commit de los cambios en el repositorio:**
   ```bash
   git commit -m "Mensaje del commit"
   ```

Una vez que hemos subido varios ficheros al repositorio, podemos decidir volver a versiones anteriores, ya sea de commits completos o de ficheros individuales.

#### Restaurar commits o ficheros con `git reset`

##### Resetear al área de staging (index)

El comando `git reset` se usa para mover el `HEAD` a un commit anterior o para restaurar un fichero en específico. Se puede usar de diferentes formas:

- Para resetear el repositorio al estado de un commit anterior en el área de staging:
   ```bash
   git reset [commit]
   ```
   Esto no modifica los archivos en el directorio de trabajo, solo mueve el estado de los archivos en staging (index) al commit seleccionado.

- Para resetear un archivo específico al estado del commit anterior en staging:
   ```bash
   git reset archivo.txt
   ```

##### Resetear el directorio de trabajo (con `--hard`)

Si se quiere restaurar no solo el staging, sino también el directorio de trabajo, se utiliza la opción `--hard`:

- Para resetear todo el proyecto a un commit anterior, incluyendo el directorio de trabajo:
   ```bash
   git reset [commit] --hard
   ```

- Para resetear un archivo específico directamente en el directorio de trabajo:
   ```bash
   git reset --hard archivo.txt
   ```

##### Ejemplo completo

Supongamos que hemos subido varios ficheros al repositorio y queremos volver a una versión anterior de nuestro proyecto:

1. **Añadir archivos:**
   ```bash
   git add index.php style.css
   ```

2. **Hacer commit:**
   ```bash
   git commit -m "Añadido archivo index.php y estilo"
   ```

3. **Resetear al commit anterior, pero manteniendo los cambios en el área de staging:**
   ```bash
   git reset [commit]
   ```

4. **Resetear completamente el proyecto a un commit anterior, eliminando todos los cambios no guardados:**
   ```bash
   git reset [commit] --hard
   ```

##### Diferencias entre `git reset` y `git revert`

Mientras que `git reset` cambia el historial del repositorio, `git revert` es una forma más segura de deshacer cambios, ya que crea un nuevo commit que deshace las modificaciones de un commit anterior sin cambiar el historial existente. Ejemplo de `git revert`:

```bash
git revert [commit]
```

Esto es útil en situaciones en las que quieres deshacer un commit, pero mantener el historial de commits intacto. No obstante, es peligroso si queremos borrar credenciales. En ese caso es mejor usar `git reset`.

##### Conclusión

El comando `git reset` es muy poderoso y puede ser peligroso si no se usa con cuidado. Es importante saber cuándo usarlo y cómo afecta el estado del repositorio y del directorio de trabajo. Recuerda siempre hacer un respaldo de tu trabajo antes de realizar acciones destructivas como `git reset --hard`.


Aquí está el contenido convertido a Markdown y ampliado, dividido en partes para mejor comprensión:

---

### 10. Observando Nuestro Repositorio con Git: `status`, `log`, `show`, `blame`, `diff`

Para comprender mejor los comandos que vamos a utilizar, puede ser útil trabajar con un proyecto de tamaño considerable, por ejemplo, un proyecto de código abierto descargado de Internet. Con un proyecto más grande, es más fácil observar cambios, ramificaciones y commits en diferentes momentos del desarrollo.

#### 1. `git status`: Observando el Estado de los Archivos

El comando `git status` permite ver los archivos que están en la zona de **staging** o aquellos que han sido modificados pero aún no añadidos al staging.

```bash
git status
```

Este comando te ofrece información como:

- Archivos modificados.
- Archivos que están listos para ser confirmados (en el staging).
- Archivos no seguidos por Git.
  
Es especialmente útil para decidir qué archivos deben ser confirmados o descartados antes de realizar un commit.

#### 2. `git log`: Explorando el Historial de Commits

El comando `git log` muestra el historial de commits en la rama actual. Los commits se muestran en orden descendente, del más reciente al más antiguo, y proporciona detalles como:

- **Título** del commit.
- **Hash** del commit (importante para trabajar con identificaciones precisas).
- **Autor** y **fecha** del commit.

##### Ejemplos Útiles de `git log`

- Ver los últimos 10 commits:

  ```bash
  git log -10
  ```

- Mostrar los últimos 10 commits con los cambios realizados en cada uno:

  ```bash
  git log -10 -p
  ```

  Esto te permite ver las diferencias específicas (líneas añadidas, eliminadas, etc.) en cada commit.

- Ver una versión condensada del historial, mostrando solo los títulos de los commits y una representación gráfica de las ramificaciones del proyecto:

  ```bash
  git log --oneline --graph --decorate -10
  ```

  La opción `--graph` es útil para visualizar las fusiones (*merges*) y bifurcaciones del proyecto.

- Mostrar estadísticas detalladas sobre los cambios en un commit (líneas añadidas, eliminadas, archivos modificados, etc.):

  ```bash
  git log -1 --stat
  ```

- Filtrar commits entre fechas específicas:

  ```bash
  git log --oneline --before="2019-12-12" --after="2019-12-01"
  ```

- Ver el historial de cambios para un archivo específico (ej. `index.php`):

  ```bash
  git log -3 index.php
  ```

##### Exploración del Historial en Varias Ramas

Es fundamental usar la opción `--graph` para observar cómo han evolucionado los commits en diferentes ramas:

```bash
git log --graph --all
```

Esto te permite ver la cronología de los commits en todas las ramas del proyecto.


#### 3. `git show`: Explorando un Commit o Archivo Específico

El comando `git show` te permite ver los detalles de un commit específico o de un archivo modificado en un commit. Es útil cuando quieres ver exactamente qué cambió en un commit determinado.

- Ver los cambios de un commit completo:

  ```bash
  git show [hash del commit]
  ```

- Ver los cambios de un archivo en particular en un commit:

  ```bash
  git show [hash del commit]:[archivo]
  ```

#### 4. `git diff`: Comparando Diferencias entre Estados

El comando `git diff` se usa para comparar diferencias entre distintos estados del proyecto, como el **directorio de trabajo**, el **índice** (staging), y los **commits** en el repositorio local.

##### Comparaciones Comunes con `git diff`

- Ver diferencias entre el **directorio de trabajo** y el **staging** (archivos modificados pero no añadidos a staging):

  ```bash
  git diff
  ```

- Ver diferencias entre el **directorio de trabajo** y `HEAD` (la última versión en el repositorio):

  ```bash
  git diff HEAD
  ```

- Comparar el **staging** con el último commit (`HEAD`):

  ```bash
  git diff --cached
  ```

  Esto es útil cuando has añadido cambios al staging (`git add .`) y quieres ver la diferencia con el último commit.

- Comparar dos commits específicos:

  ```bash
  git diff [commit1] [commit2]
  ```

- Comparar dos ramas:

  ```bash
  git diff [rama1] [rama2]
  ```

- Comparar un archivo en el **directorio de trabajo** con su versión en `HEAD`:

  ```bash
  git diff -r HEAD archivo.txt
  ```

#### 5. `git blame`: Identificando Autores y Fechas de Cambios

El comando `git blame` te permite ver quién cambió una línea específica en un archivo y cuándo lo hizo. Es muy útil para rastrear la historia de un archivo y entender la razón detrás de ciertos cambios.

- Ver quién ha modificado un archivo y en qué líneas:

  ```bash
  git blame archivo.txt
  ```

Aquí tienes el contenido convertido a Markdown y ampliado:

---

### 11. Uso de Tags en Git


Los **tags** son extremadamente útiles para:

- **Versionado del software**: Te permiten identificar fácilmente las versiones del software, lo cual es esencial para el desarrollo continuo, la entrega de versiones estables, y el mantenimiento de la compatibilidad.
- **Lanzamientos de versiones**: En muchos proyectos, los tags se utilizan para marcar los puntos en los que una nueva versión del software está lista para ser lanzada.
- **Facilitar la navegación**: En lugar de recordar un hash largo y complejo, los tags proporcionan una manera sencilla de navegar y referenciar puntos clave en el historial de un proyecto.


#### ¿Qué es un Tag?

Un **tag** en Git es un puntero inmutable a un commit específico. Puedes imaginarlo como una "etiqueta" que se le asigna a un commit. Una vez creado, ese tag no cambia, lo que te permite hacer referencia a un commit específico por su nombre en lugar de por su hash, lo cual es más conveniente, especialmente cuando trabajas con versiones liberadas de software.

Los tags son ampliamente usados para **versionar** programas y librerías siguiendo un esquema de versiones **semánticas** en el formato `x.y.z`, donde:

- **x**: Indica una versión mayor. Se usa cuando hay cambios significativos en el proyecto que pueden romper la compatibilidad con versiones anteriores. Normalmente, estos cambios ocurren en la rama `master` o `main`.
- **y**: Indica una versión menor. Se utiliza cuando se añade nueva funcionalidad sin romper la compatibilidad con versiones anteriores.
- **z**: Indica una versión de parches o correcciones de bugs.

#### Ejemplo de Versionado Semántico:

- `1.0.0`: Primera versión estable.
- `1.1.0`: Se ha añadido una nueva funcionalidad.
- `1.1.1`: Se ha corregido un bug, pero no se añade funcionalidad nueva.

#### Crear un Tag

Puedes crear un tag en cualquier momento para identificar una versión específica del código. Existen dos formas comunes de hacerlo:

##### 1. Tag en el Commit Actual

Para crear un **tag** en el commit actual (el último commit que has realizado en la rama en la que estás), puedes usar el siguiente comando:

```bash
git tag 2.0.0
```

En este caso, `2.0.0` es el nombre del tag que estamos creando. Este tag representará el estado actual del repositorio.

##### 2. Tag en un Commit Específico

Si deseas poner un **tag** en un commit anterior, puedes hacerlo especificando el hash del commit:

```bash
git tag 2.1.0 [hash del commit]
```

Esto es útil cuando quieres etiquetar un commit antiguo, por ejemplo, cuando olvidaste poner un tag en una versión anterior del proyecto.


##### Listar Tags

Para ver todos los **tags** que has creado en el repositorio, puedes usar el siguiente comando:

```bash
git tag -l
```

Este comando lista todos los tags en el repositorio, de una manera similar a cómo `git log` muestra los commits, pero en este caso solo te mostrará los tags con información más limitada.


##### Subir Tags al Repositorio Remoto

Una vez que has creado tags en tu repositorio local, puedes subirlos al repositorio remoto (por ejemplo, GitHub). Hay dos formas de hacerlo:

###### 1. Subir Todos los Tags

Si quieres enviar todos los tags que has creado al repositorio remoto, puedes usar este comando:

```bash
git push origin --tags
```

Este comando enviará todos los tags locales que aún no estén en el repositorio remoto. Es especialmente útil cuando creas múltiples tags de una sola vez.

###### 2. Subir un Tag Específico

Si prefieres subir solo un tag en particular, puedes hacerlo especificando el nombre del tag:

```bash
git push origin <nombre_tag>
```

Por ejemplo, para subir el tag `2.0.0`, puedes usar:

```bash
git push origin 2.0.0
```


#### Ejemplo Práctico

##### Crear y Subir un Tag:

1. **Realizas un commit importante**, por ejemplo, finalizas una nueva funcionalidad:

    ```bash
    git commit -m "Añadida nueva funcionalidad X"
    ```

2. **Creas un tag** que identifique este commit como una nueva versión (por ejemplo, la versión 1.2.0):

    ```bash
    git tag 1.2.0
    ```

3. **Subes el tag al repositorio remoto**:

    ```bash
    git push origin 1.2.0
    ```

Ahora, en GitHub (o cualquier otro servidor Git), podrás ver que tu repositorio tiene un tag `1.2.0`, indicando esa versión específica del proyecto.

##### ¿Cómo se ven los Tags en GitHub?

En GitHub, los **tags** se pueden ver en la pestaña "Tags" o "Releases" del repositorio. Cuando se hace clic en un tag, GitHub muestra el estado del código en ese punto, permitiendo que otros puedan descargar la versión específica del proyecto en la que ese tag fue creado.

Además, GitHub permite **crear lanzamientos** (releases) asociados con tags, lo que es ideal para proyectos de software en los que se liberan versiones formales del código. Al crear un **release**, puedes asociar un changelog, notas de la versión, y hasta archivos binarios relacionados.


### 12. Git Hooks

Git Hooks son scripts que Git ejecuta automáticamente antes o después de ciertos eventos, como commits, pushes y merges. Estos hooks permiten automatizar tareas y personalizar el flujo de trabajo de Git. Los hooks se configuran a nivel de repositorio y se encuentran en el directorio `.git/hooks`.

#### Tipos de Git Hooks

Existen dos tipos principales de hooks en Git:

1. **Hooks del Lado del Cliente (Client-Side Hooks)**:
    - **pre-commit**: Se ejecuta antes de que se realice un commit. Se utiliza para verificar el código (por ejemplo, ejecutar linters o pruebas).
    - **prepare-commit-msg**: Se ejecuta antes de que el mensaje de commit sea editado. Se puede usar para modificar el mensaje de commit predeterminado.
    - **commit-msg**: Se ejecuta después de que el mensaje de commit ha sido editado. Se utiliza para validar el mensaje de commit.
    - **post-commit**: Se ejecuta después de que se ha realizado un commit. Se puede usar para notificaciones o tareas de registro.

2. **Hooks del Lado del Servidor (Server-Side Hooks)**:
    - **pre-receive**: Se ejecuta antes de que se acepte un push en el servidor. Se utiliza para validar los cambios entrantes.
    - **update**: Similar a `pre-receive`, pero se ejecuta una vez por cada rama que se está actualizando.
    - **post-receive**: Se ejecuta después de que se ha aceptado un push. Se puede usar para desplegar código o notificar a otros sistemas.

#### Configuración de Git Hooks

Para configurar un hook, simplemente crea un archivo en el directorio `.git/hooks` con el nombre del hook y hazlo ejecutable. Aquí tienes un ejemplo de cómo configurar un hook `pre-commit` que ejecuta un linter antes de cada commit:

1. Navega al directorio de hooks:
    ```bash
    cd .git/hooks
    ```

2. Crea un archivo llamado `pre-commit`:
    ```bash
    touch pre-commit
    ```

3. Haz que el archivo sea ejecutable:
    ```bash
    chmod +x pre-commit
    ```

4. Edita el archivo `pre-commit` para que ejecute el linter:
```bash
#!/bin/sh
# Ejecutar linter
eslint . || {
    echo "Linter errors, aborting commit."
    exit 1
}
```

#### Ejemplo de Hook `pre-commit`

Aquí tienes un ejemplo más detallado de un hook `pre-commit` que verifica si hay archivos con errores de sintaxis antes de permitir un commit:

```bash
#!/bin/sh
# Hook pre-commit para verificar errores de sintaxis

# Verificar archivos Python
for file in $(git diff --cached --name-only | grep '\.py$'); do
    python -m py_compile $file
    if [ $? -ne 0 ]; then
        echo "Errores de sintaxis en $file. Abortando commit."
        exit 1
    fi
done

# Verificar archivos JavaScript
for file in $(git diff --cached --name-only | grep '\.js$'); do
    eslint $file
    if [ $? -ne 0 ]; then
        echo "Errores de linter en $file. Abortando commit."
        exit 1
    fi
done

echo "Todos los archivos están correctos. Procediendo con el commit."
```

### Husky && CommitLint

Husky y CommitLint son herramientas que ayudan a mantener la calidad del código y la consistencia en los mensajes de commit. Husky permite ejecutar scripts de Git Hooks de manera fácil y configurable, mientras que CommitLint valida los mensajes de commit según reglas definidas.

#### Instalación de Husky y CommitLint

Primero, necesitas instalar Husky y CommitLint en tu proyecto. Asegúrate de tener Node.js y npm instalados.

1. Instala Husky:
    ```bash
    npm install husky --save-dev
    ```

2. Instala CommitLint y su configuración convencional:
    ```bash
    npm install @commitlint/{config-conventional,cli} --save-dev
    ```

#### Configuración de Husky

1. Añade un script de Husky en tu `package.json` para inicializar Husky:
    ```json
    {
      "scripts": {
        "prepare": "husky install"
      }
    }
    ```

2. Ejecuta el script para inicializar Husky:
    ```bash
    npm run prepare
    ```

3. Configura Husky para usar un hook `commit-msg` que ejecute CommitLint:
    ```bash
    npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
    ```

#### Configuración de CommitLint

1. Crea un archivo de configuración para CommitLint llamado `commitlint.config.js` en la raíz de tu proyecto:
    ```javascript
    // commitlint.config.js
    module.exports = {
      extends: ['@commitlint/config-conventional'],
      rules: {
        'type-enum': [
          2,
          'always',
          [
            'build',
            'chore',
            'ci',
            'docs',
            'feat',
            'fix',
            'perf',
            'refactor',
            'revert',
            'style',
            'test'
          ]
        ],
        'subject-case': [0, 'never', ['sentence-case', 'start-case', 'pascal-case', 'upper-case']]
      }
    };
    ```

#### Ejemplo de Mensaje de Commit

CommitLint validará los mensajes de commit según las reglas definidas. Un mensaje de commit válido podría ser:

```bash
feat(entorno): añadir nueva funcionalidad de autenticación
```

Si intentas hacer un commit con un mensaje que no sigue las reglas, CommitLint lo rechazará.

## Securización de Git/GitHub
`git-secrets` es una herramienta desarrollada por AWS Labs que ayuda a prevenir la inclusión accidental de secretos (como claves de API, contraseñas y otros datos sensibles) en los repositorios de Git. Funciona escaneando los commits, las ramas y los hooks de Git en busca de patrones que coincidan con secretos conocidos y bloquea los commits que contienen estos secretos.

### Características principales de `git-secrets`:

- **Prevención de commits**: Bloquea los commits que contienen secretos conocidos.
- **Hooks de Git**: Se integra con los hooks de Git para escanear automáticamente los commits antes de que se realicen.
- **Patrones personalizados**: Permite definir patrones personalizados para buscar secretos específicos.
- **Compatibilidad**: Funciona en sistemas Unix y Windows.

### Instalación
Para instalar `git-secrets`, puedes usar `brew` en macOS o clonar el repositorio y ejecutar el script de instalación en otros sistemas.

#### macOS (usando Homebrew):
```bash
brew install git-secrets
```

#### Linux/Windows:
```bash
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
sudo make install
```

### Uso
1. **Inicializar `git-secrets` en un repositorio**:
    ```bash
    git secrets --install
    ```

2. **Añadir patrones de AWS** (opcional, pero recomendado si usas servicios de AWS):
    ```bash
    git secrets --register-aws
    ```

3. **Añadir patrones personalizados**:
    ```bash
    git secrets --add '<tu_patron>'
    ```

4. **Escanear el repositorio en busca de secretos**:
    ```bash
    git secrets --scan
    ```

### Ejemplo de uso
1. Inicializa `git-secrets` en tu repositorio:
    ```bash
    git secrets --install
    ```

2. Registra patrones de AWS:
    ```bash
    git secrets --register-aws
    ```

3. Añade un patrón personalizado para buscar una clave API específica:
    ```bash
    git secrets --add 'AKIA[0-9A-Z]{16}'
    ```

4. Escanea el repositorio en busca de secretos:
    ```bash
    git secrets --scan
    ```

Con `git-secrets`, puedes asegurarte de que no se filtren datos sensibles en tu repositorio, mejorando así la seguridad de tu código y tus datos.


### Ejemplo de git-secrets

Aquí tienes un ejemplo completo de un script que configura `git-secrets` e integra sus verificaciones con los hooks de Git:

### Script de Configuración

```bash
#!/bin/bash

# Inicializa git-secrets en el repositorio
git secrets --install

# Registra patrones de AWS
git secrets --register-aws

# Añade un patrón personalizado para buscar una clave API específica
git secrets --add 'AKIA[0-9A-Z]{16}'

# Añade un patrón personalizado

 para

 buscar contraseñas comunes
git secrets --add 'password\s*=\s*["\'][^"\']*["\']'

# Configura los hooks de pre-commit, commit-msg y pre-push para usar git-secrets
git secrets --install -f -t pre-commit
git secrets --install -f -t commit-msg
git secrets --install -f -t pre-push

echo "git-secrets ha sido configurado con éxito y los hooks han sido instalados."
```

### Explicación del Script

1. **Inicializa `git-secrets` en el repositorio**:
    ```bash
    git secrets --install
    ```

2. **Registra patrones de AWS**:
    ```bash
    git secrets --register-aws
    ```

3. **Añade patrones personalizados**:
    - Para buscar una clave API específica:
        ```bash
        git secrets --add 'AKIA[0-9A-Z]{16}'
        ```
    - Para buscar contraseñas comunes:
        ```bash
        git secrets --add 'password\s*=\s*["\'][^"\']*["\']'
        ```

4. **Configura los hooks de Git**:
    - Instala el hook de `pre-commit`:
        ```bash
        git secrets --install -f -t pre-commit
        ```
    - Instala el hook de `commit-msg`:
        ```bash
        git secrets --install -f -t commit-msg
        ```
    - Instala el hook de `pre-push`:
        ```bash
        git secrets --install -f -t pre-push
        ```

5. **Mensaje de éxito**:
    ```bash
    echo "git-secrets ha sido configurado con éxito y los hooks han sido instalados."
    ```

### Uso del Script

Guarda el script en un archivo, por ejemplo `configurar-git-secrets.sh`, y dale permisos de ejecución:

```bash
chmod +x configurar-git-secrets.sh
```

Luego, ejecuta el script desde la raíz de tu repositorio:

```bash
./configurar-git-secrets.sh
```

Este script configurará `git-secrets` en tu repositorio, registrará patrones de AWS y personalizados, y configurará los hooks de Git para que `git-secrets` verifique automáticamente los commits, mensajes de commit y pushes.

### trufflehog: Asegurando secretos

En esta sección trabajaremos con la herramienta [trufflehog](https://github.com/trufflesecurity/trufflehog)-


#### 1.**Introducción a Trufflehog**

##### ¿Qué es Trufflehog?

Trufflehog es una herramienta de código abierto diseñada para buscar secretos expuestos, como claves de API, tokens de autenticación y contraseñas, en repositorios de código, sistemas de control de versiones y otras fuentes de datos. Esta herramienta fue creada para ayudar a los desarrolladores y equipos de seguridad a identificar de manera rápida y eficiente posibles fugas de información sensible dentro de su código o historial de commits.

Trufflehog funciona escaneando archivos y repositorios en busca de patrones comunes de secretos y puede analizar no solo el contenido actual, sino también el historial completo de un repositorio, buscando cambios pasados que puedan haber expuesto información confidencial en algún momento.

##### ¿Por qué usar Trufflehog?

- **Detección automática de secretos**: En proyectos grandes, es fácil que se filtren accidentalmente secretos en el código. Trufflehog ayuda a identificar esas fugas de forma automática y rápida.
- **Análisis profundo del historial**: No solo revisa el código actual, sino también el historial de commits, lo que permite encontrar secretos que pudieron haber sido expuestos en versiones anteriores y que podrían ser vulnerables.
- **Integración en pipelines CI/CD**: Al integrarse en pipelines de integración continua, Trufflehog permite detectar y prevenir la inclusión de secretos desde las primeras fases del ciclo de desarrollo.
- **Ahorro de tiempo**: En lugar de revisar manualmente miles de líneas de código, Trufflehog automatiza el proceso, permitiendo a los equipos centrarse en la solución de problemas y mejoras de seguridad.
- **Mitigación de riesgos**: Identificar y eliminar secretos del código reduce significativamente el riesgo de que esos secretos sean explotados por atacantes, especialmente en repositorios públicos.

Trufflehog es particularmente útil en entornos de desarrollo colaborativo donde múltiples desarrolladores trabajan en un mismo proyecto, asegurando que ningún secreto quede expuesto en el código fuente.



#### 2. **Instalación de trufflehog**
 
 Para instalar la herramienta consultemos la documentación oficial donde se plantean diferentes modos de instalarla.

 [https://github.com/trufflesecurity/trufflehog?tab=readme-ov-file#floppy_disk-installation](https://github.com/trufflesecurity/trufflehog?tab=readme-ov-file#floppy_disk-installation)

#### 3. **Configuración de trufflehog**

  - **Configuración básica**
    
    Una vez que trufflehog está instalado, puedes comenzar a usarlo con configuraciones básicas para escanear tus repositorios en busca de secretos. Aquí hay algunos ejemplos de cómo configurar y ejecutar trufflehog:

    1. **Escaneo de un repositorio local**:
   
        ```bash
        trufflehog filesystem --path /ruta/a/tu/repositorio
        ```
        Este comando escaneará el repositorio local ubicado en `/ruta/a/tu/repositorio` en busca de posibles secretos.

    1. **Escaneo de un repositorio remoto**:
        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git
        ```

        Este comando escaneará el repositorio remoto especificado en la URL en busca de posibles secretos.

    2. **Especificar patrones de búsqueda**:
        Puedes especificar patrones de búsqueda personalizados utilizando el parámetro `--rules`:

        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git --rules /ruta/a/tus/reglas.json
        ```

        El archivo `reglas.json` debe contener las expresiones regulares que deseas utilizar para buscar secretos.

 - **Configuración avanzada**
  
    Para configuraciones más avanzadas, trufflehog ofrece varias opciones y parámetros que puedes ajustar según tus necesidades:

    1. **Especificar profundidad de escaneo**:
        Puedes limitar la profundidad del escaneo en el historial de commits utilizando el parámetro `--depth`:
        
        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git --depth 50
        ```
        
        Este comando escaneará solo los últimos 50 commits del repositorio.

    2. **Excluir archivos o directorios específicos**:
            
        Puedes excluir ciertos archivos o directorios del escaneo utilizando el parámetro `--exclude-paths`:
            
        ```bash
        trufflehog filesystem --path /ruta/a/tu/repositorio --exclude-paths "node_modules,tests"
        ```
        
        Este comando excluirá los directorios `node_modules` y `tests` del escaneo.

     3. **Salida en formato JSON**:
        Puedes obtener los resultados del escaneo en formato JSON utilizando el parámetro `--json`:
        
        ```bash
        trufflehog git https://github.com/tu-usuario/tu-repositorio.git --json > resultados.json
        ```
        
        Este comando guardará los resultados del escaneo en un archivo llamado `resultados.json`.

        1. **Uso de credenciales para repositorios privados**:
            Si necesitas escanear un repositorio privado, puedes agregar la clave SSH a tu cuenta de GitHub.


#### 4. **Integración de trufflehog con Git Hooks**


##### 1. Configuración de Hooks Pre-Commit

La configuración de un hook **pre-commit** en Git es útil para asegurar que ningún secreto sea añadido al código antes de que se haga un commit. A continuación, se muestra cómo configurar Trufflehog como un hook pre-commit:

1. **Crear el archivo del hook pre-commit:**
   En el directorio de tu proyecto, crea o edita el archivo `pre-commit` en la carpeta `.git/hooks/`:

   ```bash
   touch .git/hooks/pre-commit
   chmod +x .git/hooks/pre-commit
   ```

2. Agregar el script de Trufflehog: Añade el siguiente script al archivo pre-commit para que Trufflehog escanee los cambios antes de permitir el commit:

```bash
#!/bin/bash

echo "Running Trufflehog pre-commit hook..."

# Escanea los cambios que están por ser comiteados
git diff --cached --name-only | xargs trufflehog filesystem

# Captura el código de salida de Trufflehog
RESULT=$?

if [ $RESULT -ne 0 ]; then
    echo "Trufflehog found potential secrets. Commit aborted."
    exit 1
fi

echo "No secrets found. Proceeding with commit."
exit 0
```

ste script escanea los archivos modificados que están listos para el commit (`--cached`) y, si Trufflehog encuentra algún secreto, aborta el proceso del commit.

##### 2. Configuración de Hooks Pre-Push

El hook pre-push en Git es útil para asegurarse de que no se envían secretos al repositorio remoto. Este hook se ejecuta antes de que se realice un push, permitiendo escanear el código que está a punto de ser subido.

1. Crear el archivo del hook pre-push: De forma similar, en el directorio de tu proyecto, crea o edita el archivo `pre-push` en la carpeta `.git/hooks/`:

```bash
touch .git/hooks/pre-push
chmod +x .git/hooks/pre-push
```

2. Agregar el script de Trufflehog: Inserta el siguiente script en el archivo pre-push para ejecutar Trufflehog sobre los commits que se van a hacer push:

```bash
#!/bin/bash

echo "Running Trufflehog pre-push hook..."

# Obtener el rango de commits que se van a hacer push
LOCAL=$(git rev-parse @)
REMOTE=$(git rev-parse @{u})

if [ "$LOCAL" != "$REMOTE" ]; then
    # Escanear el rango de commits que se van a hacer push
    git log $REMOTE..$LOCAL --name-only | xargs trufflehog filesystem

    # Captura el código de salida de Trufflehog
    RESULT=$?

    if [ $RESULT -ne 0 ]; then
        echo "Trufflehog found potential secrets. Push aborted."
        exit 1
    fi
fi

echo "No secrets found. Proceeding with push."
exit 0
```

Este script obtiene el rango de commits que se van a hacer push y ejecuta Trufflehog sobre esos cambios. Si se detectan secretos, el push será abortado.


#### 5. **Buenas prácticas y recomendaciones**
    - Gestión de falsos positivos
    - Actualización y mantenimiento de trufflehog




# Prácticas a Entregar

Todas las prácticas deben desarrollarse creando capturas de pantalla, ficheros auxiliares (scripts, código, etc.), y debe crearse una documentación explicando qué se ha realizado.

Esta documentación se realizará utilizando Markdown o equivalente y se entregará a través de **GitHub Classroom** del módulo **Puesta en Producción Segura**.

## Práctica 2.1. Configuración de GIT (0.50 puntos)

1. Realiza la práctica guiada para configurar GIT en tu entorno de trabajo.

## Práctica 2.2. Trabajando con GIT básico (0.5 puntos)

Haciendo uso de algún proyecto “real”  descargado desde el repositorio GitHub (consultar con el profesor antes de tomar un repositorio, puesto que debe ser de un proyecto real de gran envergadura), demostrar que sabéis usar los comandos útiles de Git para observar el repositorio de dicho proyecto (`git log`, `git diff`, `git blame` y `git show`). 

Por cada uno de estos 4 comandos, hay que evidenciar **4 capturas** (junto a su explicación, ejemplo y todo lo que se desee aportar) de diferentes usos (opciones) del mismo comando. Junto a cada captura, hay que explicar con vuestras palabras lo que se quiere **probar**, **demostrar** o **buscar** con ese comando/opción. 


-  GIT LOG

   - **Captura 1**  
      - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

   - **Captura 2**  
      - Explicación de uso y captura.

   - **Captura 3**  
      - Explicación de uso y captura.

   - **Captura 4**  
      - Explicación de uso y captura.

- GIT DIFF
    - **Captura 1**  
      - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

  - **Captura 2**  
      - Explicación de uso y captura.

  - **Captura 3**  
      - Explicación de uso y captura.

  - **Captura 4**  
      - Explicación de uso y captura.

- GIT SHOW
    - **Captura 1**  
      - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

  - **Captura 2**  
      - Explicación de uso y captura.

  - **Captura 3**  
      - Explicación de uso y captura.

  - **Captura 4**  
      - Explicación de uso y captura.
- GIT BLAME
  - **Captura 1**  
    - Ejemplo comando con opciones, marcas en las capturas, explicación de lo que se pretende mostrar/buscar/etc.

  - **Captura 2**  
      - Explicación de uso y captura.

  - **Captura 3**  
      - Explicación de uso y captura.

  - **Captura 4**  
      - Explicación de uso y captura.

Aquí tienes el texto convertido a Markdown:

## Práctica 2.3. Ejercicio Práctico: Uso de Comandos de Git y Gitflow (1.5 puntos)

### Objetivo

Demuestre con ejemplos que sabe diferenciar los siguientes conceptos:
- **Resolver conflictos**
- **Fast-forward**
- **Squash**

A partir de la figura que aparece a continuación (además, la rama `master` debe mantenerse en un repositorio GitHub privado), tenéis que hacer uso de **TODOS** los comandos y las opciones más útiles, para construir el siguiente proyecto siguiendo **Gitflow** (no usar extensiones de Gitflow, únicamente los comandos explicados en clase).

![Ejercicio](./L2/exercise-git-flow.png)

> **Recomendación**: Antes de comenzar, leer el [siguiente enlace](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow) para hacerse una idea clara de lo que se debe hacer.


### Requisitos

- **Comandos anteriores**: No es necesario evidenciar (captura, comando y explicación) los comandos del ejercicio anterior (número 1), aunque **deben ser usados** para evidenciar los comandos de este ejercicio.
- La **primera vez** que aparezca un nuevo comando, explicar claramente qué realiza.
- El alumno tiene libertad para crear el guion que crea oportuno para explicar/evidenciar esta práctica (tipo de ficheros a usar -JavaScript, PHP, etc.-, número de ficheros, etc.).
- **Evidenciar el uso del fichero `.gitignore`** con algunos archivos/directorios.
- Los **commits** deben llamarse (C1-XXxx, C2-XXxx), los **tags** (v0.1XXxx, etc.) y las **ramas** (masterXXxx, developXXxx, feature1XXxx, etc.).
- Hacer uso ocasionalmente de la pestaña **Insights > Network** en GitHub para visualizar la evolución de las ramas.
- En **una o dos ocasiones**, crear un fichero vía web y actualizar el repositorio local.
- Simular que las ramas **feature** son realizadas por otro programador (**Programador Nº 2** para simularlo correctamente).
- Al generar un **tag** y subirlo al repositorio remoto, compruebe que se ha creado correctamente (fichero .zip, etc.).


### Secuencia del Proyecto

Una posible secuencia del proyecto podría ser la siguiente (obtenido del libro _Git, Controle la gestión de sus versiones - Samuel Dazón_):

1. **C1 (master)**: Primer commit del proyecto. Este commit añade la base del proyecto en el repositorio. _(Tag: versión v1.0.0)._
2. **C2 (develop)**: Creación de la rama `develop`. Después de este commit, las dos ramas principales están creadas.
3. **C3 (graph_employee)**: Creación de una rama para añadir gráficos en la página de los trabajadores (creado por Programador Nº 2).
4. **C4 (hotfix-negative-time)**: Creación de una rama para gestionar el caso en que un trabajador introduzca un valor negativo en una tarea.
5. **C5 (master)**: Integración del parche en la rama `master`. _(Tag: versión v1.0.1)._
6. **C6 (develop)**: Integración del parche en la rama `develop`.
7. **C7 (task_type)**: Creación de una rama para añadir el tipo de tareas.
8. **C8 (graph_employee)**: Último commit de la rama `graph_employee`.
9. **C9 (develop)**: Integración de la rama `graph_employee` en `develop`. Se elimina la rama `graph_employee`.
10. **C10 (task_type)**: Último commit de la rama `task_type`.
11. **C11 (develop)**: Integración de la rama `task_type` en `develop`.
12. **C12 (release-V1.1)**: Creación de la rama para la versión 1.1.
13. **C13 (export_csv)**: Creación de una rama para agregar una funcionalidad de exportar a CSV las tareas de un empleado durante un periodo.
14. **C14 (release-V1.1)**: Parche para corregir un error en el gráfico cuando el trabajador no tiene tareas.
15. **C15 (master)**: Integración de la versión 1.1 en la rama `master`. _(Tag: versión v1.1)._
16. **C16 (develop)**: Integración del parche de la versión 1.1 en la rama `develop`.
17. **C17 (export_csv)**: Último commit de la rama `export_csv`.
18. **C18 (develop)**: Integración de la rama `export_csv` en `develop`.


### Instrucciones Adicionales

1. **Muestre el estado final de las ramas creadas** con el software gratuito **SourceTree**.
2. **Muestre el estado final de las ramas creadas** con capturas del repositorio GitHub.
3. **Aspectos a evitar**: 
    - Capturas poco claras.
    - No seguir las normas de presentación.
    - Explicación desordenada (use sangrías, tabulaciones, numeración, viñetas, etc.).
    - Faltan comandos.

- **Claridad en la presentación**. Uso de sangrías, capturas concluyentes, con marcas adecuadas y concretas.
- **Explicación y uso del fichero `.gitignore`**.
- **Uso y explicación de comandos**. Adecuado uso y explicación (una vez) de los comandos: `clone`, `init`, `push`, `pull`, `commit`, `merge`, `checkout`, `branch`, `status`, `tag`.
- **Uso de `--graph`** para ver la evolución en cada commit realizado.
- **Capturas del repositorio**. Mínimo dos capturas del repositorio final/parcial de la evolución de las ramas mediante **SourceTree**.
- **Capturas en GitHub** . Mínimo dos capturas del repositorio final/parcial de la evolución de las ramas.
- **Originalidad en el guion** 


## Práctica 2.4. Configurando hooks de GIT (1 punto)

En esta práctica, aprenderás a configurar y utilizar hooks en GIT para automatizar tareas comunes en tu flujo de trabajo. Los hooks son scripts que GIT ejecuta automáticamente en ciertos puntos del ciclo de vida de los commits y otras acciones de GIT. 

#### Objetivos:
1. Configurar un hook pre-commit para verificar el formato del código antes de permitir un commit.
2. Configurar un hook commit-msg para asegurar que los mensajes de commit sigan un formato específico.
3. Configurar un hook post-merge para ejecutar pruebas automatizadas después de una fusión.

#### Instrucciones:
1. Crea un repositorio GIT nuevo o utiliza uno existente.
2. Navega al directorio `.git/hooks` dentro de tu repositorio.
3. Copia y pega los siguientes scripts en los archivos correspondientes dentro del directorio `.git/hooks`:

    - **pre-commit**:
      ```sh
      #!/bin/sh
      echo "Ejecutando pre-commit hook"
      # Aquí puedes agregar comandos para verificar el formato del código
      exit 0
      ```

    - **commit-msg**:
      ```sh
      #!/bin/sh
      echo "Ejecutando commit-msg hook"
      # Aquí puedes agregar comandos para validar el formato del mensaje de commit
      exit 0
      ```

    - **post-merge**:
      ```sh
      #!/bin/sh
      echo "Ejecutando post-merge hook"
      # Aquí puedes agregar comandos para ejecutar pruebas automatizadas
      exit 0
      ```

4. Asegúrate de que los scripts sean ejecutables:
    ```sh
    chmod +x .git/hooks/pre-commit
    chmod +x .git/hooks/commit-msg
    chmod +x .git/hooks/post-merge
    ```

5. Realiza pruebas para verificar que los hooks funcionan correctamente.

#### Entregables:
- Capturas de pantalla o registros de la terminal que muestren la ejecución de cada hook.
- Los scripts utilizados para cada hook.
- Un breve informe describiendo los pasos realizados y cualquier problema encontrado.


## Práctica 2.5. Configurando hooks usando Husky (1 punto)

En esta práctica, aprenderás a configurar y utilizar Husky para gestionar hooks en GIT de manera más sencilla y eficiente. Husky es una herramienta que facilita la creación y gestión de hooks en GIT, permitiendo automatizar tareas comunes en tu flujo de trabajo.

#### Objetivos:
1. Instalar Husky en un proyecto GIT.
2. Configurar un hook pre-commit para verificar el formato del código antes de permitir un commit.
3. Configurar un hook commit-msg para asegurar que los mensajes de commit sigan un formato específico.
4. Configurar un hook post-merge para ejecutar pruebas automatizadas después de una fusión.

#### Instrucciones:
1. Crea un repositorio GIT nuevo o utiliza uno existente.
2. Asegúrate de tener Node.js y npm instalados en tu sistema.
3. Instala Husky en tu proyecto:
    ```sh
    npm install husky --save-dev
    ```

4. Inicializa Husky en tu proyecto:
    ```sh
    npx husky install
    ```

5. Configura los hooks utilizando Husky:
    - **pre-commit**:
      ```sh
      npx husky add .husky/pre-commit "echo 'Ejecutando pre-commit hook'; # Aquí puedes agregar comandos para verificar el formato del código"
      ```

    - **commit-msg**:
      ```sh
      npx husky add .husky/commit-msg "echo 'Ejecutando commit-msg hook'; # Aquí puedes agregar comandos para validar el formato del mensaje de commit"
      ```

    - **post-merge**:
      ```sh
      npx husky add .husky/post-merge "echo 'Ejecutando post-merge hook'; # Aquí puedes agregar comandos para ejecutar pruebas automatizadas"
      ```

6. Realiza pruebas para verificar que los hooks funcionan correctamente.

#### Entregables:
- Capturas de pantalla o registros de la terminal que muestren la ejecución de cada hook.
- Los scripts utilizados para cada hook.
- Un breve informe describiendo los pasos realizados y cualquier problema encontrado.


## Práctica 2.6. Firmando commits (1.5 puntos)

En esta práctica, aprenderás a firmar tus commits en GIT utilizando GPG (GNU Privacy Guard) para asegurar la autenticidad e integridad de tus cambios. Firmar tus commits añade una capa adicional de seguridad y confianza, permitiendo a otros verificar que los commits realmente provienen de ti. Además, podrás ver la verificación de tus commits firmados en plataformas como GitHub.

#### Objetivos:
1. Generar una clave GPG si no tienes una.
2. Configurar GIT para usar tu clave GPG.
3. Firmar tus commits con GPG.
4. Verificar la firma de tus commits en GitHub.

#### Instrucciones:
1. **Generar una clave GPG**:
    - Si no tienes una clave GPG, genera una nueva clave siguiendo las instrucciones de la [documentación oficial de GPG](https://gnupg.org/documentation/).

2. **Configurar GIT para usar tu clave GPG**:
    - Exporta tu clave pública y añádela a tu cuenta de GitHub siguiendo las instrucciones de la [documentación de GitHub](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account).
    - Configura GIT para usar tu clave GPG:
      ```sh
      git config --global user.signingkey <tu_clave_gpg>
      git config --global commit.gpgSign true
      ```

3. **Firmar tus commits con GPG**:
    - Realiza un commit firmado:
      ```sh
      git commit -S -m "Tu mensaje de commit"
      ```

4. **Verificar la firma de tus commits en GitHub**:
    - Sube tus commits firmados a GitHub y verifica que aparezcan como "Verified" en la interfaz de GitHub.

#### Entregables:
- Capturas de pantalla o registros de la terminal que muestren la generación de la clave GPG y la configuración en GIT.
- Capturas de pantalla de los commits firmados y verificados en GitHub.
- Un breve informe describiendo los pasos realizados y cualquier problema encontrado.


## Práctica 2.7. Securizando GIT: Git-Secrets (1.5 puntos)

En esta práctica, aprenderás a utilizar `git-secrets` para evitar que información sensible, como claves API y contraseñas, se incluya accidentalmente en tus commits de GIT. `git-secrets` es una herramienta que escanea tus commits y previene que información sensible sea añadida al repositorio.

#### Objetivos:
1. Instalar `git-secrets` en tu sistema.
2. Configurar `git-secrets` en un repositorio GIT.
3. Añadir patrones para detectar información sensible.
4. Probar `git-secrets` para asegurarte de que funciona correctamente.

#### Instrucciones:
1. **Instalar `git-secrets`**:
    - Sigue las instrucciones de instalación para tu sistema operativo desde el [repositorio oficial de git-secrets](https://github.com/awslabs/git-secrets#installing-git-secrets).

2. **Configurar `git-secrets` en tu repositorio**:
    - Navega al directorio de tu repositorio GIT y ejecuta:
      ```sh
      git secrets --install
      git secrets --register-aws
      ```

3. **Probar `git-secrets`**:
    - Intenta hacer un commit que contenga información sensible para verificar que `git-secrets` lo detecta y bloquea:
      ```sh
      echo "AWS_SECRET_ACCESS_KEY=example" > test.txt
      git add test.txt
      git commit -m "Añadiendo clave secreta"
      ```

4. **Verificar la configuración**:
    - Asegúrate de que `git-secrets` está correctamente configurado y funcionando en tu repositorio.


5. **Añadir patrones personalizados**:
    - Puedes añadir patrones personalizados para detectar información sensible específica de tu proyecto:
      ```sh
      git secrets --add 'tu_patron_personalizado'
      ```
    - Haz comprobaciones de que se han buscado y detectado tus patrones personalizados.
  


## Práctica 2.8. Securizando GIT: Trufflehog: Local (1.25 puntos)

En esta práctica, aprenderás a utilizar TruffleHog para escanear tu repositorio GIT en busca de información sensible, como claves API y contraseñas, que puedan haberse incluido accidentalmente en tus commits. TruffleHog es una herramienta que busca patrones de información sensible en el historial de commits de tu repositorio.

#### Objetivos:
1. Instalar TruffleHog en tu sistema.
2. Configurar TruffleHog para escanear un repositorio GIT local.
3. Ejecutar TruffleHog para detectar información sensible en el historial de commits.
4. Analizar los resultados del escaneo y tomar medidas para eliminar cualquier información sensible encontrada.

#### Instrucciones:
1. **Instalar TruffleHog**

2. **Configurar TruffleHog para escanear un repositorio GIT local**:
    - Navega al directorio de tu repositorio GIT.

3. **Ejecutar TruffleHog**:
    - Ejecuta TruffleHog para escanear el historial de commits de tu repositorio:
      ```sh
      trufflehog --regex --entropy=True .
      ```

4. **Analizar los resultados del escaneo**:
    - Revisa los resultados proporcionados por TruffleHog para identificar cualquier información sensible.
    - Si se encuentra información sensible, toma las medidas necesarias para eliminarla del historial de commits (por ejemplo, utilizando `git filter-branch` o `BFG Repo-Cleaner`).

5. **Verificar la eliminación de información sensible**:
    - Asegúrate de que la información sensible ha sido eliminada correctamente y que el repositorio está limpio.
6. **Provoca una vulnerabilidad y localizala**
    - Haz un commit con credenciales que deben protegerse.
    - Haz un commit borrando la credencial.
    - Escanea y encuentra la vulnerabilidad.


## Práctica 2.9. Securizando GIT: Trufflehog: Remoto (1.25 puntos)

En esta práctica, aprenderás a utilizar TruffleHog para escanear un repositorio GIT remoto en busca de información sensible.

#### Objetivos:
1. Configurar TruffleHog para escanear un repositorio GIT remoto.
2. Ejecutar TruffleHog para detectar información sensible en el historial de commits del repositorio remoto.
3. Analizar los resultados del escaneo y tomar medidas para eliminar cualquier información sensible encontrada.

#### Instrucciones:
1. **Configurar TruffleHog para escanear un repositorio GIT remoto**:
    - Identifica la URL del repositorio GIT remoto que deseas escanear.

2. **Ejecutar TruffleHog**:
    - Ejecuta TruffleHog para escanear el historial de commits del repositorio remoto:
      ```sh
      trufflehog --regex --entropy=True <URL_del_repositorio_remoto>
      ```

3. **Analizar los resultados del escaneo**:
    - Revisa los resultados proporcionados por TruffleHog para identificar cualquier información sensible.
    - Si se encuentra información sensible, toma las medidas necesarias para eliminarla del historial de commits (por ejemplo, utilizando `git filter-branch` o `BFG Repo-Cleaner`).

4. **Verificar la eliminación de información sensible**:
    - Asegúrate de que la información sensible ha sido eliminada correctamente y que el repositorio está limpio.

## Criterios de Evaluación Asociados

| RA3  | Detecta y corrige vulnerabilidades de aplicaciones web analizando su código fuente y configurando servidores web |
|------|------------------------------------------------------------------------------------------------------------------|
| 3.e  | Se han utilizado algoritmos criptográficos seguros para almacenar las contraseñas de usuario                     |


| RA5  | Implanta sistemas seguros de desplegado de software, utilizando herramientas para la automatización de la construcción de sus elementos |
|------|---------------------------------------------------------------------------------------------------------------------------------------|
| 5.a  | Se han identificado las características, principios y objetivos de la integración del desarrollo y operación del software              |
| 5.b  | Se han implantado sistemas de control de versiones, administrando los roles y los permisos solicitados                                |
| 5.c  | Se han instalado, configurado y verificado sistemas de integración continua, conectándolos con sistemas de control de versiones        |




## Practicas investigación

### Submódulos

¿Qué son los submódulos? ¿Cómo funcionan los submódulos?

De manera práctica debes realizar las siguientes tareas:

- Crea un repositorio con submódulos.
- Utiliza submódulos (clonar, commit, log, status, y reset en submódulos).
  
[Documentación de submódulos](https://git-scm.com/book/en/v2/Git-Tools-Submodules)



### Uso avanzado: trufflehog

Realiza una investigación sobre el uso avanzado de trufflehog, se deben cubrir los siguientes puntos:

  - Gestión de falsos positivos
  - Actualización y mantenimiento de trufflehog

### Protección de credenciales.

Investigue cómo instalar y usar las herramientas:
    - El comando `git filter-branch`.
    - [`BFG Repo-Cleaner`](https://rtyley.github.io/bfg-repo-cleaner/)
    - 
### Propuesta por el estudiante

El estudiante puede proponer una práctica de investigación.
