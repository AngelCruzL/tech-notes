---
tags: programming
---
----

[Documentación]()

----

# ¿Qué es Dart

La aplicación se ejecuta desde la función main,

```dart
void main() {
  String name = 'Luis';
  int year = 2021;
  print('Hola ${name} - ${year}');
}
```

```dart
void main() {
  String name = 'Luis';
  int year = 2021;
  print('Hola ${name.runtimeType} - ${year.runtimeType} 🖖');
}
```

La propiedad runtimeType es gracias a que cualquier propiedad creada en Dart extiende de una super clase padre, esta propiedad permite conocer el tipo de dato de la variable

## Declaración de variables

A los tipos number, bool y double si no se les declara un valor inicial por defecto Dart les asignará un valor null

Al igual que como ocurre con javascript las variables se pueden modificar en tiempo de ejecución si estas se declaran con var en lugar de el tipo de dato que se desea que sea la variable, y la declaración de constantes se hace anteponiendo la palabra reservada final, sin embargo en final se puede asignar el valor en tiempo de ejecución, sin embargo con const no se puede asignar el valor en tiempo de ejecución.

Para declaración de tipos de datos dinámicos en Dart se utiliza la palabra dynamic

.isNegative -> int,double

.length

dynamic se recomienda utilizar en los llamados a las apis

```dart
void main() {
  dynamic apiResponse=[2,3,4,1];
  List<int> data=apiResponse;
  print(data);
}
```

## Funciones

Las funciones son...

```dart
void main() {
  printDate();
  int total = sum(10,2);
  print('La suma es $total');
}

void printDate() {
  print(DateTime.now());
}

int sum(int num1,int num2) {
  int result = num1+num2;
  return result;
}
```

Para hacer uso de los parámetros opcionales en Dart se usa la siguiente sintaxis:

```dart
void main() {
  printDate();
  int total = sum(10, 2, message: 'Esto es un mensaje', date: DateTime.now());
  print('La suma es $total');
}

void printDate() {
  print(DateTime.now());
}

int sum(int num1, int num2, {String message, DateTime date}) {
  print(message);
  print(date);
  int result = num1 + num2;
  return result;
}
```

Una vez definidos los parámetros opcionales después no se les puede definir ningún otro valor, también al hacer uso de los parámetros opcionales al ser estos referenciados con la relación llave valor, su orden al enviarlos por la función no es importante, o al menos con esta notación.

Existe otra notación para llamar a los parámetros opcionales, la cual es

```dart
void main() {
  printDate('Hola mundo', 26);
}

void printDate([String message, int age]) {
  print(message);
  print(age);
}
```

Para evitar pasar parámetros opcionales se envía un null

## Clases

Para definir clases en Dart utilizamos la palabra reservada `class` seguida del nombre de la clase, este nombre debe comenzar con mayúscula en caso de ser una clase pública y con un `_` en caso de ser una clase privada, ya que a diferencia de otros lenguajes de programación en Dart **no existen** las propiedades `protected` `private` `public`.

Por ejemplo:

```dart
class Vehiculo {
  String placa;
  String modelo;
  String marca;
  int year;
}

class _Vehiculo {
  String placa;
  String modelo;
  String marca;
  int year;
}

```

En Dart para instanciar una clase podemos hacer uso de las siguientes sintaxis:

```dart
var miVehiculo = new Vehiculo();

Vehiculo miVehiculo = new Vehiculo();

Vehiculo miVehiculo = Vehiculo();
```

A la hora de definir los valores de las propiedades de la clase se puede hacer de dos maneras:

```dart
Vehiculo miVehiculo = new Vehiculo();
miVehiculo.placa = 'PL4CA';
miVehiculo.modelo = 'Model 3';
miVehiculo.marca = 'Tesla';
miVehiculo.year = 2021;
```

Y la segunda es mandar las propiedades como argumentos a la hora de instanciar la clase, para ello en la declaración de la clase se necesita crear un constructor que reciba estos parámetros.

```dart
class Vehiculo {
  String placa;
  String modelo;
  String marca;
  int year;

  Vehiculo(String placa, String modelo, String marca, int year) {
    this.placa = placa;
    this.modelo = modelo;
    this.marca = marca;
    this.year = year;
  }
}

//  Instanciando la clase:
Vehiculo miVehiculo = new Vehiculo('PL4CA', 'Mazda 3', 'Mazda', 2019);
```

En Dart existe otra manera de poder inicializar los valores de una clase dentro del constructor, la cuál es la siguiente:

```dart
Vehiculo(this.placa, this.modelo, this.marca, this.year);
```

De esta última sintaxis se desprende la siguiente:

```dart
void main() {
  Vehiculo miVehiculo = new Vehiculo(
    placa: 'PL4CA',
    modelo: 'Mazda 3',
    marca: 'Mazda',
    year: 2019,
  );
  print(miVehiculo.placa);
}

class Vehiculo {
  String placa;
  String modelo;
  String marca;
  int year;

  Vehiculo({
    this.placa,
    this.modelo,
    this.marca,
    this.year,
  });
}
```

Esta última sintaxis nos permite asociarle nombres a las variables que mandamos al constructor al momento de instanciar la clase, por lo que estas variables pueden mandarse en distinto orden.

Generalmente al trabajar con clases las propiedades de estas se declararán como `final`, por lo que de querer modificarlas se debe crear una copia de la clase o instancia original modificando ciertos valores:

```dart
void main() {
  Vehiculo miVehiculo = new Vehiculo(
    placa: 'PL4CA',
    modelo: 'Mazda 3',
    marca: 'Mazda',
    year: 2019,
  );
  print(miVehiculo.placa);

  miVehiculo = miVehiculo.copyWith(placa: 'NUEVA PL4CA');
  print(miVehiculo.placa);
}

class Vehiculo {
  final String placa;
  final String modelo;
  final String marca;
  final int year;

  Vehiculo({
    this.placa,
    this.modelo,
    this.marca,
    this.year,
  });

  Vehiculo copyWith({
    String placa,
    String modelo,
    String marca,
    int year,
  }) {
    return Vehiculo(
      placa: placa ?? this.placa,
      modelo: modelo ?? this.modelo,
      marca: marca ?? this.marca,
      year: year ?? this.year,
    );
  }
}

```

## Null conditional

En el código anterior se aplico el null conditional, para ese caso funciona de la siguiente manera:

```dart
placa: placa ?? this.placa
```

Si placa que es el valor que esta a la izquierda de los dos `??` es distinto de nulo la propiedad tomará ese valor, de ser nulo tomará lo que esta a la derecha de ambos `??` (siendo `this.placa`).

## Sobreescritura de métodos

Para sobreescribir un método se utiliza el decorador `@override` dentro de la definición de la clase que se desea sobreescribir, para este caso se sobreescribira el método toString que viene por defecto en Dart.

En Dart toda variable, instancia de una clase y constante tiene un método llamado toString, el cuál pertenece a la clase Object que es la clase padre de todas las clases en Dart.

Para sobreescribirlo se hace de la siguiente manera:

```dart
void main() {
  Vehiculo miVehiculo = new Vehiculo(
    placa: 'PL4CA',
    modelo: 'Mazda 3',
    marca: 'Mazda',
    year: 2019,
  );

  String asString = miVehiculo.toString();
  print(asString);

  miVehiculo = miVehiculo.copyWith(placa: 'NUEVA PL4CA', year: 2021);
  print(miVehiculo.toString());
}

class Vehiculo {
  final String placa;
  final String modelo;
  final String marca;
  final int year;

  Vehiculo({
    this.placa,
    this.modelo,
    this.marca,
    this.year,
  });

  Vehiculo copyWith({
    String placa,
    String modelo,
    String marca,
    int year,
  }) {
    return Vehiculo(
      placa: placa ?? this.placa,
      modelo: modelo ?? this.modelo,
      marca: marca ?? this.marca,
      year: year ?? this.year,
    );
  }

  @override
  String toString() {
    return """
placa: $placa
marca: $marca
modelo:$modelo
año:$year
    """;
  }
}
```
