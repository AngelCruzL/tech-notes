---
tags: utilities
---

---

[Git - Reference (git-scm.com)](https://git-scm.com/docs)

---

# ¿Qué es Git?

[Git](https://git-scm.com/) es un software de control de versiones, su propósito es llevar registro de los cambios en archivos de computadora y coordinar el trabajo que varias personas realizan sobre archivos compartidos.

# ¿Qué es un control de versiones?

Un control de versiones es un sistema que registra cada cambio que se realiza en un archivo, esto te permite mantener un historial de todos los cambios que se han realizado en los archivos de un repositorio.

La importancia de utilizar un sistema de control de versiones no solo se limita a crear este historial (que también podría verse como una copia de seguridad), sino que también permite que otras personas puedan colaborar en el desarrollo de un mismo proyecto. Facilitando así la sincronización de los cambios entre las diferentes versiones de los diferentes equipos de trabajo.

# Configuración básica

## Identificación

Para identificarse en git es necesario ejecutar los siguientes comandos:
```sh
git config --global user.email "correo@correo.com"
git config --global user.name "usuario"
```

## Rama por defecto

Adicionalmente a este proceso de identificación se puede agregar el siguiente comando, el cual por defecto cambia la rama `master` a `main` en la inicialización del repositorio:

```sh
git config --global init.defaultBranch main
```

La siguiente configuración sirve para crear los pull con la opción de rebase:

```sh
git config --global pull.rebase true
```

En caso de querer corroborar las configuraciones globales de git se puede utilizar el siguiente comando:

```sh
git config --global -e
```
Este comando permite abrir en un editor de texto (integrado en la terminal o linea de comandos) las configuraciones globales de git.

# ¿Cómo trabaja git?

Git trabaja con base en commits, estos pueden verse como snapshots (o fotografías) de los archivos en un momento determinado en el tiempo. Es gracias a los commits que git puede realizar el control de versiones pues cada uno de estos es una version del repositorio (conjunto de archivos dentro del mismo directorio o proyecto).

# Primeros pasos en git

Para iniciar un repositorio local se utiliza el siguiente comando:

```sh
git init
```

Este comando crea la carpeta oculta `.git/` que será el lugar en el que se almacene el historial de versiones, se recomienda no modificar este directorio de manera manual.

Para crear un commit es necesario conocer el concepto de stage o área de staging, esta área de staging es un espacio temporal donde podemos mover los archivos modificados del directorio de trabajo a un estado de **preparado**, desde este estado ya es posible pasarlos a **confirmados**, y que así queden grabados en un commit dentro del repositorio.

# Comandos básicos

Dentro de los comandos básicos de git se encuentran los siguientes:

| Comando                           | Descripción                                                                                                                                                                          |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `git add <file name>`             | Sirve para añadir archivos al stage.                                                                                                                                                 |
| `git add css/`                    | Sirve para añadir al stage todos los archivos de la carpeta css.                                                                                                                     |
| `git add -A`                      | Sirve para añadir al stage todos los archivos no guardados de todo el proyecto.                                                                                                      |
| `git add .`                       | Sirve para añadir al stage todos los archivos no guardados del directorio de donde te encuentras.                                                                                    |
| `git status`                      | Realiza el seguimiento de los archivos del repositorio, muestra en color rojo aquellos que no estén registrados o que hayan recibido algún cambio desde el ultimo commit a la fecha. |
| `git commit -m "Primer commit"`   | Creará el commit del proyecto con el título **Primer commit** con los archivos que se encuentren en el stage.                                                                        |
| `git commit -am "Segundo commit"` | Creará un commit con todos los archivos a los que git les este dando seguimiento y hayan sufrido alguna modificación desde algún commit anterior.                                    |

Dentro de los comandos de git generalmente al usar `--` es seguido de una palabra y la usar `-` son letras que suelen ser abreviaciones de un comando.

| Comando                             | Descripción                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------------------ |
| `git checkout -- .`                 | Deshace los cambios de todos los archivos desde el último commit realizado                 |
| `git checkout -- sass/_footer.scss` | Deshace los cambios del archivo `footer.scss` de la carpeta `sass/` desde el último commit |
| `git reset *.xml`                   | Sirve para excluir del stage a todos los archivos con extension xml                        |
| `git add "*.txt"`                   | Sirve para añadir al stage todos los archivos txt de todo el proyecto                      |
| `git add *.txt`                     | Sirve para añadir al stage todos los archivos txt de del directorio actual                 |
| `git add -all`                      | Agrega al stage todos los archivos que se hayan modificado                                 |
| `git add docs/*.pdf`                | Sirve para añadir al stage archivos de la carpeta `docs/` con extensión `.pdf`             |

# Commits

Los commits son como fotografías de como se encuentra nuestro proyecto en ese momento, capturando tal cual están los archivos.

Entre los comandos más comunes para commits se encuentran:

| Comando                                          | Descripción                                                                                                                                                                                                                        |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `git log`                                        | Muestra los commits de los más recientes a los más antiguos. Dentro de los commits se ve al lado de uno la palabra **HEAD**, esta palabra apunta al último commit de la rama actual                                                |
| `git log --oneline`                              | Muestra los commits de manera más reducida                                                                                                                                                                                         |
| `git log --oneline --decorate --all --graph`     | Este comando muestra los commits de una manera más reducida y agregando ciertos detalles, `all` muestra información referente a las diferentes ramas y `decorate-graph` muestra de manera más gráfica las ramificaciones del árbol |
| `git status -s`                                  | Muestra en modo silenciado los archivos modificados y en stage (esto quiere decir que mostrara de un lado del nombre del archivo su estado actual)                                                                                 |
| `git status -s -b`                               | Nos muestra en modo silenciado los archivos y la rama de estos                                                                                                                                                                     |
| `git diff`                                       | Muestra los cambios entre el commit anterior y el periodo actual                                                                                                                                                                   |
| `git diff staged`                                | Muestra los cambios entre el commit anterior y el periodo actual de los archivos que se encuentren en el staged.                                                                                                                   |
| `git rm --cached index.html`                     | Retira del stage el archivo index.html                                                                                                                                                                                             |
| `git reset HEAD readme.md`                       | Quita del staged el archive readme.md.                                                                                                                                                                                             |
| `git checkout -- readme.md`                      | Revertir los cambios del archivo readme desde el ultimo commit.                                                                                                                                                                    |
| `git commit --amend -m "Actualizamos el readme"` | Con esto modificamos el mensaje del último commit.                                                                                                                                                                                 |
| `git commit`                                     | Añade lo del staged y nos permite introducir un mensaje multilínea, para salir se usa la combinación de teclas: esc, w(write, para guardar) y q(para salir).                                                                       |

## Alias para commits

Existen maneras de personalizar comandos dentro de GIT, esto se logra gracias a los alias, los cuales son maneras de crear referencia a ciertos comandos, para uso propio suelo utilizar los siguientes:

Con este comando creamos el alias, el cual será `lg`, al ejecutarse mostrará lo mismo que ejecutar el comando entre comillas, el cual es mostrar de manera gráfica las ramificaciones de la rama y los commits en esta.
```bash
git config --global alias.lg "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"
```

Para este siguiente comando nos muestra el status y la rama actual en la que nos encontramos:
```bash
git config --global alias.s "status -s -b"
```

Para poder deshacer el último commit se puede usar el siguiente alias:
```bash
git config --global alias.undo 'reset --soft HEAD^'
```

El siguiente commit esta hecho para simplificar la adición de archivos al stage por fragmentos:
```bash
git config --global alias.ap "add -p"
```

## Edición de commits

Una de las bondades de git es que te permite realizar "viajes en el tiempo" dentro del repositorio, pues puedes navegar entre los commits para ver como se encontraba el repositorio y sus archivos en ese momento.

`git reset --soft HEAD^` es un comando que permite deshacer el ultimo commit, este no tiene cambios destructivos pues los archivos que se vean afectados por deshacer el ultimo commit se mantendrán con la misma información que tenían antes de ejecutar el comando, pudiendo asi hacer un commit tomando en cuenta el estado actual.

Con el comando `git reset --mixed e6dca05` se tiene un comportamiento similar al `--soft`, en este caso el commit no apunta al `HEAD`, si no a otro commit con el hash `e6dca05`, la diferencia entre el `--soft` y el `--mixed` es que el segundo quita los cambios del stage, por lo que es necesario volver a incluirlos.
En este caso en particular quita los archivos del stage que estén arriba del commit con el hash `e6dca05`.

- `git reset --hard e6dca05` es el último commit para la edición de estos. Tiene la bandera `--hard`, esto indica que eliminara los archivos y las modificaciones realizados desde el commit con el hash `e6dca05` en adelante.

- `git reflog`: Sirve para ver los registros del repositorio, incluyendo los de los archivos borrados.

- `git reset --hard …`: Sirve para restaurar archivos borrados, con lo que a la dirección … que usemos será a la que regresaremos los archivos.

- `git mv destruir-mundo.txt salvar-mundo.txt`: Con este comando re-nombramos desde la terminal de git el archivo destruir mundo a salvar mundo, con esto el archivo, aunque cambie de nombre mantiene el historial de cambios en el repositorio.

- `git rm salvar-mundo.txt`: Con este comando borramos o removemos el archivo salvar mundo, después de esto al hacer un git s, podremos observar que el archivo posee una letra D en verde, lo cual hará que el archivo aun no esté eliminado definitivamente (pues se encuentra en el staged), para eliminarlo definitivamente debemos hacer un commit, siempre podremos volver a el con el uso de los saltos en el tiempo.

- `git add -u`: Sirve para actualizar todos los ficheros, ya sean renombrados o eliminados fuera de la terminal de git, después de esto se tiene que hacer un git add -A y posteriormente un commit para que git reconozca los cambios en los archivos.

Para poder ignorar algún archivo o carpetas que se encuentren dentro del repositorio de git tenemos que crear un archivo llamado `.gitignore`, en el cual dentro por línea debemos de escribir el nombre del archivo o carpeta que se desea ignorar por parte del repositorio, a partir de ahí se debe de hacer un `git add -A` y luego un commit para que el repositorio este consiente de los archivos a ignorar.

- `git checkout -- .`: Con este comando se restablece el contenido de los archivos a los que se les da seguimiento como estaban en el último commit

# Ramas (branches)

Las ramas son bifurcaciones en el repositorio, por ejemplo si tenemos un repositorio con dos ramas, una llamada `master` y otra llamada `dev`, en el repositorio podemos ver que los commits que se hacen en `master` se guardan en la rama `master` y los commits que se hacen en `dev` se guardan en la rama `dev`, de esta manera se permite el trabajo paralelo sobre una misma base de código.

![[branch-example.png]]

| Comando                         | Descripción                                                                                          |
| ------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `git branch rama-villanos`      | Sirve para crear la rama “rama-villanos”                                                             |
| `git branch`                    | Ver la rama en la que nos encontramos                                                                |
| `git checkout rama-villanos`    | Cambiar a la rama “rama-villanos”                                                                    |
| `git diff rama-villanos master` | Que diferencias hay entre la rama principal y rama-villanos, (en rojo mostrará mejor estos detalles) |

Para unir dos ramas es necesario estar situado en la rama sobre la cual queremos mantener los cambios.

| Comando                        | Descripción                                                         |
| ------------------------------ | ------------------------------------------------------------------- |
| `git merge rama-villanos`      | Estando en la rama principal fusiona rama villanos con la principal |
| `git branch -d rama-villanos`  | Borra la rama “rama-villanos”                                       |
| `git checkout -b rama-villano` | Crea y nos desplaza a “rama-villano”                                |

# Tags

Un tag hace referencia a un commit y a todo el estado del proyecto en ese momento en especifico.

| Comando                                                   | Descripción                                                                                 |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `git tag superRelease`                                    | Crea una etiqueta llamada "superRelease" en el lugar donde apunta el header                 |
| `git tag`                                                 | Muestra todas las etiquetas creadas                                                         |
| `git tag -d superRelease`                                 | Elimina la etiqueta "superRelease"                                                          |
| `git tag -a v1.0.0 -m "Versión 1.0.0"`                    | Crea un tag con la nota “v1.0.0”, mensaje "Versión 1.0.0" a donde apunta el head            |
| `git tag -a v0.1.0 345d7de -m "Version alpha del código"` | Crea un tag con la nota “v0.1.0”, mensaje "Versión alpha del código" a donde apunta 345d7de |
| `git show v0.1.0`                                         | Muestra los detalles del tag v0.1.0                                                         |

## Versiones para las etiquetas

| Versión       | Ejemplo de etiqueta | Descripción de la versión                                                           |
| ------------- | ------------------- | ----------------------------------------------------------------------------------- |
| First release | 1.0.0               | Inicia con 1.0.0                                                                    |
| Patch release | 1.0.1               | Incrementa el tercer dígito                                                         |
| Minor release | 1.1.0               | Incrementa el segundo dígito y reinicia el conteo del último dígito a cero          |
| Major release | 2.0.0               | Incrementa el primer dígito y reinicia el conteo del segundo y tercer dígito a cero |

# Stash

El stash es una zona temporal en la cual se pueden agregar modificaciones de los archivos, nos sirve para tomar algunos archivos y colocarlos en una línea temporal de modo que no interfiere con los commits actuales y no se pierden los cambios realizados.

Entre los comandos más comunes para el uso del stash se encuentran:

| Comando                                            | Descripción                                                                                                                                                                                      |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `git stash / git stash save`                       | Este comando permite crea un stash, guardando todo el directorio en el que se este trabajando (incluso los archivos a los que git no les esta dando seguimiento) como un WIP (Work In Progress). |
| `git stash list`                                   | Muestra el registro del stash (todos los stash que se hayan realizado), se recomienda no tener más de un stash y optar por trabajar con ramas para así evitar conflictos en los archivos.        |
| `git stash apply`                                  | Restaura el ultimo registro en el stash                                                                                                                                                          |
| `git stash drop`                                   | Elimina el ultimo stash (posición 0: `stash@{0}`), puede recibir como argumento el stash                                                                                                         |
| `git stash pop`                                    | Restaura la ultima entrada del stash y la borra, al restaurar la ultima entrada carga los cambios a los archivos a los que se le hayan realizado modificaciones.                                 |
| `git stash save --keep-index`                      | keep index: Guarda todos los archivos menos los que están en el stage                                                                                                                            |
| `git stash save --include-untracked`               | Include-untracked: Incluye todos los archivos, junto a los que git no les da seguimiento                                                                                                         |
| `git stash list –stat`                             | Mas información de cada entrada del stash                                                                                                                                                        |
| `git stash show`                                   | Muestra de manera detallada cada stash                                                                                                                                                           |
| `git stash save 'Mensaje personalizado del stash'` | Guarda el stash con el mensaje personalizado                                                                                                                                                     |
| `git stash save “…”`                               | Agrega el mensaje … a la entrada actual del stash                                                                                                                                                |
| `git stash clear`                                  | Borra de manera irrecuperable todas las entradas del stash                                                                                                                                       |

# Rebase

El rebase es una herramienta que permite ordenar los commits, tambien sirve para corregir sus mensajes y para unir o separar los commits.

- `git rebase master`: Estando en cualquier rama distinta a la rama master actualiza las referencias del inicio de la rama con las del master, permitiendo asi reorganizar la información.

- `git rebase -i HEAD~4`: Crea un rebase interactivo con el cual puedes realizar múltiples tareas, entre ellas squash (s), la cuál se utiliza para unir los dos últimos commits. Otra tarea importante es reword (r), la cual sirve para modificar el mensaje de un commit.

Para poder separar un commit se debe hacer lo siguiente:

- `git rebase -i HEAD~3`: Esto creara un rebase interactivo tomando los ultimos 3 commits. De ahí al commit que se quiera modificar se le agrega el prefijo `edit` (o `e`) y se prosigue con el rebase.
- `git reset HEAD^`: Deshace el último commit sin revertir los cambios que se guardaron de este, con esto ya podemos volver a agregar al stage los archivos de manera independiente y realizar las respectivas modificaciones.

- `git rebase --continue` sirve para terminar con el rebase, esto es necesario tras modificar el commit, pues por defecto git no termina el rebase en caso de que se quieran realizar modificaciones adicionales.

# Fetch

El comando `git fetch` sirve para traer los cambios del repositorio remoto al repositorio local
