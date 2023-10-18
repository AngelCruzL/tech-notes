---
tags: programming
---

---

[Our Documentation | Python.org](https://www.python.org/doc/)

---

# ¿Qué es python?

[Python](https://www.python.org/) es un lenguaje de programación interpretado cuya filosofía hace hincapié en la legibilidad de su código. Se trata de un lenguaje de programación multiparadigma, ya que soporta orientación a objetos, programación imperativa y, en menor medida, programación funcional. Es un lenguaje interpretado, dinámico y multiplataforma.

Algo importante de mencionar es que python no necesita `;` ni hacer uso de `{}` en el código, por lo que el indentado de este debe ser correcto para una buena ejecución del mismo.

## Preparación del entorno de desarrollo

Lo primero para poder trabajar con python es [descargarlo](https://www.python.org/), una vez hecho esto puedes corroborar la instalación introduciendo el siguiente comando en la terminal:

```sh
python3 --version
```

```sh
py --version

python --version
```

En caso de que el comando anterior sea reconocido por el sistema y devuelva una versión de python felicidades, has instalado python.

Para poder crear archivos de python necesitas un editor de texto o IDE, en este caso yo por preferencia personal me inclino por [VSCode](https://code.visualstudio.com/download), ya que este es gratuito y puede ser mejorado a traves de sus pluggins, de entre los cuales recomiendo los siguientes:

- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

## Variables y tipos de datos

Dentro de python existen distintos tipos de datos, estos tipos de datos pueden ser asignados a variables. Las variables son espacios en memoria que pueden almacenar datos.

Entre los principales tipos de datos de python se encuentran:

- Strings
- Integer
- Float
- Boolean

Al ser python un lenguaje dinámico no es necesario la declaración explicita del tipo de dato de la variable.

Existe una [guía](https://pep8.org/) de estilos o convenciones para poder estructurar el código de python, entre estas convenciones se menciona que para declarar variables con más de una palabra estas palabras deben estar separadas por un `_` , por ejemplo:

```py
my_list = []
```

Se recomienda llamar a la variable de la manera más descriptiva posible para poder evitar confusiones futuras.

### Datos Numéricos

Para estos existen los datos enteros, flotantes, complejos, etc.

Este tipo de dato permite realizar operaciones matemáticas.

### Strings

Los strings son cadenas de caracteres, por ejemplo:

```py
string = 'Hola desde la variable 🎉'
```

También puedes mezclar números con letras o palabras, sin embargo estos números serán interpretados como un tipo de dato string y no como un tipo de dato numérico. Por ejemplo:

```py
long_string = """
Esto es un comentario multilínea
mezclando texto y números 54564
sa32454234
"""
```

Dentro de python solo se pueden concatenar tipos de datos string, en caso de querer concatenar un tipo de dato entero debe transformarse a un string

`str(var_name)`

## Impresión de datos en consola

Existen distintas maneras de poder imprimir datos en consola, desde REPL se utiliza la siguiente:

```py
name = 'Luis'
f"Hello {name}"
```

Otras de las sintaxis que se puede utilizar son:

```py
print("The function is declared like this:")

name = 'Luis'
print(f"Hello {name}")

print('Hello', name)
```

Para concatenar strings se hace con ayuda del operador `+`, por ejemplo:

```py
print('print("Hello " + "world") ')
```

Puedes hacer uso del input de datos con la función `input()`

De la misma manera se pueden crear cosas mas complejas, como el siguiente código que imprime en pantalla la cantidad de caracteres que tiene el nombre o la palabra que se le haya introducido:

```py
print( len( input("What is your name? ") ) )
```

## Funciones

Las funciones son trozos de código reutilizable, los cuales suelen regresar algún valor tras ser ejecutadas, también pueden recibir parámetros, los cuales son datos con los que trabajará la función, aunque estos parámetros son opcionales dependiendo del trabajo que realice la función.

Para definir una función en python se utiliza la siguiente sintaxis:

```py
# Función básica la cual no retorna nada
def hola_mundo():
    print("Hola Mundo!")


# Función que acepta parámetros y regresa un valor
def suma(x, y):
    return x + y
```

A las funciones se les puede colocar valores por defecto en los parámetros, así en caso de no recibir un parámetro necesario para la ejecución de la función ésta pueda ejecutarse previniendo un error en el programa.

```py
def create_query(language="JavaScript", num_stars=50, sort="desc"):
    return f"language:{language} num_stars:{num_stars} sort:{sort}"
```

Para declarar los parámetros opcionales se utiliza la sintaxis de arriba, es importante aclarar que dado que los argumentos se envían en el orden que son declarados los parámetros opcionales siempre se declaran al final.

Para poder llamar o invocar una función se hace de la siguiente manera:

```py
# Función sin parámetros
hola_mundo()

# Función sin parámetros
suma(4, 7)
```

## Listas

Las listas en python son similares a los arreglos en cualquier otro lenguaje de programación. Estas listas son una colección de elementos ordenados de manera secuencial en la memoria.

| type               | list                                                                                                                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| use                | Used for storing similar items, and in cases where items need to be added or removed.                                                                                                      |
| creation           | [] or list() for empty list, or [1, 2, 3] for a list with items.                                                                                                                           |
| search methods     | my_list.index(item) or item in my_list                                                                                                                                                     |
| search speed       | Searching in an item in a large list is slow. Each item must be checked.                                                                                                                   |
| common methods     | len(my_list), append(item) to add, insert(index, item) to insert in the middle, pop() to remove.                                                                                           |
| order preserved?   | Yes. Items can be accessed by index.                                                                                                                                                       |
| mutable?           | Yes                                                                                                                                                                                        |
| in-place sortable? | Yes. my_list.sort() will sort the list in-place. my_list.sort(reverse=True) will sort the list in-place in descending order. my_list.reverse() will reverse the items in my_list in-place. |

## Tuplas

Las tuplas son similares a las listas, con la diferencia de que las tuplas son inmutables, es decir que ya una vez creada la tupla esta no puede cambiar sus valores, sin embargo permite utilizar los métodos de búsqueda como los que se utilizan en las listas.

Generalmente las tuplas son utilizadas para almacenar data y que esta no pueda ser modificada después.

## Index

`[start:stop:stepover]`

## Diccionarios

Los diccionarios en python son similares a los arreglos de javascript, ya que se trata de un conjunto de elementos que contienen una llave-valor.

Es posible modificar el valor de un diccionario accediendo a el desde su llave.

## Sets

Los sets son un tipo de colección desordenada ya que sus elementos no se van a mostrar en el orden en el que fueron declarados, también es importante destacar que esta estructura de datos no permite valores duplicados ni almacenar otro tipo de colecciones como listas o diccionarios.

```py
conjunto = set();
```

Para poder imprimir en consola se sigue la siguiente sintaxis:

```python
print("The function is declared like this:")
```

Para concatenar strings se hace con ayuda del operador `+`, por ejemplo:

`print('print("Hello " + "world") ')

Puedes hacer uso del input de datos con la siguiente sintaxis: `input()`

De la misma manera se pueden crear cosas mas complejas, como el siguiente código que imprime en pantalla la cantidad de caracteres que tiene el nombre o la palabra que se le haya introducido: `print( len( input("What is your name? ") ) )`

Entre los tipos de datos del lenguaje se encuentran los primitivos, los cuales son:

- Strings
- Integer
- Float
- Boolean

Dentro de python solo se pueden concatenar tipos de datos string, en caso de querer concatenar un tipo de dato entero debe transformarse a un string

`str(var_name)`

---

Las listas en python son similares a los arreglos en javascript, al introducir indices negativos el conteo del arreglo comienza de atrás hacia delante

`.append()` agregará al final de la lista el parámetro que se le mande a la función

[5. Data Structures - Python 3.9.2 documentation](https://docs.python.org/3/tutorial/datastructures.html)

## Indentación

Es importante mencionar que Python al no tener en su sintaxis el uso de las llaves `{}` es necesario que se indente de manera correcta para poder separar los grupos o bloques de código.

## Módulos

Los módulos son partes de código creados por distintos desarrolladores para facilitar la ejecución del alguna tarea en especifico, de esta manera te evitas estar creando el mismo trozo de código, así utilizando o reutilizando algo que ya sabes que funciona.

## Random module

El módulo random de Python funciona para generar valores aleatorios

[Python random Module - Generate Random Numbers/Sequences - AskPython](https://www.askpython.com/python-modules/python-random-module-generate-random-numbers-sequences)

Para poder utilizar este modulo debes importarlo dentro de tu programa:

```python
import random

print(random.randint(1, 10))
```

La función `shuffle()` permite reordenar de manera aleatoria el contenido de una lista, dicha lista será enviada a la función como parámetro.

## Range Function

La función de rango como su nombre lo indica crea un rango, generalmente es utilizada para los bloques for, teniendo así una sintaxis como la siguiente:

```python
for number in range(a, b):
	print(number)
```

Donde a y b son los límites del rango que se esta declarando, sin embargo el límite superior no será tomado en cuenta, por lo que el último valor será `b-1`, también la función range puede recibir como tercer parámetro el `step` que será el valor en cada tanto se recorrerá el rango.

## Functions

Las funciones son trozos de código los cuales pueden ser reutilizables ya que cumplen una función en especifico.

Las funciones pueden llamar o invocar a otras funciones desde dentro de sí mismas.

[Built-in Functions - Python 3.9.2 documentation](https://docs.python.org/3/library/functions.html)

Para poder crear una función en python se utiliza la siguiente sintaxis:

```python
def my_function():
	#Do this
	#Then do this
	#Finally do this
```

Algo importante a la hora de programar es poder crear un diagrama de fujo del programa, esto permitirá seccionar el programa en pequeñas tareas en especifico, con esto se podrán abordar con mayor facilidad ya que son tareas concretas.

[PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)

[Python Lists | Python Education | Google Developers](https://developers.google.com/edu/python/lists#for-and-in)

---

#Rule: params, \*args, default params, \*\*kwargs
