---
tags: guide c# backend
---
----

# ¿Qué es Entity Framework?

Entity Framework es un ORM (Object-Relational Mapping) de Microsoft que se utiliza en el desarrollo de aplicaciones en C#. Con Entity Framework, puedes interactuar con una base de datos relacional utilizando objetos y consultas en C# en lugar de tener que escribir sentencias SQL directamente.

Entity Framework se encarga de traducir las operaciones realizadas en objetos de C# a sentencias SQL para interactuar con la base de datos. También maneja automáticamente la creación y actualización de la estructura de la base de datos en función de las definiciones de clases de C#.

Existen dos formas principales de trabajar con Entity Framework:

1. **Database First**: En esta opción, primero se crea la estructura de la base de datos y luego se genera el modelo de objetos de C# a partir de ella.
2. **Code First**: En este caso, primero se definen las clases y relaciones de objetos de C# y luego se genera la estructura de la base de datos a partir de ellas.

Entity Framework también admite otras características, como el seguimiento de cambios en objetos y la carga perezosa de datos para mejorar el rendimiento de la aplicación.

En resumen, Entity Framework es una herramienta de mapeo objeto-relacional que te permite interactuar con bases de datos utilizando objetos y consultas en C#, lo que facilita el desarrollo de aplicaciones y reduce la cantidad de código que se debe escribir para interactuar con la base de datos.


# Instalación de Entity Framework

Dentro del directorio del proyecto en donde queramos realizar la instalación ejecutamos el siguiente comando:

```bash
dotnet add package Microsoft.EntityFrameworkCore --version="<dotnet sdk version>"

dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version="<dotnet sdk version>"
```


# Ejemplo de uso de Entity Framework

En esta guía utilizaremos la opción de trabajar bajo la metodología Code First, así que necesitamos crear nuestras clases que posteriormente serán convertidas a tablas con ayuda de Entity Framework.

Para el ejemplo crearemos una base de datos de un sitio de cursos que obedezca al siguiente diagrama:

Primero comenzamos con la creación de la clase `Curso`:

```csharp
namespace Domain.Entities;

public class Course
{
	public int Id { get; set; }
  public string Title { get; set; }
  public string Description { get; set; }
  public DateTime PublishDate { get; set; }
  public byte[] BannerPicture { get; set; }
}
```

Ahora necesitamos convertir esta clase en una entidad, para esto necesitamos apoyarnos en un DbSet que es un contexto que le agregará a la clase métodos, estos métodos nos permitirán invocar a la librería LINQ

## LINQ

LINQ es una abreviatura de "Language Integrated Query", que se refiere a una característica de C# que permite escribir consultas de bases de datos, colecciones de objetos y servicios web directamente en el código C# en lugar de tener que utilizar lenguajes de consulta separados como SQL o XPath.

Con LINQ, puedes realizar operaciones de filtrado, proyección, ordenamiento y agrupamiento en datos utilizando una sintaxis similar a la de SQL. LINQ también te permite unir múltiples fuentes de datos en una sola consulta y realizar operaciones complejas utilizando métodos y expresiones lambda.

# Creación del DbContext

Una instancia del DbContext representa una instancia de una sesión con la base de datos con la cual quieres conectarte.

Un `DbSet` dentro del DbContext representa una tabla o una vista de la base de datos.
