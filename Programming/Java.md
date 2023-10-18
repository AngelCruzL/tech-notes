---
tags: programming
---
----

[Documentación]()

----

# ¿Qué es Java?

Si dentro de un archivo ninguna clase es publica el archivo puede tomar cualquier nombre, en caso de que el archivo contenga alguna clase publica esta clase debe tener el mismo nombre del archivo.

## Variables

Dentro de java existen las variables y las constantes, las constantes mantienen su valor, mientras que el valor del una variable cambia a lo largo del tiempo.

Al declarar una variable no puede iniciar con un número, no se pueden utilizar palabras reservadas

Para poder declarar constantes se hace utilizando la palabra reservada `final`:

```java
final int age = 33;
```

Dentro de java existen diferentes tipos de datos

- boolean
- byte
- short
- char
- int
- float (definir la `f` al final)
- long (definir la `l` al final)
- double

## Operadores

Existen distintos tipos de operadores, entre ellos se encuentran los aritmeticos, los cuales se encargan de realizar operaciones matematicas como la suma, resta, división, multiplicación y el módulo (el cual regresa el residuo de una división).

Otro tipo de operadores son los de comparación, estos sirven para comparar las variables.

## Métodos

Estos definen un valor de retorno.

## Constructores

Dentro de Java podemos identificar los constructores porque no tienen un valor de retorno y generalmente tienen el mismo nombre que la clase.

Siempre se tiene un constructor por defecto, sin embargo al crear uno se sobreescribe el que esta por defecto, aunque **se puede tener tantos constructores como se requiera**, a esto se le conoce como **sobrecarga de constructores**.

## This

Esta palabra reservada tiene la función de remover ambiguedades en los parametros de los constructores, para asi evitar que se confundan con propiedades, tambien permite invocar a un constructor dentro de otro constructor.

```java
 Person(String name, int age, char gender) {
    this();
    this.name = name;
    this.age = age;
    this.gender = gender;
  }
```

## Herencia

Se puede utilizar para reducir el codigo (is a/is an)

Evita inconsistencias y por ende bugs

## Polimorfismo

Es la capacidad que tienen los objetos para ser identificados como otros.

## Paquetes

Sirven para agrupar clases que pertenecen al mismo contexto.

## @Override

Sirve para sobreescribir el comportamiento de un metodo heredado de una clase padre. Tambien se puede implementar el metodo de la clase padre con ayuda de la palabra reservada `super`:

```java
  @Override
  void sleep() {
    super.sleep();
    System.out.println("Hi, I'm " + name + " and I'm not sleep because I'm student! 😖");
  }
```

Para sobreescribir un metodo se deben seguir ciertas reglas:

- Los argumentos deben ser los mismos al metodo que se va a sobre escribir
- El valor de retorno debe ser el mismo o un subtipo del que se va a sobre escribir
- El nivel de acceso no puede ser mas restrictivo que el que se va a sobre escribir

## Convenciones de nombres

Los paquetes deben escribirse en minúsculas, una buena practica es agregarle prefijos.

Los nombres de las clases deben ser sustantivos y en caso de tener mas de una palabra se debe utilizar la notacion UpperCamelCase

Para las variables y los metodos de las clases deben utilizarse verbos y la notacion es camelCase

En caso de constantes estas se escriben en mayúsculas y se separan por guiones bajos.

## Niveles y modificadores de acceso

Se tienen 4 niveles:

- private -> disponible dentro de la misma clase
- default -> disponible dentro del mismo paquete
- protected -> disponible a traves de herencia en diferentes paquetes
- public -> siempre disponible

Permite controlar el acceso a los metodos de las clases

## Encapusulamiento

Es el control de acceso al objeto

puedes modificar metodos a traves de getters y setters

## Abstraccion

Se refiere a que vamos a exponer

Los detalles de implementacion son metodos que son utilizados dentro de la clase y no son utilizados de forma externa. Este tipo de metodos deben ser privado.

El acoplamiento es que tanto depende tu clase de otras clases, la cohesion son metodos y atributos que van de acuerdo al contexto de la clase.

El polimorfismo hace referencia a que las clases se pueden ver de distintas formas.

---

Una clase no puede heredar de mas de una clase padre


Todos los programas de java se rigen bajo las reglas del classpath, esto hace referencia a los directorios que java utilizara para encontrar las clases que java necesita.

Por defecto el classpath se encuentra en el directorio actual, aunque puede ejecutarse indicando el directorio de manera explicita:

```bash
javac -cp <classpath dir> <Classname>
```

Las clases se pueden encontrar en directorios o en archivos `.jar`, los cuales son un equivalente a los `.zip`.

La programacion orientada a objetos implemento el concepto de `package`, esto permite evitar colisiones entre los nombres, esto se logra gracias a la siguiente sintaxis:

```java
package com.angelc.oop;

public class MyClass {}
```

Pero no es suficiente definir el package, para esto es necesario almacenar el archivo `MyClass` dentro de una estructura de directorios anidados que hagan referencia al nombre del package, en este caso seria `com/angelc/oop/MyClass.java`.

En caso de querer importar un clase de otro package dentro del que estamos trabajando tendremos que hacer uso de la sentencia `import`:

```java
package com.angelc.example;

import com.angelc.oop.MyClass;

public class AnotherClass {
	MyClass obj = new MyClass();
}
```

Dentro de UML cuando la relación es de uno a muchos (Factura → ItemFactura[]) la relación es conocida como agregación.

## Clases Wrapper

Estas sirven para envolver a un primitivo como si fuera un objeto, esto le permite aniadirle mayor funcionalidad

# ORM

Un Object Relational Mapping es una técnica de programación para mapear los modelos de objetos de dominio de la aplicación con las tablas de bases de datos relacionales.

# JPA
Jakarta Persistence es una especificación para facilitar la relación del mapeo entre objetos y relaciones para manejar la información de las aplicaciones java. Proporciona una manera de trabajar directamente con los objetos en lugar de utilizar consultas SQL. Entre los más populares se encuentran Hibernate, EclipseLink, TopLink, MyBatis.

# Hibernate

Hibernate es un ORM que ofrece la funcionalidad de mapear los objetos a tablas de bases de datos y viceversa. Esto gracias a que el framework genera los queries necesarios en SQL para realizar las operaciones por medio de JDBC.

## JPA vs Hibernate

JPA es una especificación, mientras que Hibernate es la implementación de JPA, además de que Hibernate genera los queries SQL y los ejecuta por medio de JDBC.

## JPA Repository Methods

- save
- saveAll
- findById
- existsById
- findAll
- findAllById
- count
- deleteById
- delete
- deleteAllById
- deleteAll

