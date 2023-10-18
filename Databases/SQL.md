---
tags: database
---
----

[SQL Tutorial (w3schools.com)](https://www.w3schools.com/sql/)

----

# ¿Qué es SQL?

SQL (Structured Query Language) en español Lenguaje de consulta Estructurado, es un lenguaje específico utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales. Una de sus principales características es el manejo de álgebra y el cálculo relacional para realizar consultas y obtener información de forma sencilla, y además para realizar cambios en ella.

En SQL las columnas son llamadas campos y las filas son llamados registros.

Un tipo de dato es el tipo de valor que va a contener ese campo, al almacenar números se utiliza el **int**, para caracteres se utiliza el **char**.

SQL (Structured Query Language) en español Lenguaje de consulta Estructurado, es un lenguaje específico utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales. Una de sus principales características es el manejo de álgebra y el cálculo relacional para realizar consultas y obtener información de forma sencilla, y además para realizar cambios en ella.

En SQL las columnas son llamadas campos y las filas son llamados registros.

Un tipo de dato es el tipo de valor que va a contener ese campo, al almacenar números se utiliza el **int**, para caracteres se utiliza el **char**.

It’s a language what allows us to interact with a database (set of data), the queries is the instruction (SQL statement), for example:

```jsx
SELECT * FROM USERS
```

![[query-breakdown.png]]

SQL is a declarative language, this meaning that declares what will happen,

Imperative is how it will happen

SQL is divided on tables, this tables has columns and rows, each column has a name to store specific data

![[domain.png]]

![[attributes.png]]

![[rows.png]]

Es convencion en este lenguaje escribir todos los comandos en mayuscula y es importante colocar un `;` al final de un comando.

# Tipos de datos

Al igual que los lenguajes de programación tradicionales, el lenguaje SQL contiene distintos tipos de datos, entre los más comunes podemos encontrar los siguientes:

- varchar → texto corto, o que varia en su extensión (hasta 255 caracteres)
- char → texto corto de extensión fija (común para contraseñas o datos encriptados)
- text → texto largo (común en entradas de blog)
- enum → Un valor de una lista numerada
- blob → Útil para almacenar imagenes, sonidos, y archivos comprimidos
- int → números enteros
- tinyint → números enteros pequeños (como edades)
- decimal → números con valores decimales (dinero y medidas)
- datetime → útil para fechas

# Manejo de tablas

Para poder mostrar las bases de datos del equipo ejecutamos el siguiente comando:

```sql
SHOW DATABASES;
```

Para crear nuevas tablas por medio de comandos ejecutamos la siguiente instrucción:

```sql
CREATE DATABASE <name>;
```

Usa la base de datos name para las futuras queries o instrucciones que se le daran a la base de datos:

```sql
USE <db_name>;
```

```sql
SHOW DATABASES;
	Muestra las bases de datos en el equipo

CREATE DATABASE <NAME>;
	Crea la base de datos name

USE <NAME>;
	Usa la base de datos name para las futuras queries o instrucciones que se le daran
	a la base de datos

SHOW TABLES;

CREATE TABLE servicios(
	id INT(11) NOT NULL AUTO_INCREMENT,
	nombre VARCHAR(60) NOT NULL,
	precio DECIMAL (6,2) NOT NULL,          (6 digitos de los cuales 2 seran decimales)
	PRIMARY KEY (id)
);

DESCRIBE servicios;
	Muestra de manera mas detallada la tabla de servicio
```

## Insertar registros dentro de tablas

```sql
INSERT INTO servicios (nombre, precio) VALUES ("Corte de cabello para hombre",80);

INSERT INTO servicios (nombre, precio) VALUES
	("Corte de cabello para hombre",80),
	("Peinado para mujer", 60);
```

## Modificación de las columnas de tablas ya creadas

```sql
ALTER TABLE servicios ADD descripcion VARCHAR(100) NOT NULL;
	Modifica la tabla de servicios y a~ade la columna de descripcion, con un
	tipo de dato varchar y sin la posibilidad de ser nulo

ALTER TABLE servicios CHANGE descripcion nuevonombre VARCHAR(11) NOT NULL;
	Modifica el nombre de la columna descripcion en la tabla servicios y lo
	cambia por nuevonombre
```

## Eliminacion de tablas

```sql
DROP TABLE servicios;
	Elimina la tabla servicios

CREATE TABLE reservaciones (
	id INT(11) NOT NULL AUTO_INCREMENT,
  nombre VARCHAR(60) NOT NULL,
  apellido VARCHAR(60) NOT NULL,
  hora TIME DEFAULT NULL,
  fecha DATE DEFAULT NULL,
  servicios VARCHAR(255) NOT NULL,
  PRIMARY KEY (id)
);
```

## Renombrar tabla

```sql
RENAME TABLE reservaciones TO reservacion;
```

# Comandos para trabajo SQL

## Comandos para SELECT

```sql
SELECT * FROM servicios;
	Realiza una consulta de servicios trayendo todos los registros disponibles

SELECT nombre, precio FROM servicios;
	Realiza una consulta de servicios y trae solo los registros de los nombres y precios

SELECT nombre, id, precio FROM servicios ORDER BY precio;
	Consulta el nombre, id, precio de servicios y los ordena deacuerdo al precio
	(del menor al mayor)

SELECT nombre, id, precio FROM servicios ORDER BY precio ASC;
	Consulta el nombre, id, precio de servicios y los ordena deacuerdo al precio
	de manera ascendente

SELECT nombre, id, precio FROM servicios ORDER BY precio DESC;
	Consulta el nombre, id, precio de servicios y los ordena deacuerdo al precio
	de manera descendente

SELECT nombre, id, precio FROM servicios LIMIT 2;
	Consulta los dos primeros registros del nombre, id, precio de servicios.

SELECT nombre, id, precio FROM servicios WHERE id = 3;
	Consulta el nombre, id, precio de servicios del registro con el id de 3.
```

Dentro del select podemos mandar comandos para realizar busquedas más especificas:

`LIMIT` → Limita la cantidad de resultados que pueda mostrar la consulta, mostrando solo las primeras coincidencias de búsqueda

`WHERE` → Establece una condición

`WHERE <> AND <>` → Establece una concatenación de condiciones, mostrara solo los registros que cumplan con todas las condiciones especificadas

`WHERE <> OR <>` → Establece dos posibles condiciones, mostrando los registros que cumplan con cualquiera de ambas

`WHERE <> != <>` → Donde el valor de la condición sea distinto al que se manda en la sentencia

`WHERE <> BETWEEN <> AND <>` → Donde el valor que se establece en el where se encuentre entre dos valore proporcionados

```sql
SELECT * FROM servicios WHERE precio BETWEEN 100 AND 200;
```

```sql
SELECT MAX(precio) AS mayor FROM servicios;
```

```sql
SELECT MIN(precio) AS menor FROM servicios;
```

```sql
SELECT AVGprecio) AS precio_promedio FROM servicios;
```

```sql
SELECT * FROM <table> WHERE <property> LIKE 'Corte%';
```
Funciona para traer todos los resultados de la tabla `<table>` donde la propiedad `<property>` inicia con Corte

```sql
SELECT * FROM <table> WHERE <property> LIKE '%Cabello';
```
Funciona para traer todos los resultados de la tabla `<table>` donde la propiedad `<property>` termina con Cabello

```sql
SELECT * FROM <table> WHERE <property> LIKE '%Lavado%';
```
Funciona para traer todos los resultados de la tabla `<table>` donde la propiedad `<property>` contiene Lavado en medio o al inicio o al final

```sql
SELECT * FROM reservaciones
WHERE CONCAT(nombre, " ", apellido)
LIKE '%Ana Preciado%';
```

```sql
SELECT hora, fecha, CONCAT(nombre, " ", apellido) AS 'Nombre Completo'
FROM reservaciones
WHERE CONCAT(nombre, " ", apellido)
LIKE '%Ana Preciado%';
```

`IN` sirve como un shorthand para condiciones `OR`

```sql
SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');
```

## Actualizar registros

El comando para actualizar un registro dentro de la base de datos es el siguiente

```sql
UPDATE <db_name> SET <field> = <new_field_value> WHERE <condition>;

# Ejemplo:
UPDATE animales SET estado = 'feliz' WHERE id = 3;
```

```sql
UPDATE servicios SET precio = 70 WHERE id = 2;
	Se actualiza el precio del id 2 a 70

UPDATE servicios SET nombre= "Corte actualizado", precio = 120 WHERE id = 1;
	Actualiza el nombre y precio del registro con el id 1
```

## Eliminar registros

El comando para eliminar un registro dentro de la base de datos es el siguiente

```sql
DELETE FROM <db_name> WHERE <condition>;

# Ejemplo:
DELETE FROM animales WHERE id = 3;
```

```sql
DELETE FROM servicios WHERE id = 2;
	Elimina el registro con el id 2
```

# Reglas de normalizacion

Existen tres reglas de normalizacion, la primera establece lo siguiente

> Cada columna debe tener solo 1 valor y no debe haber grupos repetidos

La segunda regla se aplica tras la primera y marca lo siguiente:

> En esta regla se utilizan llaves primarias compuestas

# Denormalizacion

Son bases de datos que no siguen las reglas de normalizacion

# Cardinalidad

La cardinalidad se refiere a las relaciones que pueden tener unas tablas con otras

![[cardinalidad.png]]

## Primary Key

The primary key is a unique identifier to distinguish your data, and this allows you create relationships

![[primary-key.png]]

## Foreign key

The foreign key allows you to create a relationship with a different table, the foreign key refers the primary key from another table

![[foreign-key.png]]

```sql
CREATE TABLE product (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(50),
	created_by INT NOT NULL,
	PRIMARY KEY(id),
	FOREIGN KEY(created_by) REFERENCES user(id)
);
```

# Uniones

## Left join

Al seleccionar los datos se retornan los que sean de la tabla de la izquierda y los de la derecha que tengan relación con la de la izquierda

![[left-join.png]]

```sql
SELECT u.id, u.email, p.name FROM user AS u
LEFT JOIN product AS p
ON u.id = p.created_by;
```

## Right JOIN

Similar al left join pero ahora regresará las coincidencias de la tabla 2 y las relaciones que tenga con la tabla 1

![[right-join.png]]

```sql
SELECT u.id, u.email, p.name FROM user AS u
RIGHT JOIN product AS p
ON u.id = p.created_by;
```

## Inner Join

![[inner-join.png]]

```sql
SELECT u.id, u.email, p.name FROM user AS u
INNER JOIN product AS p
ON u.id = p.created_by;
```

## Cross join

Relaciona cada campo de una tabla con cada campo de la otra tabla

![[cross-join.png]]

```sql
SELECT u.id, u.email, p.id FROM user AS u
CROSS JOIN product AS p;
```

# Índices

Los indices sirven para que los campos que poseen esta propiedad sean cargados en memoria, lo cual permite que al momento de buscar estos registros la búsqueda sea mucho más rápida
