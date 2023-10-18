---
tags: utilities
---
----

[GitHub Actions documentation - GitHub Docs](https://docs.github.com/en/actions)

----

# ¿Qué son las GitHub Actions?

GitHub Actions es una plataforma de automatización integrada en GitHub que te permite automatizar tareas en tu flujo de trabajo de desarrollo de software, como la compilación de código, las pruebas y la implementación de aplicaciones. Con GitHub Actions, puedes crear flujos de trabajo personalizados y automatizados que se activan en respuesta a eventos específicos en tu repositorio de GitHub, como la creación de una nueva solicitud de extracción o la publicación de una nueva versión de tu código.

# ¿Cómo funciona GitHub Actions?

GitHub Actions funciona mediante la creación de flujos de trabajo. Estos flujos de trabajo se definen en un archivo YAML dentro de tu repositorio y especifican las acciones que deben realizarse cuando se activa un evento determinado.

Una acción es una tarea específica que se realiza dentro del flujo de trabajo, como la ejecución de un script o la compilación de código.

Cuando se produce un evento específico en tu repositorio de GitHub, como la creación de una nueva _pull request_, GitHub Actions busca automáticamente el archivo YAML del flujo de trabajo correspondiente y ejecuta las acciones especificadas en el archivo.

Es importante tomar en cuenta 3 conceptos:

- Workflows
- Jobs
- Steps

## Workflows

Un workflow o flujo de trabajo en GitHub Actions es una serie de tareas automatizadas que se ejecutan en respuesta a un evento específico en tu repositorio de GitHub. Puedes definir un flujo de trabajo en un archivo YAML en la carpeta **`.github/workflows`** de tu repositorio.

Los workflows se encuentran constituidos por distintos jobs y se activan tras ciertos eventos. Entre los eventos más comunes para activar los workflows son:

- push
- fork
- watch (al darle una estrella al repositorio)
- pull_request (puede ser activado con cualquier acción como opened o closed)
- issues (al abrirlo, eliminarlo, etc)
- dicussion
- create (al crear una rama o etiqueta)
- issue_comment (al comentar un issue o pull request)
- workflow_dispatch (definidos dentro del workflow)
- repository_dispatch (API REST request triggers workflow)
- schedule (al programar un workflow)
- workflow_call (ser llamado por otro workflow)

## Jobs

Un job es una unidad de trabajo que contiene uno o más pasos. Cada job se ejecuta en una máquina virtual y puedes definir tantos jobs como necesites en un workflow. Estos jobs contienen uno o más steps y pueden ser configurados para ejecutarse condicionalmente y/o de manera secuencial o paralela

## Steps

Un step es una tarea individual que se ejecuta dentro de un job. Cada step realiza una tarea específica, como clonar el repositorio, compilar el código o ejecutar pruebas.

![[steps.png]]

# Ejemplo de una acción

Supongamos que tienes un proyecto en GitHub que utiliza Node.js y deseas asegurarte de que se ejecuten todas las pruebas unitarias en cada _pull request_. Para lograr esto, podrías crear un workflow de GitHub Actions que realice las siguientes acciones:

1. Comprobar que se ha producido una _pull request_ en el repositorio.
2. Configurar el entorno de Node.js y todas las dependencias necesarias para ejecutar las pruebas.
3. Ejecutar todas las pruebas unitarias utilizando una herramienta de prueba como Jest o Mocha.
4. Notificar al desarrollador si se ha producido algún error en las pruebas.

Para crear este workflow, deberás crear un archivo YAML dentro de tu repositorio de GitHub con la siguiente estructura:

```yaml
name: Test

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 14.x
        uses: actions/setup-node@v1
        with:
          node-version: 14.x
      - name: Install dependencies
        run: npm ci
      - name: Run tests
        run: npm run test
      - name: Notify on failure
        if: failure()
        run: echo "Tests failed"
```

Este archivo YAML define un flujo de trabajo de GitHub Actions llamado "Test" que se activará cuando se produzca una _pull request_ en la rama "main" del repositorio.

El workflow contiene un job llamado "test" que se ejecuta en una máquina virtual de Ubuntu. Este job tiene varios steps que se ejecutan en secuencia:

1. El primer paso usa la acción **`actions/checkout@v2`** para clonar el repositorio dentro de la máquina virtual.
2. El segundo paso utiliza la acción **`actions/setup-node@v1`** para configurar el entorno de Node.js en la máquina virtual y establecer la versión 14.x de Node.js.
3. El tercer paso instala todas las dependencias necesarias para ejecutar las pruebas utilizando el comando **`npm ci`**.
4. El cuarto paso ejecuta todas las pruebas utilizando el comando **`npm run test`**.
5. Si alguna prueba falla, el último paso notificará al desarrollador imprimiendo "Tests failed" en la consola.

De la misma manera se pueden incluir multiples jobs dentro de un workflow

```yaml
name: My Workflow
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Build
        run: npm run build

  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
```

En este ejemplo, el flujo de trabajo tiene dos jobs: "build" y "test". El primer job ("build") se ejecuta en una máquina virtual de Ubuntu y consta de tres steps que clonan el código del repositorio, instalan las dependencias y luego compilan el código.

El segundo job ("test") también se ejecuta en una máquina virtual de Ubuntu y consta de tres steps que clonan el código del repositorio, instalan las dependencias y luego ejecutan las pruebas.

Cada job se ejecuta de forma independiente y puede contener cualquier número de steps. Puedes personalizar los jobs y steps para adaptarse a las necesidades específicas de tu proyecto de software.

# Artifacts

Un artifact se refiere a cualquier archivo o conjunto de archivos que son producidos por una acción y que pueden ser almacenados y utilizados por otras acciones en el mismo flujo de trabajo o en flujos de trabajo futuros.

Los artifacts son útiles cuando quieres compartir información o datos entre diferentes acciones o flujos de trabajo en GitHub Actions. Por ejemplo, si tienes una acción que compila tu código y quieres que la siguiente acción en tu flujo de trabajo tome ese código compilado para realizar pruebas o despliegue, puedes utilizar un artifact para compartir el código compilado entre las dos acciones.

Ejemplo:

```yaml
name: Deploy website
on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Lint code
        run: npm run lint
      - name: Test code
        run: npm run test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Build website
        run: npm run build
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: website
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: echo "Deploying..."
```

En este caso el workflow se encarga de 3 jobs, `test`, `build` y `deploy`. Estos se ejecutan de manera secuencial al depender del anterior, en el job de `build` se puede ver que se crea un _********artifact********_ con el nombre de _******website******_, este será accesible como recurso al finalizar la acción de manera exitosa.

![[artifacts-github.png]]

Aunque también se puede disponer de este recurso dentro de la misma acción:

```yaml
name: Deploy website
on:
  push:
    branches:
      - main

jobs:
  test:
    ...

  build:
    ...

  deploy:
    ...
    steps:
      - name: Get build artifacts
        uses: actions/download-artifact@v3
        with:
          name: website
      - name: Output contents
        run: ls -la
      - name: Deploy
        run: echo "Deploying..."
```

## Outputs

Los outputs de una acción en GitHub Actions son valores que se producen durante la ejecución de la acción y que se pueden utilizar en otras acciones del mismo flujo de trabajo. Se pueden utilizar para compartir información entre diferentes acciones y para simplificar la comunicación entre ellas.

Los outputs se definen en una acción utilizando la sintaxis **`outputs`** en el archivo **`action.yml`** o en el encabezado de la acción en el archivo del flujo de trabajo. Por ejemplo:

```yaml
outputs:
  output_variable_name:
    description: A description of the output variable.
    value: ${{ steps.action_step.outputs.output_value }}
```

En este ejemplo, se define un output llamado **`output_variable_name`** con una descripción y un valor que se extrae de otro paso en la acción.

Una vez que se define un output en una acción, se puede utilizar en otras acciones del mismo flujo de trabajo. Para acceder a un output de otra acción, se utiliza la sintaxis **`steps.<step-id>.outputs.<output-variable-name>`** en el archivo del flujo de trabajo. Por ejemplo:

```yaml
steps:
  - name: My action step
    id: my-action-step
    uses: my-action
    with:
      some-input: some-value

  - name: My other action step
    uses: my-other-action
    with:
      my-output: ${{ steps.my-action-step.outputs.output_variable_name }
```

En este ejemplo, se utiliza el output **`output_variable_name`** de la acción **`my-action-step`** en la acción **`my-other-action`**.

En resumen, los outputs de una acción en GitHub Actions permiten compartir información entre diferentes acciones en un flujo de trabajo y simplificar la comunicación entre ellas.

## Cache

Se pueden cachear las dependencias de un job para utilizarlo en otro, para esto tenemos que utilizar la siguiente acción `actions/cache@v3`:

```tsx
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
```

Al agregar esto se puede utilizar el cache de estas dependencias de `npm` a través de los distintos jobs, pero también a través de las distintas actions, pues es posible que al volver a disparar la acción en caso de no haber modificado las dependencias ya sea por actualizarlas o agregar o retirar alguna se utilicen las que se habían cacheado en acciones pasadas.

# Secrets y variables de entorno

Es posible agregar dentro de las acciones variables de entorno y secretos para poder evitar colocar de manera directa (hardcodeado) los datos sensibles de la aplicación como las credenciales de la conexión a la base de datos.

Para implementarlo dentro de las acciones podemos usar la siguente sintaxis:

```tsx
jobs:
  test:
    env:
      MONGODB_CLUSTER_ADDRESS: ${{ secrets.MONGODB_CLUSTER_ADDRESS }}
      MONGODB_USERNAME: ${{ secrets.MONGODB_USERNAME }}
      MONGODB_PASSWORD: ${{ secrets.MONGODB_PASSWORD }}
      PORT: 3000
    runs-on: ubuntu-latest
```

Para poder inculir estos secretos se debe acceder al repositorio de GitHub y colocarlo en las configuraciones del mismo:

![[secrets-1.png]]

![[secrets-2.png]]

![[secrets-3.png]]

Ademas de esto podemos hacer uso de repository environments para modificar de manera dinámica los valores de los secretos dependiendo del ambiente:

![[environment-1.png]]

![[environment-2.png]]

Es en este punto que podemos agregar secretos específicos:
![[environment-3.png]]

![[environment-4.png]]

Después de agregar los secretos necesarios observamos algo como lo siguiente:

![[secrets-4.png]]

Ahora dentro del workflow de la acción podemos ver algo similar a esto

```yaml
jobs:
  test:
    environment: testing
    env:
      MONGODB_CLUSTER_ADDRESS: ${{ secrets.MONGODB_CLUSTER_ADDRESS }}
      MONGODB_USERNAME: ${{ secrets.MONGODB_USERNAME }}
      MONGODB_PASSWORD: ${{ secrets.MONGODB_PASSWORD }}
      PORT: 3000
```

Además de esto podemos agregar configuraciones adicionales al environment como el hecho de que la acción que se ejecute con este ambiente solo se active en ciertas ramas, como la rama ****main**** por ejemplo:

![[especificidad.png]]

Ahora con esto configurado podemos eliminar los secretos del repositorio pues ahora que hemos configurado el ambiente de testing no debería tener problemas al obtener estos datos.

![[Untitled-2.png]]

![[Untitled-3.png]]

Al realizar el push a `main` se puede observar que la acción se ejecuto de manera correcta como era el comportamiento esperado:

![[Untitled-4.png]]

Sin embargo gracias a la configuración que colocamos en el job de `test` este solo se ejecutaba en la rama `main`, por lo que al hacer push en la rama `dev` este job no se ejecuta:

![[Untitled-5.png]]

Al `deploy` depender de `dev` la acción cancela la ejecución.

La acción `.github/workflowa/deploy.yml` quedo de la siguiente manera:

```yaml
name: Deployment
on:
  push:
    branches:
      - main
      - dev
env:
  MONGODB_DB_NAME: ${{ secrets.MONGODB_DB_NAME }}
jobs:
  test:
    environment: testing
    env:
      MONGODB_CLUSTER_ADDRESS: ${{ secrets.MONGODB_CLUSTER_ADDRESS }}
      MONGODB_USERNAME: ${{ secrets.MONGODB_USERNAME }}
      MONGODB_PASSWORD: ${{ secrets.MONGODB_PASSWORD }}
      PORT: 3000
    runs-on: ubuntu-latest
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Cache dependencies
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: npm-deps-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Run server
        run: npm start & npx wait-on <http://127.0.0.1>:$PORT
      - name: Run tests
        run: npm test
      - name: Output information
        run: |
          echo "The next line uses a \\".env\\" notation"
          echo "MONGODB_USERNAME: ${{ env.MONGODB_USERNAME }}"
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Output information
        run: |
          echo "The next line uses a \\".env\\" notation"
          echo "MONGODB_USERNAME: ${{ env.MONGODB_USERNAME }}"
          echo "MONGODB_DB_NAME: $MONGODB_DB_NAME"
```

# Control del flujo de ejecución de una acción

Ya se comentó anteriormente que se pueden ejecutar jobs en paralelo y en secuencia, teniendo el siguiente workflow al romper un test podemos encontrarnos con lo siguiente:

```yaml
name: Website Deployment
on:
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Lint code
        run: npm run lint
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Test code
        run: npm run test
      - name: Upload test report
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Build website
        id: build-website
        run: npm run build
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist-files
          path: dist
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Get build artifacts
        uses: actions/download-artifact@v3
        with:
          name: dist-files
      - name: Output contents
        run: ls
      - name: Deploy
        run: echo "Deploying..."
```

![[especificidad.png]]

Podemos observar que al `build` y `deploy` depender de `test` estos no se ejecutan en caso de que el último mencionado falle.

Podemos agregar ejecuciones condicionales haciendo uso de un `if`:

```yaml
test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Test code
        id: test-code
        run: npm run test
      - name: Upload test report
        if: failure() && steps.test-code.outcome == 'failure'
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
```

En este caso hacemos uso de la función `failure()` que nos ofrece GitHub Actions. Esto nos dará como resultado lo siguiente:

![[Untitled-2.png]]

## Conditional Functions

Existen funciones que cambian el comportamiento por defecto del flujo de la acción, estas son las siguientes 4:

- failure(): Retorna true si algun step o job anterior fallo
- success(): retorna true si ninguno de los steps anteriores fallo
- always(): Ejecuta el step siempre, incluso si ha sido cancelado
- cancelled(): Retorna true si el workflow ha sido cancelado

Utilizando estas cunciones condicionales es posible crear jobs que dependan de otros, y en caso de que algún otro genere errores ejecutar ese job en especifico

```yaml
name: Website Deployment
on:
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
      - name: Lint code
        run: npm run lint
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
      - name: Test code
        id: test-code
        run: npm run test
      - name: Upload test report
        if: failure() && steps.test-code.outcome == 'failure'
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
      - name: Build website
        id: build-website
        run: npm run build
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist-files
          path: dist
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Get build artifacts
        uses: actions/download-artifact@v3
        with:
          name: dist-files
      - name: Output contents
        run: ls
      - name: Deploy
        run: echo "Deploying..."
  report:
    needs: [lint, deploy]
    if: failure()
    runs-on: ubuntu-latest
    steps:
      - name: Output information
        run: |
          echo "Something went wrong!"
          echo "${{ toJSON(github) }}"
```

En caso de un error se muestra lo siguiente:

![[Untitled-3.png]]

En caso de que los jobs sean satisfactorios lo observamos de la siguiente manera:

![[Untitled-4.png]]

Otra manera de realizar la acción es utilizando la expresión: `continue-on-error`:

![[Untitled-5.png]]

## Matrix

La idea de una matriz es ejecutar el mismo job con diferentes configuraciones

```yaml
name: Matrix Demo
on: [push]
jobs:
  build:
    continue-on-error: true
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]
        operating-system: [ubuntu-latest, windows-latest]
        # Alternative:
        # os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.operating-system }}
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Install NodeJS
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - name: Install dependencies
        run: npm ci
      - name: Build project
        run: npm run build
```

Podemos observar que con este workflow tendremos 6 procesos ejecutandose en paralelo, esto debido a las 3 versiones de node que se ejecutan en 2 sistemas operativos. Podemos incluir una versión adicional de node (ej. 18, con ubuntu-latest), sin la necesidad de generar dos nuevas combinaciones, esto se logra gracias a la palabra reservada `include`. Del mismo modo podemos excluir combinaciones

```yaml
name: Matrix Demo
on: [push]
jobs:
  build:
    continue-on-error: true
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]
        operating-system: [ubuntu-latest, windows-latest]
        # Alternative:
        # os: [ubuntu-latest, windows-latest]
        include:
          - node-version: 18.x
            operating-system: ubuntu-latest
        exclude:
          - node-version: 12.x
            operating-system: windows-latest
    runs-on: ${{ matrix.operating-system }}
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Install NodeJS
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - name: Install dependencies
        run: npm ci
      - name: Build project
        run: npm run build
```

## Reusable workflows

Podemo utilizar worflows dentro de otros workflows:

`reusable.yml`

```yaml
name: Reusable Deploy
on:
  workflow_call:
    inputs:
      artifact-name:
        description: The name of the deployable artifact files
        required: false
        default: dist
        type: string
    outputs:
      result:
        description: The result of the deployment
        value: ${{ jobs.deploy.outputs.outcome }}
    # secrets:
    #   deploy-key:
    #     description: The SSH private key to use for deployment
    #     required: true
jobs:
  deploy:
    outputs:
      outcome: ${{ steps.set-result.outputs.step-result }}
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
        with:
          name: ${{ inputs.artifact-name }}
      - name: List files
        run: ls -la
      - name: Output information
        run: |
          echo "Deploying and uploading..."
      - name: Set result output
        id: set-result
        run: |
          echo "step-result=success" >> $GITHUB_OUTPUT
```

`use-reuse.yml`

```yaml
name: Using Reusable Workflow
on:
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
      - name: Lint code
        run: npm run lint
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
      - name: Test code
        id: test-code
        run: npm run test
      - name: Upload test report
        if: failure() && steps.test-code.outcome == 'failure'
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: steps.cache.outputs.cache-hit != 'true'
        run: npm ci
      - name: Build website
        id: build-website
        run: npm run build
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist-files
          path: dist
  deploy:
    needs: build
    uses: ./.github/workflows/reusable.yml
    with:
      artifact-name: dist-files
    # secrets:
    #   deploy-key: ${{ secrets.DEPLOY_KEY }}
  print-deploy-result:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Print deploy output information
        run: |
          echo "Deploy result: ${{ needs.deploy.outputs.result }}"
  report:
    needs: [lint, deploy]
    if: failure()
    runs-on: ubuntu-latest
    steps:
      - name: Output information
        run: |
          echo "Something went wrong!"
          echo "${{ toJSON(github) }}"
```

# Acciones dentro de contenedores

Es posible ejecutar acciones dentro de contenedores, esto se logra gracias a agregar a nivel job la sintaxis de container y la respectiva imagen que se utilizará. Esto mayormente se puede utilizar para realizar test sobre una base de datos que exista a nivel contenedor para así evitar alterar los datos de la db que se encuentra en producción.

```yaml
name: Deployment (Container)
on:
  push:
    branches:
      - main
      - dev
env:
  CACHE_KEY: node-deps
  MONGODB_DB_NAME: gha-demo
jobs:
  test:
    environment: testing
    runs-on: ubuntu-latest
    container:
      image: node:16
    env:
      MONGODB_CONNECTION_PROTOCOL: mongodb
      MONGODB_CLUSTER_ADDRESS: mongo-db
      MONGODB_USERNAME: root
      MONGODB_PASSWORD: example
      PORT: 8080
    services:
      mongo-db:
        image: mongo
        env:
          MONGO_INITDB_ROOT_USERNAME: root
          MONGO_INITDB_ROOT_PASSWORD: example
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Cache dependencies
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: ${{ env.CACHE_KEY }}-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Run server
        run: npm start & npx wait-on <http://127.0.0.1>:$PORT # requires MongoDB Atlas to accept requests from anywhere!
      - name: Run tests
        run: npm test
      - name: Output information
        run: |
          echo "MONGODB_USERNAME: $MONGODB_USERNAME"
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Output information
        env:
          PORT: 3000
        run: |
          echo "MONGODB_DB_NAME: $MONGODB_DB_NAME"
          echo "MONGODB_USERNAME: $MONGODB_USERNAME"
          echo "${{ env.PORT }}"
```

Esto nos da el siguiente resultado:

![[Untitled-6.png]]

Para este caso tanto el job como la base de datos se encuentran dentro de un contenedor, en caso de querer ejecutar el job desde fuera del contenedor se utiliza la siguiente acción:

```yaml
name: Deployment (Container)
on:
  push:
    branches:
      - main
      - dev
env:
  CACHE_KEY: node-deps
  MONGODB_DB_NAME: gha-demo
jobs:
  test:
    environment: testing
    runs-on: ubuntu-latest
    # container:
    #   image: node:16
    env:
      MONGODB_CONNECTION_PROTOCOL: mongodb
      MONGODB_CLUSTER_ADDRESS: 127.0.0.1:27017
      MONGODB_USERNAME: root
      MONGODB_PASSWORD: example
      PORT: 8080
    services:
      mongo-db:
        image: mongo
        ports:
          - 27017:27017
        env:
          MONGO_INITDB_ROOT_USERNAME: root
          MONGO_INITDB_ROOT_PASSWORD: example
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Cache dependencies
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: ${{ env.CACHE_KEY }}-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Run server
        run: npm start & npx wait-on <http://127.0.0.1>:$PORT # requires MongoDB Atlas to accept requests from anywhere!
      - name: Run tests
        run: npm test
      - name: Output information
        run: |
          echo "MONGODB_USERNAME: $MONGODB_USERNAME"
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Output information
        env:
          PORT: 3000
        run: |
          echo "MONGODB_DB_NAME: $MONGODB_DB_NAME"
          echo "MONGODB_USERNAME: $MONGODB_USERNAME"
          echo "${{ env.PORT }}"
```

Esto nos da el siguiente resultado:

![[Untitled-7.png]]

# Custom Actions

Estas se recomienda utilizarlas para evitar escribir multiples (y probablemente muy compejos) steps, remplazandolos por una accion personalizada.

También puede que dentro del repositorio de acciones no exista una que satisfaga nuestra necesidad, entonces en ese momento también es viable optar por una custom action.

## Tipos de acciones personalizadas

- Javascript actions: Ejecuta un archivo javascript, usa js o node y paquetes de tu elección, son bastante sencillas si ya conoces JS.
- Docker actions: Crea un Dockerfile con la configuración requerida, ejecuta las tareas de tu elección en cualquier lenguaje y ofrece gran flexibilidad.
- Composite actions: Combina multiples steps de un workflow en una sola acción, combina run (comandos) y uses (acciones). También permite reutilizar steps compartidos.

## Ejemplo composite action:

`.github/actions/cached-deps/action.yml`

```yaml
name: 'Get & Cache Dependencies'
description: 'Get and cache dependencies for a Node.js project'
inputs:
  caching:
    description: 'Whether to cache dependencies or not'
    required: false
    default: 'true'
outputs:
  used-cache:
    description: 'Whether cached dependencies were used'
    value: ${{ steps.install.outputs.cache }}
runs:
  using: 'composite'
  steps:
    - name: Cache dependencies
      if: inputs.caching == 'true'
      id: cache
      uses: actions/cache@v3
      with:
        path: node_modules
        key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
    - name: Install dependencies
      if: steps.cache.outputs.cache-hit != 'true' || inputs.caching != 'true'
      id: install
      run: |
        npm ci
        echo "cache='${{ inputs.caching }}'" >> $GITHUB_OUTPUT
      shell: bash
```

`.github/workflows/deploy.yml`

```yaml
name: Deployment
on:
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Load and cache dependencies
        id: cache-deps
        uses: ./.github/actions/cached-deps
        with:
          caching: 'false'
      - name: Output information
        run: echo "Cache used? ${{ steps.cache-deps.outputs.used-cache }}"
      - name: Lint code
        run: npm run lint
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Load and cache dependencies
        uses: ./.github/actions/cached-deps
      - name: Test code
        id: run-tests
        run: npm run test
      - name: Upload test report
        if: failure() && steps.run-tests.outcome == 'failure'
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Load and cache dependencies
        uses: ./.github/actions/cached-deps
      - name: Build website
        run: npm run build
      - name: Upload artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist-files
          path: dist
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Get build artifacts
        uses: actions/download-artifact@v3
        with:
          name: dist-files
          path: ./dist
      - name: Output contents
        run: ls
      - name: Deploy site
        run: echo "Deploying..."
```

Lo que nos da como resultado lo siguiente:

![[Untitled-8.png]]

# Seguridad y permisos

Script injection: Es cuando un valor asignado desde fuera del workflow es utilizado dentro de este, por lo cual cambia el comportamiento del mismo.

Malicious third party actions: Acciones de terceros maliciosas, por ejemplo aquellas que leen y exponen los secretos de tu repositorio. Se recomienda no utilizar acciones no confiables o inspeccionarlas para garantizar la seguridad del repositorio.

Permission Issues: Asignar solo los permisos necesarios, por ejemplo el modo solo lectura.