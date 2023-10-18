---
tags: guide
---
----

# Creación de una API con C# y Entity Framework

1. Crea un nuevo proyecto de [ASP.NET](http://ASP.NET) Web API en Visual Studio.
2. Asegúrate de que Entity Framework esté instalado en tu proyecto. Puedes hacer esto mediante la instalación del paquete NuGet "EntityFramework".
3. Define el modelo de datos de tu aplicación. Puedes hacer esto creando clases que representen las tablas de la base de datos, y luego decorarlas con atributos de Entity Framework para definir cómo se deben mapear a la base de datos.
4. Crea un contexto de base de datos de Entity Framework que herede de DbContext. Este contexto debe incluir propiedades DbSet que representen las tablas de la base de datos que deseas utilizar en tu API.
5. Implementa los métodos CRUD (crear, leer, actualizar y eliminar) para tu API utilizando Entity Framework. Puedes hacer esto definiendo controladores que hereden de ApiController, y luego utilizando métodos de Entity Framework como SaveChanges() para interactuar con la base de datos.
6. Configura las rutas de tu API en el archivo de configuración de ruta. Puedes hacer esto en el método Configure del archivo Startup.cs. Asegúrate de que las rutas estén configuradas para que coincidan con los controladores que creaste en el paso anterior.
7. Prueba tu API utilizando herramientas como Postman o Swagger. Puedes enviar solicitudes HTTP a tu API para verificar que se están devolviendo los resultados esperados.

Es importante tener en cuenta que estos son solo los pasos básicos para comenzar una API con C# y Entity Framework. En la práctica, hay muchos otros aspectos a considerar, como la seguridad, la autenticación, la autorización, el manejo de errores, la optimización del rendimiento, entre otros. Por lo tanto, te recomiendo que consultes la documentación oficial de [ASP.NET](http://ASP.NET) y Entity Framework para obtener más información y mejores prácticas.

# Creación proyecto

```sh
dotnet webapi --name <ProjectName>
```

## Agregamos paquetes

Para agilizar el desarrollo podemos apoyarnos de paquetes, estos al ser agregados al proyecto añaden una referencia en el archivo `<ProjectName>.csproj`.

### AutoMapper

AutoMapper es una herramienta útil para reducir la cantidad de código que se necesita para mapear objetos entre sí en una aplicación de .NET. Si bien no es necesario utilizar AutoMapper, puede ser una forma eficiente de mejorar la productividad y reducir la cantidad de errores potenciales al trabajar con objetos mapeados.

```bash
dotnet add package AutoMapper
```

### Microsoft.EntityFrameworkCore

Microsoft.EntityFrameworkCore es un paquete de NuGet que proporciona una implementación de Entity Framework Core, un ORM que te permite interactuar con una base de datos relacional utilizando objetos y consultas en C#.

```bash
dotnet add package Microsoft.EntityFrameworkCore
```

### Microsoft.EntityFrameworkCore.Design

Microsoft.EntityFrameworkCore.Design proporciona herramientas de diseño de Entity Framework Core para el desarrollo de aplicaciones de .NET. Este paquete se utiliza para generar código de Entity Framework Core, como clases de contexto de base de datos y clases de modelo, a partir de una base de datos existente.

```bash
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Microsoft.EntityFrameworkCore.SqlServer

Proporciona una implementación del proveedor de bases de datos de Microsoft SQL Server para Entity Framework Core. Este paquete se utiliza para interactuar con una base de datos de SQL Server utilizando Entity Framework Core.

```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### Newtonsoft.Json

Newtonsoft.Json es un paquete de NuGet que proporciona una biblioteca para trabajar con JSON (JavaScript Object Notation) en aplicaciones de .NET. JSON es un formato de intercambio de datos ligero y fácil de leer que se utiliza comúnmente en aplicaciones web y móviles.

```bash
dotnet add package Newtonsoft.Json
```

### Microsoft.AspNetCore.Identity.EntityFrameworkCore

Microsoft.AspNetCore.Identity.EntityFrameworkCore es un paquete de NuGet que proporciona una implementación de Entity Framework Core para la administración de identidad en aplicaciones web de [ASP.NET](http://ASP.NET) Core. Este paquete se utiliza para gestionar la autenticación y autorización de usuarios en aplicaciones web de [ASP.NET](http://asp.net/) Core.

```bash
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
```

### Microsoft.AspNetCore.Authentication.JwtBearer

Microsoft.AspNetCore.Authentication.JwtBearer es un paquete de NuGet que proporciona soporte para la autenticación basada en tokens JWT (JSON Web Tokens) en aplicaciones web de [ASP.NET](http://asp.net/) Core. JWT es un estándar abierto para la creación de tokens de acceso seguro y compactos que se utilizan para la autenticación y autorización en aplicaciones web y móviles.

```bash
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

### System.IdentityModel.Tokens.Jwt

System.IdentityModel.Tokens.Jwt es una biblioteca de clases que proporciona herramientas para crear y validar tokens JWT (JSON Web Tokens) en aplicaciones de .NET Framework y .NET Core. JWT es un estándar abierto para la creación de tokens de acceso seguro y compactos que se utilizan para la autenticación y autorización en aplicaciones web y móviles.

```bash
dotnet add package System.IdentityModel.Tokens.Jwt
```

## Configuración del proyecto

Dentro del archivo `Program.cs` realizamos la siguiente modificación:

```csharp
app.UseAuthentication();
// app.UseHttpsRedirection();
```

Y en el archivo `launchSettings.json` eliminamos el objeto `https` localizado dentro de `profiles`.

## Creación de Modelos

Un modelo (Model en inglés) es una clase o estructura de datos que representa la información que se utiliza en una aplicación web. Un modelo contiene los datos que se muestran en las vistas y se utiliza para interactuar con la base de datos a través de Entity Framework.

Ejemplo:

`Models/Estate.cs`

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace NetKubernetes.Models;

public class Estate
{
 [Key]
 [Required]
 public int Id { get; set; }
 public string? Name { get; set; }
 public string? Address { get; set; }
 [Required]
 [Column(TypeName = "decimal(18,2)")]
 public int Price { get; set; }
 public string? Picture { get; set; }
 public DateTime? CreatedAt { get; set; }
}
```

`Models/User.cs`:

```csharp
using Microsoft.AspNetCore.Identity;

namespace NetKubernetes.Models;

public class User : IdentityUser
{
 public string? Name { get; set; }
 public string? Lastname { get; set; }
}
```

`Data/AppDbContext.cs`:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using NetKubernetes.Models;

namespace NetKubernetes.Data;

public class AppDbContext : IdentityDbContext<User>
{
 public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
 {
 }

 protected override void OnModelCreating(ModelBuilder builder)
 {
   // This generates all the tables for the Identity framework
   base.OnModelCreating(builder);
 }

 public DbSet<Estate>? Estates { get; set; }
}
```

Este código define una clase llamada `AppDbContext` que extiende la clase `IdentityDbContext` de [ASP.NET](http://asp.net/) Core Identity. La clase `IdentityDbContext` se utiliza para definir un contexto de base de datos que incluye las tablas necesarias para la autenticación y la gestión de roles de usuario.

La clase `AppDbContext` también define un constructor que toma un objeto de opciones de contexto de base de datos y lo pasa a la clase base para inicializar el contexto de la base de datos.

Además, la clase "AppDbContext" define un DbSet para la entidad "Estate". DbSet es una propiedad que se utiliza para acceder a una tabla específica en la base de datos. En este caso, la tabla "Estates" está representada por la clase "Estate" y se puede acceder a ella a través de la propiedad "Estates" en el contexto de la base de datos.

Por último, la clase "AppDbContext" anula el método "OnModelCreating" para proporcionar configuraciones personalizadas para el modelo de datos generado por Entity Framework. En este caso, la llamada a "base.OnModelCreating(builder)" se utiliza para generar todas las tablas necesarias para el framework de identidad de [ASP.NET](http://asp.net/) Core.

`Data/LoadDatabase.cs`

```csharp
using Microsoft.AspNetCore.Identity;
using NetKubernetes.Models;

namespace NetKubernetes.Data;

public class LoadDatabase
{
 public static async Task InsertDataAsync(AppDbContext context, UserManager<User> userManager)
 {
   if (!userManager.Users.Any())
   {
     var user = new User
     {
       Name = "Angel",
       Lastname = "Cruz",
       Email = "me@angelcruzl.dev",
       UserName = "AngelCruzL",
       PhoneNumber = "1234567890"
     };

     await userManager.CreateAsync(user, "$ecretP@ssw0rd");
   }

   if (!context.Estates!.Any())
   {
     context.Estates!.AddRange(
       new Estate
       {
         Name = "Casa en la playa",
         Address = "Calle 1, Playa del Carmen, Quintana Roo",
         Price = 1000000,
         Picture = "<https://picsum.photos/200/300>",
         CreatedAt = DateTime.Now
       },
       new Estate
       {
         Name = "Casa en la montaña",
         Address = "Calle 2, San Cristobal de las Casas, Chiapas",
         Price = 2000000,
         Picture = "<https://picsum.photos/200/300>",
         CreatedAt = DateTime.Now
       },
       new Estate
       {
         Name = "Casa en el campo",
         Address = "Calle 3, Tlaxcala, Tlaxcala",
         Price = 3000000,
         Picture = "<https://picsum.photos/200/300>",
         CreatedAt = DateTime.Now
       }
     );
   }

   await context.SaveChangesAsync();
 }
}
```

Este código define una clase llamada `LoadDatabase` que contiene un método estático llamado "InsertDataAsync". Este método toma dos argumentos: un objeto "AppDbContext" y un objeto "UserManager" de [ASP.NET](http://ASP.NET) Core Identity.

El método "InsertDataAsync" comprueba si no hay usuarios en la base de datos y, si no los hay, crea un nuevo usuario utilizando el objeto "UserManager". En este caso, el usuario creado tiene el nombre "Angel", el apellido "Cruz", la dirección de correo electrónico "**[me@angelcruzl.dev](mailto:me@angelcruzl.dev)**", el nombre de usuario "AngelCruzL" y el número de teléfono "1234567890". El usuario también se crea con una contraseña encriptada "$ecretP@ssw0rd".

A continuación, el método "InsertDataAsync" comprueba si no hay registros en la tabla "Estates" de la base de datos y, si no los hay, crea tres nuevos registros utilizando el objeto "AppDbContext". Cada registro representa una propiedad inmobiliaria y contiene el nombre, la dirección, el precio, la imagen y la fecha de creación. Estos registros se agregan a la tabla "Estates" utilizando el método "AddRange" de "DbSet".

Por último, el método "InsertDataAsync" llama al método "SaveChangesAsync" de "AppDbContext" para guardar los cambios en la base de datos. En general, este código se utiliza para inicializar la base de datos con datos iniciales cuando se ejecuta la aplicación.