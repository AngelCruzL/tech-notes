---
tags: programming
---
----

[Documentación]()

----

# ¿Qué es C#?

Es un lenguaje de programación multi-paradigma, esto quiere decir que soporta distintos estilos de programación.

Existe una diferencia entre C# y .NET, C# es un lenguaje de programación, mientras que .NET es un término general que cubre el framework .NET (que es una biblioteca de marcos de aplicaciones) y el tiempo de ejecución que se necesita para ejecutar aplicaciones.

## Arquitectura de aplicaciones .NET

![[Csharp-architecture.png]]

Entre la arquitectura de estas aplicaciones existen 4 implementaciones principales:

- Implementaciones .NET
- .NET Standard Library
- Entorno en tiempo de ejecución
- Infraestructura común

En las implementaciones de .NET se utiliza para desarrollar aplicaciones de Windows, Windows Mobile y Windows Server.

.NET Standard Library se utiliza para desarrollo de aplicaciones móviles.

## Creación de un nuevo proyecto

Para poder crear un nuevo proyecto en C# debemos ejecutar el siguiente comando:

```bash
dotnet new console --output <proyecto>
```

Para poder ejecutar el proyecto debemos usar el siguiente comando:

```bash
dotnet run --project <proyecto>
```

## Estructura básica de un programa de consola en C\#

```c#
using System;

namespace HelloWorld
{
  class Program
  {
    static void Main(string[] args)
    {
      Console.WriteLine("Hola a todos 😀");
    }
  }
}
```

Los identificadores son un nombre o una etiqueta que se le asigna a los elementos del programa como variables, constantes, métodos, clases, etc.

Los identificadores estarán formados por letras y dígitos y su finalidad es identificar los distintos componentes del programa

```c#
public class AprendiendoCS
{
  static public void Main ()
  {
    int x;
  }
}
```

Para definir a los identificadores se deben seguir ciertas reglas, ya que de lo contrario pueden presentarse errores de compilación, entre estas reglas están:

- Utilizar caracteres alfanuméricos
- No utilizar espacios
- No comenzar con dígitos
- No utilizar palabras reservadas, en caso de usar un @ antes para que sea permitido
- Es key sensitive, por lo que distingue mayúsculas de minúsculas
- No usar más de 512 caracteres
- No añadir guiones bajos

Existen convenciones para el nombre de los identificadores, entre las nomenclaturas más comunes están:

- camelCase
- PascalCase
- \_underScore

## Manejo de mensajes por pantalla

Para gestionar de manera exitosa los mensajes por pantalla se hace uso de

- Write
- WriteLine
- ReadLine

### Write

La clase Write es utilizada para escribir mensajes por pantalla, no tiene saltos de línea.

```c#
Console.Write("Hola");
```

### WriteLine

La clase WriteLine al igual que Write te permite escribir mensajes por pantalla, pero a diferencia de éste último, WriteLine si contiene saltos de línea.

```c#
Console.WriteLine("Hola con salto de línea");
```

### ReadLine

La clase ReadLine nos permite capturar los datos que el usuario introduce al programa para hacer uso de estos, ya sea en ese momento o más adelante en el programa, pues dicha información puede almacenarse en variables.

En el siguiente ejemplo se le solicita al usuario su edad y después se imprime por pantalla con ayuda de la concatenación de variables de C#:

```c#
  Console.WriteLine("Introduce tu edad:");
  Console.Write($"Mi edad es: {Console.ReadLine()} años");
```

## Variables

Las variables son posiciones de memoria a la que se le asigna un dato, estas pueden trabajar con datos simples y no simples.

Existen también constantes, la diferencia entre las variables y constantes es que como su nombre lo indica las variables pueden modificar su valor y las constantes no.

## Datos Simples

### Numéricos con signo

| Dato  | Rango                                                  | Bits    | .NET         | Valor inicia |
| ----- | ------------------------------------------------------ | ------- | ------------ | ------------ |
| sbyte | -128 / 127                                             | 8       | System.SByte | 0            |
| short | -32 768 / 32 767                                       | 16      | System.Int16 | 0            |
| int   | -2.147.483.648 / 2.147.483.647                         | 32 bits | System.Int32 | 0            |
| long  | -9.223.372.036.854.775.808 / 9.223.372.036.854.775.807 | 64 bits | System.Int64 | 0            |

### Numéricos sin signo

| Dato  | Rango                          | Bits    | .NET         | Valor inicia |
| ----- | ------------------------------ | ------- | ------------ | ------------ |
| sbyte | 0 / 255                        | 8       | System.SByte | 0            |
| short | 0 / 65.535                     | 16      | System.Int16 | 0            |
| int   | 0 / 4.294.967.295              | 32 bits | System.Int32 | 0            |
| long  | 0 / 18.446.744.073.709.551.615 | 64 bits | System.Int64 | 0            |

### Numéricos no enteros

| Dato    | Rango                           | Dígitos | .NET           | Valor inicia |
| ------- | ------------------------------- | ------- | -------------- | ------------ |
| float   | ±1,5 x 10^-45 / ±3,4 x 10^38    | 7       | System.Single  | 0.0f         |
| double  | ±5,0 × 10^−324 / ±1,7 ×10^308   | 15-16   | System.Double  | 0.0d         |
| decimal | ±1,0 x 10^-28 to ±7,9228 x10^28 | 28-29   | System.Decimal | 0m           |

### No numéricos

| Dato | Rango           | Valores                     | .NET           | Valor inicia |
| ---- | --------------- | --------------------------- | -------------- | ------------ |
| char | U+0000 a U+FFFF | Carácter Unicode de 16 bits | System.Char    | \x0000       |
| bool | Booleano        | true, false                 | System.Boolean | false        |

## Operadores de asignación

### Aritméticos

### Lógicos

### Desplazamiento bit a bit
