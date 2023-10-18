---
tags: database
---
----

[MongoDB Documentation](https://www.mongodb.com/docs/)

----

# ¿Qué es MongoDB?


![[MongoDB - Docker#Utilizar una imagen de MongoDB]]


# Comandos básicos

Para mostrar las bases de datos puedes utilizar el comando:

```sh
show dbs
```

Para utilizar una base de datos especifica utilizamos el siguiente comando

```sh
use <db name>
```

En este caso este comando puede utilizarse también para crear una nueva base de datos, enviando como parámetro el nombre que queremos para la nueva base de datos, la cual se creará tras realizar la primera inserción del dato.

Para introducir un registro dentro de la base de datos utilizamos la siguiente sintaxis:

```bash
db.<collection>.insertOne({<object data>})
```

Para comprobar el estado de la data de la colección formateada de una manera correcta podemos utilizar el siguiente comando:

```sh
db.<collection>.find().pretty()
```

![[mongo-pretty.png]]

## Types of databases

- Relational
- Document
- Key Value
- Graph

```sh
use natours // Uses the natours database, if not exists creates it

db.<collection>.insertMany({ name: "The Forest Hiker", price: 297, rating: 4.7 })
```

```sh
query middleware:

userSchema.pre('find'

------

A cookie is a small piece of text that the server sends to the client,
when the client receibes the cookie automatically stores it
and automaticaly will send it back with all the futures requests
to the same server
```

MongoDB is faster than SQL databases because all the information is stored on the same place, this allows more speed and reactivity on the modifications.

Scalability is the property that refers to process data, you can grow horizontal or vertical.

Shading is the property that refers to the capacity to split data on different databases and use a router system to locate a specific data distributing the queries and the server process the requests



|                                  | MongoDB                    | PostgreSQL               |
| -------------------------------- | -------------------------- | ------------------------ |
| Tipo                             | Documento                  | Relacional               |
| Información organizada dentro de | Colecciones                | Tablas                   |
| Lenguaje de consulta             | NoSQL                      | SQL                      |
| Escalamiento                     | Primordialmente horizontal | Primordialmente vertical |
| Esquema                          | Flexible                   | Rigido                         |

The schema defines the structure of the NoSQL database


# CRUD Operations

## Create

```js
insertOne(data, options)
insertMany(data, options)
```

## Read

```js
find(filter, options)
findOne(filter, options)
```

## Update

```js
updateOne(filter, data, options)
updateMany(filter, data, options)
replaceOne(filter, data, options)
```

## Delete

```js
deleteOne(filter, options)
deleteMany(filter, options)
```


## Examples

```bash
use test_db

db.flightData.insertOne({
  "departureAirport": "MUC",
  "arrivalAirport": "SFO",
  "aircraft": "Airbus A380",
  "distance": 12000,
  "intercontinental": true
})

db.flightData.insertOne({
  "departureAirport": "LHR",
  "arrivalAirport": "TXL",
  "aircraft": "Airbus A320",
  "distance": 950,
  "intercontinental": false
})

db.flightData.insertOne({
  "departureAirport": "LHR",
  "arrivalAirport": "TXL",
  "aircraft": "Airbus A321",
  "distance": 950,
  "intercontinental": false
})

db.flightData.find().pretty()

db.flightData.deleteOne({
  "distance": 950
})

db.flightData.updateOne({
  "distance": 950
}, {
  $set: {
    "marker": "delete"
  }
})

db.flightData.updateMany(
  {},
  {
    $set: {
      "marker": "toDelete"
    }
  })

db.flightData.deleteMany({
  "marker": "toDelete"
})
```

## Drop DB or Collection

```sh
use <db_name>
db.dropDatabase()

db.<collectionName>.drop()
```

find() Te devuelve por defecto los primeros 20 elementos
`db.flightData.find().toArray()` hace el fetch de los datos y los devuelve convertidos en un array

![[find-to-array.png]]

![[find-foreach.png]]

# Projection

Permite realizar un filtrado de los datos que se requieren al realizar la consulta de los documentos, por ejemplo si en este caso de los vuelos solo queremos ver los aeropuertos y si es intercontinental necesitamos ejecutar lo siguiente:
![[find-with-filter.png]]

Por defecto esto nos muestra siempre el ID, en caso de querer quitarlo necesitamos mandar el argumento con un valor de 0:
![[find-with-filter-2.png]]

# Embedded Documents

Es importante destacar que en Mongo puedes llegar a tener 100 niveles de identación y un máximo de 16mb del tamaño del documento.
También puedes tener arrays de documentos.
![[update-many.png]]

Con el siguiente ejemplo agregamos el documento anidado de detalles que contiene al responsable solo a los vuelos que tengan una distancia mayor a 1000:
```sh
db.flightData.updateMany({
  "distance": {
    $gt: 1000
  }
}, {
  $set: {
    "status": {
      "description": "on-time",
      "lastUpdated": "1 hour ago",
      "details": {
        "responsible": "Ángel Cruz"
      }
    }
  }
})
```

![[update-many-2.png]]

Esto se puede comprobar si también solicitamos un documento que no cumpla con esta condición, en donde no veremos el objeto de detalles:

![[find-one.png]]

Ahora podemos accesar a estos documentos anidados con la siguiente sintaxis:
```sh
db.flightData.find({
  "status.description": "on-time"
}).pretty()
```

![[find-one-2.png]]

# Schemas

Los esquemas son modelos que permiten estructurar los documentos de una cierta manera, por defecto MongoDB no te forza a usar los esquemas dentro de las colecciones, sin embargo, eso no significa que no pueda utilizarse, de hecho es recomendable utilizarlos para poder mantener una estructura común entre los documentos.

Por ejemplo:
```sh
db.products.insertOne({
  name: "A book",
  price: 12.99,
})

db.products.insertOne({
  name: "A T-shirt",
  price: 29.99,
})

db.products.insertOne({
  name: "A laptop",
  price: 1299.99,
  details: {
    cpu: "Intel i7",
    memory: 16,
    screen: 15
  }
})
```

![[schemas.png]]


# Tipos de datos

- Text
- Boolean
- Number
- Integer (int32)
- NumberLong (int64)
- NumberDecimal (12.99)
- ObjectId
- ISODate
- Timestamp
- Embedded Document
- Array

```sh
db.companies.insertOne({
  name: "Fresh Apples Inc.",
  isStartup: true,
  employees: 33,
  funding: 12345678901234567890,
  details: {
    ceo: "Mark Super",
    country: "USA"
  },
  tags: [
    {
      title: "super"
    },
    {
      title: "perfect"
    }
  ],
  foundingDate: new Date(),
  insertedAt: new Timestamp()
})
```

![[data-types.png]]

# Data Schemas & Data Modelling

