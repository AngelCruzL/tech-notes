---
tags: utilities
---
----


El nombre bash viene de Bourne Again Shell, esto es un tipo de shell (línea de comandos o intérprete de comandos) que encontramos en sistemas operativos base Linux.

Para poder saber cual es el interprete de comandos que se esta utilizando se pueden utilizar los siguientes scripts:

```bash
echo $SHELL

echo $0
```

El `PATH` es el directorio donde se va a buscar o desde donde se va a ejecutar la linea de comandos

---

Es posible ejecutar varias sentencias en la misma línea de comando, solo es necesario separarlas por `;`, por ejemplo:

```bash
cd /home; ls -l; cat /etc/hosts
```

También existe la opción de introducir un AND lógico, de esta manera solo se ejecutara el segundo comando si el primero se ejecuta satisfactoriamente, o un OR, donde si el primero se ejecuta el segundo ya no:

```bash
cd /home/user && pwd

touch new_file.txt || ls -l 
```

Es posible ejecutar cosas en segundo plano, para ello es necesario incluir en el comando un `&`:

```bash
tar czvf copiavar.tar.gz /var &
```

Con `fg` es posible volver a colocar la ejecución del comando en primer plano. Otros comandos relacionados a este son: `bg`, `ps`, `jobs`.

Es importante mencionar que al cerrar la ventana de comandos se pierde la ejecución del comando actual.

Un problema de ejecutar comandos en segundo plano es que los mensajes siguen apareciendo en primer plano, para evitar esto es recomendable redirigirlo los comandos:

Para esto es importante hablar antes de las salidas de los comandos, por defecto existen las siguientes:

**stdout (1):** Esta es la normal

**stderr (2):** Esta es la de errores

_**********stdin (0):**********_ Esta es la entrada

Se puede cambiar el flujo de hacia donde se envía la salida y a esto es a lo que se le llama redirección.

Para realizar esta redirección se puede hacer de la siguiente manera:

```bash
ls -l > test_file.txt
```

Al tener solo un `>` esto crea que el contenido del archivo `test_file.txt` si es que ya existía se sobrescrita, y en caso de no existir se crea con el resultado del comando, para realizar un append al mismo archivo se debe utilizar el siguiente comando:

```bash
ls -l >> test_file.txt
```

Para el manejo y redirección de errores lo que se hace es lo siguiente:

```bash
ls -l 2> /dev/null
```

Esto establece que los errores se irán al dispositivo nulo

En caso de querer mandar tanto la información del comando (1) y la salida de errores (2) a un mismo archivo se puede utilizar el siguiente comando:

```bash
ls -l &> test_file.txt
```

Se pueden combinar estos comandos para realizar comandos con funcionalidades más complejas, por ejemplo:

```bash
tar czvf copiavar.tar.gz /var & > logtar 2> logtarerr
```

En este caso la salida de la ejecución del comando en segundo plano se envía al archivo `logtar`, mientras que la salida de errores del mismo comando se envía a `logtarerr`.

En el siguiente comando se utiliza `nohub`, esto con la finalidad de que tras ejecutar el siguiente comando se pueda ejecutar `fg`, y después `ctrl + c` y no se interrumpa la ejecución del comando.

```bash
nohub tar czvf copiavar.tar.gz /var & > archivo.txt
```

El comando **`nohup`** es una herramienta de la línea de comandos en sistemas operativos tipo Unix (como Linux) que se utiliza para ejecutar comandos o programas que necesitan seguir ejecutándose incluso después de cerrar la sesión del terminal o de desconectar de la máquina.

El nombre "nohup" significa "no hang up" (no cuelgue) y se utiliza para prevenir que un programa se detenga si el terminal que lo inició se cierra o si hay alguna interrupción en la conexión de red.

Para usar el comando **`nohup`**, simplemente se coloca antes del comando que se desea ejecutar, de la siguiente manera: nohup comando &

```bash
nohup comando &
```

El símbolo **`&`** al final indica que el comando debe ejecutarse en segundo plano, es decir, en segundo plano, lo que permite que el terminal quede disponible para la introducción de nuevos comandos.

Además, **`nohup`** re-dirige la salida estándar del comando a un archivo llamado **`nohup.out`** en el directorio actual, de manera que si el programa imprime algún mensaje por pantalla, este será almacenado en el archivo en lugar de ser mostrado en el terminal.

En resumen, **`nohup`** es útil cuando se quiere ejecutar un comando o programa que puede tardar mucho tiempo en terminar o que necesita seguir ejecutándose incluso después de que se haya cerrado la sesión del terminal o desconectado de la máquina.

Así como existen las re-direcciones de salida existen también las de entrada, en este caso se utiliza el símbolo `<`

```bash
sort < prueba.txt > prueba.txt
```

En este caso el comando `sort` toma como entrada el contenido del archivo `prueba.txt` y ordena su contenido de manera alfabética, después de ejecutar esta sentencia la salida de este comando va al mismo archivo `prueba.txt` sobre-escribiendo el contenido original por el contenido ordenado alfabéticamente.

Existe una redireccion particular la cual es la siguiente:

```bash
cat << FIN > test.txt
```

![Screenshot 2023-03-04 at 12.10.56.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/943a09e3-d5ad-4d1b-92f0-53ce19efe8c3/Screenshot_2023-03-04_at_12.10.56.png)

## Pipes

Dentro de bash se puede hacer uso de tuberías, estas ayudan a concatenar comandos, de modo que la salida del primer comando funciona como entrada para el segundo, un ejemplo es el siguiente:

![Screenshot 2023-03-04 at 12.19.13.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d2d57698-7b07-450a-aa56-63d6f5d6ae44/Screenshot_2023-03-04_at_12.19.13.png)

## Creación de scripts

Al crear un script siempre en la primera línea se tendrá una sentencia llamada shebang, esto le indica al archivo `.sh` (convención con la cual se crean los scripts) que se quiere ejecutar el script según este indicado, en caso de bash se utiliza el siguiente:

```bash
#!/bin/bash
```

Para leer una variable al momento de ejecución se utiliza el comando `read`, este puede recibir distintas banderas, las más comunes son `-s` y `-p`, la primera es para ocultar lo que escribe el usuario y la segunda para introducir un mensaje, por ejemplo:

```bash
#!/bin/bash

read -sp "Introduzca la contraseña: " pass
echo La contraseña es $pass
```

## cut

Este comando sirve para escoger los caracteres necesarios de un string, ya que corta las columnas o campos seleccionados de uno o más ficheros. Podemos mandarle las banderas `-d` la cual indica que hara un delimitador de columnas y la `-f`, que indica la cantidad de columnas que queremos escoger. (similar a un splice en js)

## df

Este comando muestra todas las particiones montadas en el disco con `df -h`

## tr

Este comando es similar al trim y lo que hace es que corta los espacios delante de un texto

## bash

con este comando se ejecuta el script ignorando el shebang, puede recibir de argumento `-x` para ejecutarlo línea por línea o/y `-v` para hacerlo más explicito y mostrar detalles.

