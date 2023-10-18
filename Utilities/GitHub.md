---
tags: utilities
---
----

[GitHub Docs](https://docs.github.com/en)

----

# ¿Qué es GitHub?

Es un servicio online el cual nos permite mantener un control de versiones como [[Git]], cuyo propósito es llevar un registro de los cambios en archivos.

Para ello es necesario identificarse en git
![[Git#Identificación]]

# Comandos básicos

Estos comandos se ejecutan dentro de la terminal de comandos:

```bash
git push
		Sube los cambios del repositorio al servidor

git pull
		Descarga los cambios del servidor

git remote -v
		Muestra de donde se descargan y cargan los archivos del repositorio

git push --tags
		Subir los tags a github

git clone https…
		Nos permite clonar el repositorio de la web https…

git fetch
		Sirve para mostrar si hay cambios en el repositorio en línea respecto al local
```

Se recomienda hacer un `git fetch` en vez de un pull ya que el pull es destructivo al hacer el merge de manera automática, de aquí se realiza el `pull` y luego un `push`.

# Avanzado

Para poder colaborar en un repositorio de GitHub necesitamos hacer uso del fork, el cual consiste en tomar el repositorio original y clonarlo en un lugar en el que nosotros tenemos total acceso a ese repositorio

Un fork puede ser considerado como una rama adicional independiente de la principal, pero con la posibilidad de poder volver a ser unida a la rama padre o la rama principal.

Para subir los cambios a un repositorio original desde uno que hayamos hecho folk necesitamos hacer uso del pull request

También se puede realizar un folk, lo que nos permitirá tener una copia del repositorio original en nuestra cuenta de github y realizar commits localmente, para después realizar un pull request del proyecto local (el folk creado anteriormente) al repositorio original.

Para esto se usa los comandos:

```bash
git remote add *nombre del repositorio original* *url del repositorio*

git pull *nombre del repositorio original* *nombre de la rama a descargar*
```

Para el flujo de trabajo mediante pull request se debe crear otra rama en la que se dejarán las modificaciones

```bash
git checkout -b *nombre de la rama*
```

de ahí se realiza el commit en la nueva rama.

Tras esto si se regresa al master no se verán estos cambios, pues estos cambios sucedieron en una rama independiente.

Para subir la nueva rama en la que trabajamos a Github (y que sea revisada por el resto del equipo) se usa:

```bash
git push origin *nombre de la rama*
        Carga la rama a github con la posibilidad de hacer un pull request desde la página
```

De aquí se puede aceptar el pull request desde la página de GitHub, y de ahí se procede a hacer un merge para unir las ramas, tras esto se debe borrar la rama en el equipo donde se creó (es importante que se borré una vez que los cambios en github ya hayan sido aceptados, de lo contrario se perderá el trabajo). Para esto se borra con el siguiente comando:

```bash
git branch -D *Nombre de la rama*
```

De ahí se procede a realizar un git pull para poder traer los cambios desde el repositorio en GitHub

Si se revisa el contenido de una rama ajena al equipo donde se ejecutan los comandos y se actualiza y busca juntar con el master se debe hacer lo siguiente:

Primero descargar la rama con
```bash
git pull --all
```

De ahí se hace un
```bash
git branch -a
```
Esto nos mostrará todas las ramas incluyendo las remotas, estas ramas remotas poseen las mismas propiedades que las locales

De ahí se realizan los cambios que se requieran en los archivos generados en esa rama, se hace un commit y de ahí se realiza el siguiente comando:

```bash
git push origin *nombre de la rama*
```

Y en github se hace el pull request

De aquí se puede ir generando basura (ramas no deseadas y/o vacías), estas se pueden eliminar desde GitHub o en la terminal de comandos con:

```bash
git branch -a
        Muestra todas las ramas (incluyendo las remotas)
```

De ahí nos movemos a una rama ajena a la que se desea borrar y de borra con el comando

```bash
git branch -D *Nombre de la rama*
```

De ahí se puede realizar un git pull para obtener los cambios localmente (esto hará un fast fordward)

Ahora para borrar ramas que sean remotas se usa el siguiente comando

```bash
git push origin :*nombre de la rama*
```

En caso de que mande un error al borrar una rama que haya sido borrada desde github o se desee actualizar y limpiar las ramas se usa el comando:

```bash
git remote prune origin
```

# Ramas de producción

Las ramas de producción son aquellas independientes al master en donde se realizaron cambios a una versión especifica de la aplicación a desarrollar, estas ramas de producción sirven para:

- Cuando le damos soporte prolongado a una versión en particular
- Arreglos rápidos y urgentes
- Cuando subimos a producción, pero constantemente tenemos que estar dando soporte y arreglos cada vez que hay un nuevo despliegue de la aplicación

Para crear una rama de producción se crea una rama normalmente y se realiza el commit de los archivos que hayan sido modificados, de aquí se realiza un push de la rama y estando en esta rama se crea un tag.

Los Releases a diferencia de los tags nos permiten compartir archivos binarios y se puede proveer información adicional sobre ese release en particular

Los issues de github pueden cerrarse mediante terminal con un commit haciendo uso de la palabra fixes seguida del numeral del issue, por ejemplo:

```bash
git commit -am “Agregamos …. Fixes #5”
```

Ademes de la palabra fixes se pueden usar otras dos, las cuales son closes o resolves

Los milestones son agrupaciones de issues

# Wikis

Las wikis son espacios proporcionados por GitHub en los cuales podemos desplegar alguna wiki que brinde información (estas son públicas, y por defecto todos podrán editarlas)

Los proyectos de GitHub se pueden utilizar como listas de cosas por hacer, a estos puedes agregar historial del repositorio etc

Las GitHub Pages son un espacio proporcionado por GitHub en el cual puedes desplegar una página o aplicación, es importante que el nombre del repositorio que contendrá la página sea el mismo que el del usuario seguido de .github.io

De ahí en las configuraciones del repositorio en la sección de GitHub pages puedes personalizar tus temas y diseño de la página

Estos contenidos de la página deben cargarse en la master Branch, en una carpeta llamada docs o en una rama especial que debe llamarse `gh-pages`

# Organizaciones

Las organizaciones en GitHub pueden ser grupos de trabajo, a estos se les pueden transferir los repositorios, sin embargo los repositorios personales transferidos a la organización se perderán de la cuenta personal, para “recuperarlos” se debe realizar un fork desde el repositorio de la organización

# Gists

Los gist son como micro-repositorios que contienen uno o dos archivos, en ellos se pueden guardar configuraciones snippets, etc.

# Utilidades

Para eliminar la referencia a un repositorio de GitHub o GitLab puedes ejecutar el comando:

```bash
git remote remove origin
```

Git también contiene hooks, los cuales ayudan a ejecutar ciertas acciones. Husky se ayuda de estas herramientas

```powershell
npm i -D husky@1.3.1
```

```powershell
git tag --delete nombreDeltag
```

```powershell
git push --delete origin nombreDeltag
```

```powershell
git push origin :nombreDelTag

# Y con esto lo borras del servidor (sustituye origin por el nombre de tu remoto)
```