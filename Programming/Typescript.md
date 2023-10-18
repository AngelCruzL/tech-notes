---
tags: programming
---
----

[Documentación]()

----

# ¿Qué es TypeScript?

Typescript es un superset de [[Javascript]], el cual le añade tipado a los datos, lo que previene futuros errores en las aplicaciones ya que Javascript por si mismo al ser un lenguaje dinámico es bastante maleable en cuestión de data.

Otra de las principales ventajas de typescript es que al ser fuertemente tipado puede ayudar en el autocompletado del código y provee una básica documentación acerca del mismo, pues las propiedades y datos deben ser declarados de manera explicita.

## Tipos de Datos

Al ser una extensión de JS se manejan principalmente 3 tipos de datos
- `number` para números, independientemente de si se trata de valores enteros o de punto flotante.
- `string` para cualquier cadena de caracteres o textos.
- `boolean` para valores `true` o `false`


Los genéricos sirven para indicar que tipo de dato va a retornar una función y que tipo de dato va a obtener

`[id]='jsExpresion'`

`id={{jsExpresion}}`

Para hacer uso de los formularios en angular debemos importar el FormsModule, después en el envio del formulario pasamos el evento `(ngSubmit)`

Las llaves cuadradas son para establecer un valor de TS al objeto en el html, mientras que los parentesis son para escuchar eventos en el html y referirlos al archivo de ts

`[(ngModel)]="[newChar.name](<http://newchar.name/>)"` nos permite tanto emitir como recibir propiedades _two way data binding_

Testing Angular

describe

Describe las pruebas sobre dicho componente

expect

Es el método que espera que el valor que le pasemos cumpla con una condicion

fdescribe

Solo se emitirán las pruebas de este componente y se omitirán las demás

Tipos de testing

- Unit test: Se trata de tests aislados sin recursos externos
- Integration Test: Pruebas de componentes con recursos externos
- End-to-end: Simulación de usuario real

Dentro de Angular para mandar un evento desde el component.html a su contra parte de ts se debe hacer con la nomenclatura `function($event)`, de manera estricta debe contener el simbolo **$**

# Namespaces

Un namespace es una manera de separar el código agrupandolo en distintos espacios con el fin de evitar la colisión de variables, métodos o funciones.

## Decoradores

Los decoradores son funciones que se utilizan para añadirle funcionalidades a los objetos.

Estas se ejecutan en el momento de transpilación de TypeScript a JavaScript