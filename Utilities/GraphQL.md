---
tags: utilities frontend backend
---
----

[Introduction to GraphQL | GraphQL](https://graphql.org/learn/)

----

# ¿Qué es GraphQL?

GraphQL es un lenguaje de consulta para su API y un tiempo de ejecución del lado del servidor para ejecutar consultas utilizando un sistema de tipos que defina para sus datos. GraphQL no está vinculado a ninguna base de datos o motor de almacenamiento específico y, en cambio, está respaldado por su código y datos existentes.

# Básicos

## Schema

El schema es el centro de cualquier implementación del servidor de GraphQL, sin ello no podemos construir la API de GraphQL, es la parte mas compleja del proyecto y con este nos alejamos de la funcionalidad de las apis rest tradicionales

El schema de GraphQL esta definido y diseñado por:

- Los tipos y directivas que admite
- Los tipos de operación que admite:
	- Query: Encargado de realizar las consultas
	- Mutation: Encargado de realizar la modificación de los datos
	- Subscription: Escucha los cambios en el servidor en tiempo real

```gql
type Query{
  miPrimerQuery: <tipo_de_dato>
}

Donde el tipo de dato puede ser un escalar o un objeto
```

El schema describe los tipos de objetos, datos y queries, siendo el query el único obligatorio en el schema

Los resolvers son funciones encargadas de retornar el valor existente en el schema

## Convenciones generales

- Para la sintaxis en los campos se utiliza el `camelCase`
- Para los tipos se usa el `PascalCase`
- Para Enums se utiliza en el nombre del tipo `PascalCase` y para los valores se usa `ALL_CAPS`
- También se recomienda diseñar en base a las necesidades del cliente, construir el esquema como la API GraphQL sera utilizada por el frontend y no incluir campos o relaciones que el cliente no necesita

## Comentarios

Se pueden documentar las API's añadiendo comentarios en el schema, ademas de esto graphQL admite markdown dentro del schema para los comentarios

```gql
"Este es un comentario de una linea"

"""
Este es un
comentario
# multilinea
"""
```

# Tipos

## Scalar Type

Los tipos escalares son datos primitivos que se pueden almacenar en un solo valor, es un tipo de valor imprescindible y define mayoría de las propiedades de las entidades

Existen 5 tipos de datos distintos para GraphQL los cuales son:

- `Int` → Números unicamente enteros
- `Float` → Números con decimales
- `String` → Son cadenas de texto
- `Boolean` → Verdadero / Falso
- `ID` → Es un identificador único puede ser de tipo Int o de tipo String

### Sintaxis

```gql
nombreDeLaPropiedad: tipoDeData

nombre: String
edad: Int
activo: Boolean
```

## Escalares personalizados

Se define en el `schema.gql`

```gql
scalar MyScalarPersonalizado
```

![[scalar-personalizado.png]]

## Object type

Los tipos de objetos son las entidades con las que modelamos y estructuramos los servicios, estos definen como se verá la API.
Todos los tipos deben contener campos y estos campos pueden ser cuantos el creador desee
Todos los tipos deben tener datos

### Sintaxis

```gql
type TipoDePascalCase {
  propiedadCamelCaseUno: DataTypePascal
  propiedadCamelCaseDos: DataTypePascal
  ...
  propiedadCamelCaseN: DataTypePascal
}

Donde DataTypePascal es un tipo de dato que se puede ser un Scalar o un Object Type
```

Ejemplo:
```gql
// schema.graphql

type Profesor {
  nombre: String
  apellidos: String
  experiencia: Int
}

type Asignatura {
  id: ID
  nombre: String
  horasLectivas: Int
  profesor: Profesor
}
```

## Enum type

Los tipos de Enum son similares a un tipo escalar y son útiles para trabajar con una lista de valores predeterminados. Para crear un enum dentro de un schema debemos usar enum en vez de type

### Sintaxis

Sin valor por defecto
```gql
type Profesor {
  nombres: String
  apellidos: String
  experiencia: Int
  curso: Cursos
}

enum Cursos {
  MATEMATICAS
  LENGUAJE
  QUIMICA
}
```

Con valor por defecto
```gql
type Profesor {
  nombres: String
  apellidos: String
  experiencia: Int
  curso: Cursos = LENGUAJE
}

enum Cursos {
  MATEMATICAS
  LENGUAJE
  QUIMICA
}
```

# Modificadores

Los tipos de modificadores sirven para modificar el comportamiento cuando usamos un tipo. Existen dos tipos:

## ! - Indica un valor obligatorio

Permite que el valor sea obligatorio, por ello el valor no puede ser nulo (null).
Si el campo no contiene ! quiere decir que este campo no es obligatorio, por ende no mandarlo no marcara un error

```gql
Ejemplo:
type Profesor {
  nombre: String!
}

const resultado = "Hola"          resultado => NO ERROR en la validación
const resultado2 = null           resultado2 => ERROR en la validación
```

## [ ] - Lista de valores con un elemento o mas

Este se usa cuando se cuenta con uno o mas valores, funciona como las listas o arrays en cualquier lenguaje de programación

```gql
Ejemplo:
type Profesor {
  nombre: String!
  cursos: [String]
}

const resultado = [null, '1', 'Hola', null]           resultado => NO ERROR en la validación (No se indico un valor obligatorio, por ende se pueden recibir datos nulos)

const resultado2 = ['Hola', 'Luis', '23']             resultado2 => NO ERROR en la validación
```

# Combinaciones de modificadores

## [ ]! - Lista con valor requerido/obligatorio

La lista no puede ser nula pero los valores pueden serlo, el valor de la lista no puede ser nulo

```gql
Ejemplo:
type Profesor {
  nombre: String!
  cursos: [String]!
}

const resultado = [null, '1', 'Hola']              resultado => NO ERROR en la validacion (los datos de la lista pueden ser nulos pero la lista no)
const resultado2 = null                            resultado2 => ERROR en la validacion
```

## [ !] - Valores de la lista requeridos/obligatorios

La lista puede ser nula pero los valores no pueden serlo

```gql
Ejemplo:
type Profesor {
  nombre: String
  cursos: [String!]
}

const resultado = [null, 'Hola', '1']            resultado => ERROR en la validacion
const resultado2 = null                          resultado2 => NO ERROR en la validacion
```

## [ !]! - Lista y valores requeridos/obligatorios

Ni la lista ni su contenido puede ser nulo

```gql
Ejemplo:
type Profesor {
  nombre: String!
  cursos: [String!]!
}

const resultado = [null, 'Hola', '1']            resultado => ERROR en la validacion
const resultado2 = null                          resultado2 => ERROR en la validacion
const resultado3 = ['Hola', '1']                 resultado3 => NO ERROR en la validacion
```

# Interfaces

Son definiciones abstractas de atributos comunes, son utiles para retornar objetos de distintos tipos

Son necesarias cuando se busca acceder a cierto grupo de objetos que deben cumplir con las propiedades definidas por la interfaz

```gql
interface Perfil {
  nombre: String!
  email: String!
  edad: Int!
}

type Alumno implements Perfil {
  nombre: String!
  email: String!
  edad: Int!
  curso: String
}
```

# Root Types

Son los puntos de entrada, estos comunican al cliente con el servidor a través de ellos.
Existen tres tipos de raíz

## Query

Es el punto de entrada para realizar consultas, es el equivalente al GET en REST o a un SELECT en SQL

Los tipos de datos que devuelve son Escalares y Objetos

```gql
type Query {
  listaDeElementos: [String]    // Definición sin parámetro - Devuelve una lista de escalares
  elemento(id: ID!): String     // Definición con parámetro - Devuelve un escalar
}

Ejemplo:

query {
  listaDeElementos
}

Esto devuelve
{
  "data" {
    "listaDeElementos":[
      "Elemento1",
      "Elemento2"
    ]
  }
}
```

## Mutation

Es el punto de entrada para realizar las operaciones de modificación, es el equivalente a POST, PUT, PATCH, DELETE en REST o a un DELETE, UPDATE, INSERT en SQL

Los tipos de datos que devuelve son Escalares y Objetos

```gql
type Mutation {
  insertarElemento(nombre:String): String    // Definición con parámetro - Devuelve un escalar
  borrarElemento(id: ID!): [String]          // Definición con parámetro - Devuelve una lista de escalares
  borrarTodo: [String]                       // Definición sin parámetro - Devuelve una lista de escalares
}

Ejemplo:

mutation {
  borrarElemento(id: "1")
}

Esto devuelve
{
  "data" {
    "borrarElemento":[
      "Elemento1",
      "Elemento2"
    ]
  }
}
```

## Subscription

Es el punto de entrada para obtener información en tiempo real

Utiliza la conexión mediante Web Sockets para obtener los cambios

```gql
type Subscription {
  nuevoElemento: String
}

Ejemplo:

query {
  elementosCambio
}

Esto devuelve
{
  "data" {
    "elementosCambio":[
      "Elemento1",
      "Elemento2"
    ]
  }
}
```

# Input Types

Son elementos que nos permiten pasar valores a Queries / Mutations, se pueden pasar valores simples (tipo de escalares), o también objetos mas complejos

Estos tipos de entrada se comportan como los argumentos ante cualquier operación

```gql
input TagInput {
  label: String!
  description: String
}
type Tag {
  id: ID!
  label: String!
  description: String
}
mutation {
  nuevoTag(tag: TagInput!): Tag
}

Ejemplo:

mutation {
  nuevoTag(tag:{
    label: "graphQL",
    description:""
  })
}

{
  "data":{
    "nuevoTag":{
      "id":"1",
      label: "graphQL",
      description:""
    }
  }
}
```

# Alias

De manera automática al realizar multiples peticiones simultaneas GraphQL realiza un merge, esto consiste en unificar todos los campos de distintas solicitudes en una sola.
En caso de que se realicen consultas con diferentes parámetros estas consultas deberán contener un alias que las diferencie entre si, por ejemplo

```gql
{
  walter: character(id:1){
    name
    id
    description
    photo
  }
  jesse: character(id:2){
    name
    id
    description
    photo
  }
}
```