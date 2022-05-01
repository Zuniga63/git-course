# Git

Git es un sistema de control de versiones distribuido, creado por Linus Torvald en 2005.

**Repositorio**: Es una carpeta que va a almacenar todo el historial de cambios del proyecto.

**Commits**: Es una fotografía del proyecto en un momento determinado.

## Configuración Global

`git config`

### Nombre y Correo

`git config --global user.name Julanito Martinez`
`git config --global user.email julanito@correo.com`

### Configurar editor de texto

Git por defecto utiliza un editor de texto para consola llamado **Vim**. Para utilizar por ejemplo VScode se usa:

`git config --global core.editor "code --wait"`

### Configurar la rama main por defecto

`git config --global init.defaultBranch main`

## Comando Basicos

### Inicializar el repositorio

`git init`

### Commits

~~~git
git add .
git commit -m "first commit"
~~~

### Historial de cambios

~~~git
git log
git log --oneline
git log --oneline --graph
git log --oneline --all
~~~

### Ver un commit especifico

`git show fc3b561`

Se debe especificar el identificador del commit, se puede utilizar los primeros 7 caracteres.

- Cada cambio comienza con la linea *"diff --git"*

### El estado del repositorio

`git status`

### Agregar archivos al index

`git add index.html`

### Removiendo archivo del index

`git reset HEAD index.html`

Los cambios no se pierden pero los cambios de ese archivo no se incluirán en el siguiente commit.

### Descartando los cambios

`git checkout -- index.html`

**Nota:** Para que este comando funciones debe estar fuera del index.

### Explorando los cambios

~~~git
git diff
git diff --staged
~~~

## Ignorando archivos y carpetas

### Comentarios

`# Esto es un comentario`

### Archivos

`development.log`
Ignora todos los archivos llamados *development.log* que este dentro de cualquier carpeta.
`/development.log`
Ignora solo ese archivo que es este en la raiz del proyecto.

### Uso de patrones

~~~text
# ignorar todos los archivos que terminen en .log
*.log

# excepto production.log
!production.log

# ignorar los archivos terminados en .txt dentro de la carpeta doc (pero no sus subdirectorios)
doc/*.txt

# ignorar todos los archivos terminados en .pdf dentro de la carpeta doc y sus subdirectorios
doc/**/*.pdf
~~~

## TRABAJANDO CON RAMAS

Las ramas nos permiten desvianos de la linea principal de desarrollo y son utiles para trabajar en nuevas funcionalidades, solucion de errores o realizar experimentos.

con ***git status***  podemos ver en la primera linea en que rama estamos.

### Listado de ramas

Para visualizar el listado de ramas locales se usa el siguiente comando:

`git branch`

~~~text
* main
rama-1
rama-2
~~~

La rama cin el asterisco es la rama en la que se encuentra actualmente.

### Crear una rama

`git branch mi-rama`

El comando anterior crea la rama pero no nos ubica sobre ella.

~~~git
git branch mi-rama
git checkout mi-rama
-o-
git checkout -b mi-rama
~~~

### Cambiando el nombre de una rama

`git branch -m otra-rama`

**Nota:** Para cambiar el nombre debes estár unicado sobre ella.

### Integrar los cambios de una rama en otra

~~~git
git checkout main
git merge mi-rama
~~~

### Eliminando una rama

Para eliminar una rama que ya ha sido integrada en otra se utiliza el comando **git branch -d** y para que funciones debemos está sobre la rama que fue integrada.

~~~git
git checkout master
git branch -d mi-rama
~~~

Si la rama tiene cambios no nos dejará eliminarla.

`git branch -D mi-rama`

## Repositorios remotos

### Configurar repositorio remoto

`git remote add <nombre> <url>`

### Sincronizar con el repositorio remoto

#### primera vez

`git push -u origin main`

#### posteriores

`git push`

### Clonando un repositorio

`git clone <url> [<name>]`

El comando anterior crea una copia original del repositorio, con todos los commit y el repositorio remoto en *origin*

#### Descargando otras rama

~~~git
git fetch origin
git checkout rama-1
~~~

### Actualizando una rama

Para actualizar una rama primero se debe ubicar sobre ella y ejecutar el comando:

`git pull`

el comando anterior es equivalente a:

~~~git
git fetch origin
git merge origin/master
~~~

**Nota:** Este comando solo es recomendable si no hay commit en la rama actual.

### Reemplanzao una rama local

~~~git
git fetch origin
git reset --hard origin/rama-1
~~~

### Reemplazando una rama remota

`git push -f <remoto> <rama>`

## Etiquetas

Las etiquetas sirven para representar algun momento importante del proyecto.

### Etiquetas anotadas

`git tag -a v1.4.2 -m "Version 1.4.2`

El nombre va despues de la opción -a, y la opción m define el mensaje que se le va a adjudicar.

### Etiquetas ligeras

`git tag v.1.4.2`

### Listando las etiquetas

~~~git
git tag

-o-

git show <nombre etiqueta>
~~~

### Eliminando las etiquetas

`git tag -d v1.4.2`

### Etiquetas y repositorios remotos

~~~git
git push origin <nombreEtiqueta>
~~~

Si hay varias etiquetas

~~~git
git push origin --tags
~~~

### Eliminando etiquetas remotas

`git push --delete origin v.1.4.2`

***

## Reescribiendo la historia

**Nota:** Nunca se debe reescribir la historia sobre ramas publicas y sobre las que otros colaboradores ya puedan estar trabajando.

### Cambiando el ultimo commit

`git commit --amend`

El camando anterior abre el editor de texto por defecto para cambiar el mensaje.

Si no se quiere cambiar el mensaje
`git commit --amend --no-edit`

**Nota:** EN realizad git no cambia el ultimo commit, solo crea uno nuevo y descarta el anterior.

### Descartar el ultimo commit

`git reset --hard HEAD^`

**Nota:** Si se omite la opción *--hard* los cambios del commit van a quedar en el espacio de trabajo.

Si se quiere deshacer mas commit se simplemente se hace:

~~~git
git reset --hard HEAD^^^

-o-

git reset --hard HEAD~3
~~~

***

## Stashing

### Agregando cambios en el stash

~~~git
git add .
git stash
~~~

### Listando las entradas en el stash

`git stash list`

### Reucperan los cambios en el stash

~~~git
git stash pop

-o-

git stash apply
~~~

A efectos practicos los dos comandos funcionan igual, solo que el primero elimina la entrada y el segundo la conserva en el stash.

Se puede aplicar los cambios de otras entradas usando **stash@{1}**

~~~git
git stash pop stash@{1}

-o-


git stash apply stash@{1}
~~~

### Ver los cambios de una entrada del stash

`git stash show -p stash@{0}`

### Creando una rama a partir de un stash

`git stash branch rama-stash stash@{1}`

### Eliminando un entrada del stash

`git stash drop stash@{1}`

O eliminar todas las entradas

`git stash clear`
