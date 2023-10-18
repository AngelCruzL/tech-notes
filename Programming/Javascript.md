---
tags: programming frontend javascript
---

---

[JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/javascript)

---

# ¿Qué es Javascript?

Es un lenguaje de programación que permite manipular elementos en el navegador dentro de las páginas web, es por ello que JS permite crear interacción en sitios web.

## Tipos de datos

En JavaScript existen 2 tipos de datos:

- Objetos
- Primitivos
  - Numérico: Enteros, decimales y flotantes
  - String: Secuencia de caracteres
  - Boolean: Datos lógicos (true or false)
  - Undefined: Tipos de datos no definidos
  - Null: Datos que no existen, llamados datos nulos

## Variables

Al ser javascript un lenguaje débilmente tipado no se debe de indicar el tipo de dato de la variable, ya que JavaScript realiza automáticamente la asignación dell tipo de dato dependiendo del valor de la variable, por lo que el tipo de dato se redefinir a la hora de su ejecución.

Existen tres maneras distintas de declarar las variables en Javascript, estas son `var`, `let` y `const`.

`var` redefine la variable a nivel global de la aplicación, por lo que puede redefinir funciones importantes del programa, por ello no recomiendo utilizarlo.

`let` define una variable a nivel [scope](#scope) o bloque, esta no puede ser redefinida pero si puede mutar su valor o su tipo de objeto, adicionalmente a esto pueden existir en distintos bloques variables con el mismo nombre y estas al tener diferente scope no tendrán conflictos entre si.

`const` se utiliza para nombrar a las constantes, en este caso la variable no puede ser redefinida y ademas de esto no puede cambiar su tipo ni su valor.

## Scope

El scope es el alcance que tiene la declaración de una variable, este puede ser local o global dependiendo de en donde sea declarada la variable y el tipo de declaración que tenga.

### Scope Global

Este nivel de alcance permite acceder al valor de una variable que se encuentra en un bloque superior.

```js
const cliente = 'Luis';

function mostrarCliente() {
  console.log(cliente);
}

function obtenerCliente() {
  console.log(cliente);
}

mostrarCliente(); // > Luis
obtenerCliente(); // > Luis
```

### Function Scope

Este alcance solo es a nivel de una función, por lo que si una función que tiene una variable que se llama igual el valor será distinto.

```js
const cliente = 'Luis';
function mostrarCliente() {
  const cliente = 'Angel';
  console.log(cliente);
}

function obtenerCliente() {
  console.log(cliente);
}

mostrarCliente(); // > Angel
obtenerCliente(); // > Luis
```

### Scope por bloque

```js
const login = true;

function clienteLogueado() {
  const cliente = 'Luis';
  console.log(cliente); // > Luis

  if (login) {
    const cliente = 'Admin';
    console.log(cliente); // > Admin
  }
}
clienteLogueado();
```

## Operadores


### Aritméticos

Estos son los encargados de realizar las operaciones aritméticas, por lo que entre estos se encuentran `+`, `-`, `*`, `/`, `**` y `%`

Se puede reducir el código al hacer uso de la siguiente sintaxis

```js
a = a + 5    ->   a += 5
```

De la misma manera anteriormente se utilizaba la función matemática `pow()` para poder elevar a n exponente x cantidad, sin embargo ahora se puede hacer uso del operador `**` para hacerlo:

```js
Math.pow(5, 2) = 25    ->   5 ** 2 = 25
```

Por ultimo tenemos al modulo, con el cual este nos regresara el residuo de una división:

```js
10 % 2 = 0    ó     5 % 2 = 1
```

### De comparación

Estos tienen como función realizar comparaciones entre distintos datos, entre los mas comunes se encuentran los operadores `<`, `>`, `==`, `===`, `!=`
Estos valores regresan después de su comparación un valor booleano

De estos dos últimos operadores de comparación el doble igual compara solo el valor, por ejemplo

```js
4 == '4'; // >  true
```

Sin embargo el triple igual es un operador más estricto, este no solo compara
los valores sino también el tipo de dato

```js
4 === '4'; // >  false
```

El operador != se utiliza para comparar que dos valores sean diferentes

```js
4 != 7; // >  true
```

### Lógicos

Los operadores lógicos mas comunes son `AND` (`&&`) y `OR` (`||`), estos se encargan de comparar una condición, en el caso de AND regresara true si ambas condiciones se cumplen, mientras que con OR se regresara true si al menos una de estas condiciones se cumple (se verán ejemplos prácticos con las condicionales)

Ahora, existen otros operadores denominados como lógicos de arrastre, por ejemplo:

```js
let a = 2;
let b = 4;

console.log(a << b); // Arrastre a la izquierda añade ceros
```

Esto imprimirá 32, ya que transforma el numero dos a su valor en bits (10) y hará un arrastre hacia la izquierda de 4 posiciones, dando como resultado 32 (cuyo valor en bits es 100000)

```js
let a = 32;
let b = 4;

console.log(a >> b); // Arrastre a la derecha elimina ceros
```

Por otro lado en este caso el arrastre sera de 4 posiciones hacia la derecha, por lo que se obtendrá un resultado de 2.

También existe el operador OR a nivel de bit, lo que hará que las cantidades a nivel bit se sumen, por ejemplo:

```js
let a = 2;
let b = 4;

console.log(a | b);
```

Esto imprimirá 6 ya que 2 (010) mas 4 (100) es igual a 6 (110), todo esto trabajado a nivel de bits.

Si se utiliza el operador de AND a nivel de bits este hará una suma, sin embargo en esta suma el resultado sera 1 solo si ambos sumandos son 1, por ejemplo:

```js
let a = 7;
let b = 4;

console.log(a & b);
```

Esto imprimirá 4 ya que 7 (111) mas 4 (100) es igual a 4 (100), todo esto trabajado a nivel de bits.

Al igual que con los operadores aritméticos estas operaciones se pueden
simplificar

```js
a = a << b    ó    a <<= b
```

## Estructuras Condicionales

Los condicionales nos sirven para ejecutar una parte del programa si es que se cumple cierta condición

### If - If / else

En este condicional se va a ejecutar cierta parte del código solamente si se cumple la condición especificada, mientras que con if/else se ejecutara la primera sentencia si se cumple la condición y si no se cumple el segundo bloque de código

```js
const edad = 13;

if (edad >= 18) {
  console.log('Eres mayor de edad, puedes obtener tu licencia');
} else {
  console.error('Aún eres menor para conducir');
}
```

En este caso por consola se imprimirá un error con el texto `Aún eres menor para conducir`, pues la edad no es mayor o igual a 18.

Un ejemplo del uso de AND para las condicionales es:

```js
const usuario = false;
const puedePagar = true;

if (usuario && puedePagar) {
  console.log('Tu Pedido se hizo con éxito...');
} else {
  console.log('hubo un error con tu pago.');
}
```

En el caso del operador OR esta el siguiente ejemplo:

```js
let efectivo = 300;
let credito = 400;
let disponible = efectivo + credito;
let totalCarrito = 700;

if (efectivo > totalCarrito || credito > totalCarrito) {
  console.log('Puedes pagar con efectivo o credito');
} else if (disponible >= totalCarrito) {
  console.log('Paga con ambos');
} else {
  console.log('No puedes pagar');
}
```

### While

En este caso esta estructura de control ejecutara cierto fragmento de código mientras se cumpla una condición

```js
let i = 0;
while (i < 10) {
  // condición

  // Bloque de código...
  console.log(`Numero: ${i}`);

  i++; // incremento
}
```

El while se ejecuta mientras una condición sea verdadera, por lo tanto si la variable inicia con el valor 20, no hará nada.

### Do / While

Esta estructura condicional es similar a la del while, la diferencia es que esta se ejecuta mínimo una vez, pues la evaluación de la condición ocurre justo al final del bloque de código.

```js
let i = 50;

do {
  console.log('El valor de i es menor que 10');
} while (i < 10);
```

### Switch

Esta estructura condicional permite seleccionar entre distintas opciones para evitar caer en muchos if/else anidados, por ejemplo:

```js
const metodoPago = 'efectivo';

switch (metodoPago) {
  case 'efectivo':
    console.log(`Pagaste con ${metodoPago}`);
    break;
  case 'cheque':
    console.log(
      `Pagaste con ${metodoPago} revisaremos que tenga fondos primero`
    );
    break;
  case 'tarjeta':
    console.log(`Pagaste con ${metodoPago} espere un momento...`);
    break;
  default:
    console.log('Aún no has pagado');
    break;
}
```

## ES6 y operadores modernos

Entre estos se encuentran el [operador ternario](#operador-ternario), [spread y rest operator](#spread-and-rest-operator), los operadores de [corto circuito](#short-circuiting-operator), el [operador de fusión nulo](#the-nullish-coalescing-operator) y el [operador de encadenamiento opcional](#optional-chaining-operator)

### Operador ternario

El operador ternario es como un if resumido y este siempre tiene un valor verdadero y uno falso, un ejemplo de su declaración es:

```js
edad >= 18
  ? console.log(nombre + 'es mayor de edad')
  : console.log(nombre + 'no es mayor de edad');
```

En donde el primer segmento es la condición de evaluación, en este caso `edad >= 18`, después de esto viene la parte que se ejecuta en caso de que la condición sea cierta, para este ejemplo es `console.log(nombre + 'es mayor de edad')` y en caso de que la condición sea falsa se ejecuta el último bloque de código, en este caso: `console.log(nombre + 'no es mayor de edad')`

La variable puede tener valores falsos o no falsos (verdaderos).
Los falsos ocurren cuando la variable es `undefined`, `null` o `0`

El `||` y el `&&` también se pueden utilizar en el ternario

```js
console.log(
  autenticado && puedePagar ? 'Si esta autenticado' : 'No esta autenticado'
);
```

### Spread and REST Operator

El spread operator `...` sirve para separar los elementos de los arreglos y los objetos.

```js
// Trabajando con arrays
const numbers = [1, 2, 3];
const newNumbers = [...numbers, 4];
console.log(newNumbers);
// > (4) [1, 2, 3, 4]

// Trabajando con objetos
const person = { name: 'Luis' };
const newPerson = { ...person, age: 22 };
console.log(newPerson);
// > {name: "Luis", age: 22}
```

El rest operator `...` sirve para unir los argumentos de una función dentro de un nuevo arreglo

```js
const filter = (...args) => args.filter((el) => el === 1);
filter(1, 2, 3, 5, 1, 9, 10, 6, 1);
// > (3) [1, 1, 1]
```

Aunque se utiliza la misma sintaxis los usos de estos operadores son distintos, una manera adicional de poder identificarlos es:

```js
// SPREAD, because on RIGHT side of =
const arr = [1, 2, ...[3, 4]];

// REST, because on LEFT side of =
const [a, b, ...others] = [1, 2, 3, 4, 5];
console.log(a, b, others);
```

### Short Circuiting Operators

Estos operadores son `&&` y `||` que son los [operadores lógicos](#lógicos), sin embargo estos tienen este uso adicional con el cual se puede evaluar una condición y dependiendo de si la condición regresa un valor `true` o `false` regresan cierto fragmento del código, pudiendo así simplificar el uso de un operador ternario.

Ejemplo usando el operador `&&`

```js
console.log(0 && 'Luis'); // > 0
console.log(7 && 'Luis'); // > Luis
console.log('Hello' && 23 && null && 'Luis'); // > null
```

En este caso se utilizó el operador `AND`(`&&`), el cual retorna el primer valor `false` dentro de la evaluación., sin embargo para el segundo ejemplo al no contar con valores falsos se retorna el último valor verdadero. en este caso `Luis`

```js
// Ejemplo de la simplificación del operador ternario con &&
const restaurant = {
  orderPizza(mainIngredient, ...otherIngredients) {
    console.log(mainIngredient);
    console.log(otherIngredients);
  },
};

if (restaurant.orderPizza) {
  restaurant.orderPizza('mushrooms', 'spinach');
}
// > mushrooms
// > [ 'spinach' ]

restaurant.orderPizza && restaurant.orderPizza('mushrooms', 'spinach');
// > mushrooms
// > [ 'spinach' ]
```

Ejemplo usando el operador `||`

```js
console.log(3 || 'Luis'); // > 3
console.log('' || 'Luis'); // > Luis
console.log(true || 0); // > true
console.log(undefined || null); // > null
console.log(undefined || 0 || '' || 'Hello' || 23 || null); // > Hello
```

En este caso se utilizó el operador `OR`(`||`), el cual retorna el primer valor no falso (o verdadero) dentro de la evaluación, sin embargo para el cuarto ejemplo al no contar con valores verdaderos se retorna el último valor falso. en este caso `null`

```js
// Ejemplo de la simplificación del operador ternario con ||

const restaurant = {};
restaurant.numGuests = 0;

const guests1 = restaurant.numGuests ? restaurant.numGuests : 10;
console.log(guests1); // > 10

const guests2 = restaurant.numGuests || 10;
console.log(guests2); // > 10
```

Para este caso no es tan conveniente el uso de este operador, ni del ternario pues puede que `0` sea un valor y no se desee que se ejecute como `false`, pero para ello contamos con el [operador de fusión nulo](#the-nullish-coalescing-operator)

### The Nullish Coalescing Operator

Este operador soluciona el problema de recibir un `0` o un `''` como valor y convertirlo a un `false`, pues solo se obtiene un valor `false` al obtener un `null` o `undefined`.

```js
const restaurant = {};

restaurant.numGuests = 0;
const guests = restaurant.numGuests || 10;
console.log(guests); // > 10

const guestCorrect = restaurant.numGuests ?? 10;
console.log(guestCorrect); // > 0

const restaurantGreeting = restaurant.hello ?? 'Hello, I was undefined';
console.log(restaurantGreeting); // > Hello, I was undefined
```

Para este caso si existe el valor `restaurant.numGuests` que es el valor que esta a la izquierda del operador (`??`) la propiedad tomará ese valor, de ser nulo tomará lo que esta a la derecha del operador.

### Optional Chaining Operator

El operador de encadenamiento opcional `?.` permite acceder a una propiedad anidada dentro de un objeto en caso de que esta exista,evitando así crear validaciones para estas propiedades, por ejemplo:

```js
if (users.length > 0) console.log(users[0].name);
else console.log('user array empty');

const users = [{ name: 'Luis', email: 'hello@me.io' }];
console.log(users[0]?.name ?? 'User array empty');

if (restaurant.openingHours && restaurant.openingHours.mon)
  console.log(restaurant.openingHours.mon.open);

console.log(restaurant.openingHours.mon?.open);

// También se pueden concatenar varios operadores, los cuales se ejecutan de izquierda a derecha

console.log(restaurant.openingHours?.mon?.open);

console.log(restaurant.order?.(0, 1) ?? 'Method does not exist');
console.log(restaurant.orderRisotto?.(0, 1) ?? 'Method does not exist');
```

## Bucles

### for

El bucle `for` sirve para poder ejecutar una

### forEach

### map

## Funciones

Las funciones son bloques de código que realizan cierta acción especifica, se hace uso de ellas para evitar tener código duplicado, para acceder a ellas se usa la palabra reservada `function`.
Entre las partes de una función se encuentran:
Parámetros de entrada (argumento), el bloque de la función (su código) y la salida de la función (return)

```js
function cuadradoNumero(num) {
  const resultado = num * num;
  return resultado;
}
```

En este caso, la función recibe el nombre de `cuadradoNumero`, el parámetro que recibe es la variable `num` y retorna la variable `resultado`.

Al ser invocada la función esta se iguala a la variable valor, como parámetro se le manda la variable numero y para imprimir el resultado se hace uso de un `console.log` el cual incluirá la variable valor, con lo que se incluyen las propiedades de la función

Esto es posible ya que en JS se pueden utilizar también las [funciones a manera de expresión](#function-expression), esto se realiza igualando la función a una variable, a la hora de invocar a la variable a la que se igualo, esta variable será capaz de llevar a cabo la función, ya que posee sus propiedades.

También en JS puedes declarar argumentos por default, estos se accionarán en caso de invocar a una función y que la función no reciba los argumentos necesarios con los que fue declarada.

```js
const add = function (a = 5, b = 3, c = 8) {
  return a + b + c;
};
```

Al hacer uso de un parámetro `null`, la variable en el argumento al que esté relacionada no actuara dentro de la función.

Para hacer uso de los template string en JS se debe utilizar el símbolo `$` , con esto le indicamos a JS que a partir de ahí comienza una expresión (la expresión va entre llaves), y al hacer uso de estas expresiones se utilizan las tildes invertidas `\``.

```js
const name = Luis;
console.log(`Tu nombre es ${name}`);

const a = 4,
  b = 14;
console.log(`La suma de los números es: ${a + b}`);
```

Existen 3 diferentes maneras de declarar una función, estas son:

### Function declaration

Esta es la manera mas tradicional en la que se declaran las funciones, la ventaja de utilizar esta sintaxis es que la función puede ser utilizada antes de ser declarada, por ejemplo:

```js
console.log(cuadradoNumero(7)); // > 49

function cuadradoNumero(num) {
  const resultado = num * num;
  return resultado;
}
```

Esto es posible gracias al [hoisting](#hoisting), el cual recibe la declaración de la función la primera vez que escanea el archivo de JavaScript.

### Function expression

La declaración de este tipo de función es la del segundo ejemplo, en este caso se iguala la función a una variable:

```js
const add = function (a = 5, b = 3, c = 8) {
  return a + b + c;
};

console.log(add()); // > 16
```

En este caso el resultado de la función es 16, pues posee parámetros con valores por defecto, es importante mencionar que al asociarse esta función a una variable no es posible utilizarla antes de ser declarada.

### Arrow function

Las funciones de flecha son otro tipo de sintaxis para las funciones, en este caso al igual que en [function expression](#function-expression) se igualan a una variable, por ejemplo si transformamos la función pasada a una función de flecha queda de la siguiente manera:

```js
const add = (a = 5, b = 3, c = 8) => a + b + c;

console.log(add()); // > 16
```

Este tipo de funciones tampoco puede ser utilizada antes de declararse, entre sus diferencias con las [function expression](#function-expression) es que las funciones de flecha no poseen la palabra reservada [this](#this), y que se puede obviar el `return`, pues estas funciones se deshacen de la palabra reservada `function` en su sintaxis, y de manera opcional también de las llaves `{}` y la palabra `return`, retornando aquello que se encuentre después de la flecha `=>`, aunque también pueden ser declaradas con `{}` y `return`, por ejemplo:

```js
const add = (a = 5, b = 3, c = 8) => {
  return a + b + c;
};

console.log(add()); // > 16
```

## Estructuras de datos

Las estructuras de datos son la manera en la Javascript permite organizar información a lo largo de un programa, la estructura principal son los [objetos](#objetos), aunque de esta se desprenden algunas otras como los [arreglos](#arreglos), los [sets](#sets) y los [maps](#maps)

### Arreglos

Los arreglos (o matrices) también conocidos como listas ordenan elementos de manera secuencial en la memoría, esto quiere decir uno después del otro.
Generalmente estas listas contienen ciertos elementos relacionados entre sí, por ejemplo:

```js
const friends = ['Michael', 'Steven', 'Peter'];

const years = new Array(1991, 1984, 2008, 2020);
```

Adicionalmente a los arreglos se le pueden agregar o eliminar elementos con los siguientes métodos:

- `.push()`: agrega el elemento al final del arreglo.
- `.unshift()`: agrega el elemento al inicio del arreglo.
- `.pop()`: Elimina el último elemento del arreglo.
- `.shift()`: Elimina el primer elemento del arreglo.
- `.indexOf()`: Sirve para mostrar la posición en la que se encuentra el elemento.
- `.splice(1,2)`: Sirve para borrar contenido de un arreglo, el primer parámetro es para ubicar la posición y el segundo para indicar los elementos que se eliminarán a partir de esta posición.
- `.join()`: Convierte todos los elementos del array a un string de texto, estos elementos serán separados por comas.
- `.split()`: Convierte los textos separados por (“, ”) en un array.
- `.sort()`: Ordena por orden alfabético los elementos del arreglo.
- `.reverse()`: Nos permite ordenar de manera inversa el array.
- `.some()`: Retorna `true` en caso que un elemento del arreglo cumpla la condición que se envía en el parámetro.
- `.every()`: Retorna `true` en caso que todos los elementos del arreglo cumpla la condición.

### Objetos

Para poder declarar objetos en JS se puede hacer de dos maneras, las cuales son:

Haciendo uso de la manera literal.

```js
const persona = {
  name: 'Luis',
  lastname: 'Lara',
  hobbies: ['Listen music', 'See movies', 'Learn new things'],
};
```

O con la sintaxis propia del mismo.

```js
const persona2 = new Object();
persona2.name = 'Luis';
persona2.lastname = 'Lara';
```

Para la creación e implementación de métodos dentro de los objetos se puede hacer uso de la siguiente sintaxis:

```js
const persona = {
  name: 'Luis',
  lastname: 'Lara',
  hobbies: ['Listen music', 'See movies', 'Learn new things'],
  yearNacimiento: 1999,
  // Método:
  calcularEdad: function () {
    this.edad = 2021 - this.yearNacimiento;
  },
};
```

Entre algunas de las propiedades más importante de los objetos se encuentran las siguientes:

- `Object.keys(object)` Esta propiedad muestra los nombres de las propiedades (llaves) dentro de un objeto.
- `Object.values(object)` Esta propiedad muestra los valores ligados a las llaves dentro de un objeto.
- `Object.entries(object)` Esta propiedad muestra los valores y las propiedades dentro de un objeto.

Ejemplo:

```js
const persona = {
  name: 'Luis',
  lastname: 'Lara',
  hobbies: ['Listen music', 'See movies', 'Learn new things'],
};

console.log(Object.keys(persona));
// > [ 'name', 'lastname', 'hobbies' ]

console.log(Object.values(persona));
// > [ 'Luis', 'Lara', [ 'Listen music', 'See movies', 'Learn new things' ] ]

console.log(Object.entries(persona));
// > [ [ 'name', 'Luis' ], [ 'lastname', 'Lara' ], [ 'hobbies', [ 'Listen music', 'See movies', 'Learn new things' ] ] ]
```

### Arreglos 2

Ya conociendo los arreglos y los objetos es importante aclarar una cosa, y es que como se vio al inicio en los [tipos de datos](#tipos-de-datos) JavaScript cuenta con objetos y primitivos, por lo que todas las estructuras de datos son objetos (incluso los arreglos), por ende tienen las mismas reglas, con la diferencia de que los arreglos tienen métodos que los objetos por si mismos no.

De entre los métodos especiales con los que cuentan los arreglos el más importante es `.length`, el cual muestra la longitud de un arreglo **basándose en sus indices numéricos**, por ejemplo:

```js
y = [];
y[0] = true;
y.hello = 'goodbye';
y[10] = false;
y;
```

Esto devolverá en consola lo siguiente: `[true, empty × 9, false, hello: "goodbye"]`

Al usar la propiedad `.length` en el arreglo `y` nos devuelve `11`, ya que el arreglo cuenta con 11 indices numéricos (del 0 al 10).

### Dot vs Bracket Notation

La diferencia entre la notación de punto y de corchetes es porque al hacer uso de dot notation se espera acceder a una propiedad de tipo string.

En otras palabras usamos los corchetes cuando no podemos usar el punto, ya que el punto aplica coercion a las llaves de los objetos y los convierte en strings, por ejemplo

```js
let person = [];
person.name = 'Ángel';
person[lastname] = 'Cruz';

console.log(person);
```

El código anterior marcara error, pues no se puede declarar la línea 3 `person[lastname]='Cruz'`, para poder hacerlo de manera correcta tiene que ser a través de la notación con el punto o agregándole comillas a la propiedad `lastname`

```js
let person = [];
person.name = 'Ángel';
person['lastname'] = 'Cruz';

console.log(person);
```

Sin embargo el uso más importante que diferencia a ambas sintaxis es que la notación de corchetes puede no solo recibir una propiedad tipo string sino que dentro de estos corchetes puedes ejecutar una expresión de JavaScript en si, por ejemplo:

```js
const luis = {
  firstName: 'Luis',
  lastName: 'Cruz',
  hobbies: ['Listen music', 'See movies', 'Learn new things'],
};

const nameKey = 'Name';
console.log(luis['first' + nameKey]); // > Luis
console.log(luis['last' + nameKey]); // > Cruz
```

Esto es util ya que con la notación de corchetes puedes enviar una variable, con esto se buscará en el objeto la llave que sea igual al valor de la variable, por ejemplo:

```js
const interestedIn = prompt(
  'What do you want to know about Luis? Choose between firstName, lastName or hobbies'
);

if (luis[interestedIn]) {
  console.log(luis[interestedIn]);
} else {
  console.log('Wrong request! Choose between firstName, lastName, or hobbies');
}
```

En caso de que al `prompt` se ingrese `firstName` ese será el valor que tome la variable `interestedIn`, por lo que al ejecutarse la condicional if queda de la siguiente manera:

```js
if (luis['firstName']) {
  console.log(luis['firstName']); // > Luis
} else {
  console.log('Wrong request! Choose between firstName, lastName, or hobbies');
}
```

De esta manera se puede jugar con los valores computados de la variable `interestedIn`, haciendo más dinámico el código.

### Sets

Los sets son similares a los arreglos, pues también se trata de listas de elementos, con la diferencia de que los sets solo almacenan valores únicos, por lo cual son una excelente alternativa al método `.filter` o a la iteración de un arreglo en caso de querer crear una nueva lista con valores únicos (llave-valor dentro de un set es el mismo).

```js
const carrito = new Set();

carrito.add('Camisa');
carrito.add('Disco #1');
carrito.add('Disco #2');
carrito.add('Disco #3');
carrito.add('Camisa');

console.log(carrito); // > Set { 'Camisa', 'Disco #1', 'Disco #2', 'Disco #3' }
```

También puedes crear un set partiendo de un arreglo ya existente:

```js
let numeros = new Set([1, 2, 3, 4, 5, 6, 7, 3, 3, 3, 3]);
console.log(numeros.size); // > 7
```

Algunos de los métodos de los sets son:

```js
let carrito = new Set();
carrito.add('Camisa');
carrito.add('Disco #1');
carrito.add('Disco #2');
carrito.add('Disco #3');
carrito.add('Disco #3');
// El método add añade un elemento al set, y retorna el set después de agregar ese elemento
console.log(carrito4.add('Camisa')); // > Set { 'Disco #1', 'Disco #2', 'Disco #3', 'Camisa' }

// Comprobando que un valor existe en el set.
console.log(carrito.has('Camisa')); // > true

// Eliminando del set
console.log(carrito.delete('Camisa')); // > true
console.log(carrito.has('Camisa')); // > false
console.log(carrito); // > Set { 'Disco #1', 'Disco #2', 'Disco #3' }

// Limpiar un set
carrito.clear();
console.log(carrito); // > Set {  }

// Foreach a un set
carrito.forEach((producto) => {
  console.log(producto);
});

// Foreach a un set
carrito.forEach((producto, index, pertenece) => {
  console.log(`${index} : ${producto}`);
  console.log(pertenece === carrito); // nombre de la variable
});

// Convertir un set a un arreglo...
const arregloCarrito = [...carrito];
console.log(arregloCarrito);
```

#### WeakSet

Los WeakSets son similares a los sets, solo que estos funcionan únicamente con objetos, por lo que no se pueden agregar otros tipos de datos.

En este tipo de datos no existe `.size` ni son iterables

```js
var ws = new WeakSet();
const cliente = {
  nombre: 'Juan',
  saldo: 3000,
};

const cliente2 = {
  nombre: 'Pedro',
  saldo: 3000,
};

const nombre = 'Pedro';

ws.add(cliente);
ws.add(cliente2);

ws.add(nombre); // > Invalid value used in weak set
// Recordar que solo se pueden agregar objetos

console.log(ws.has(cliente)); // > true
console.log(ws.has(cliente2)); // > true
// console.log( ws.has(nombre)); // > false

// console.log( ws.delete(window)); // > window is not defined
console.log(ws.delete(cliente)); // > true
console.log(ws.has(cliente)); // > false
```

### Maps

Los maps en javascript son como los objetos, pues se trata de pares de valores (llave-valor) sin embargo a diferencia de los objetos en los maps es posible utilizar como llave cualquier tipo de dato primitivo sin limitarse simplemente a los strings.

En cuanto a performance los maps tienen mejor performance que los objetos, son especialmente diseñados para agregar o quitar elementos así como recorrer, también cuando son muy grandes tienen mejor performance que un objeto

```js
let cliente = new Map();

cliente.set('nombre', 'Karen');
cliente.set('tipo', 'Premium');
cliente.set('saldo', 3000);
cliente.set(1, 'Firenze, Italy');
console.log(cliente.set(2, 'Lisbon, Portugal'));

console.log(cliente); /* > Map { 'nombre' => 'Karen', 
  'tipo' => 'Premium', 
  'saldo' => 3000, 
  1 => 'Firenze, Italy', 
  2 => 'Lisbon, Portugal' } */

// acceder a los valores
console.log(cliente.get('nombre')); // > Karen
console.log(cliente.get(2)); // > Lisbon, Portugal
```

Métodos al MAP:

```js
// Tamaño del MAP
console.log(cliente.size); // > 5

// Contiene un valor
console.log(cliente.has('tipo')); // > true
console.log(cliente.get('tipo')); // > Premium

// Borrar
cliente.delete('nombre');
console.log(cliente.has('nombre')); // > false
console.log(cliente.get('nombre')); // > undefined
console.log(cliente.size); // > 4

// Borrar el map
cliente.clear();
console.log(cliente); // > Map {  }

// También se puede inicializar un map con diferentes valores...
const paciente = new Map([
  ['nombre', 'paciente'],
  ['cuarto', 'no definido'],
]);

paciente.set('nombre', 'Antonio');

console.log(paciente); // > Map { 'nombre' => 'Antonio', 'cuarto' => 'no definido' }

// Foreach a un map
paciente.forEach((datos, index) => {
  console.log(`${index}: ${datos}`);
  // > nombre: Antonio
  // > cuarto: no definido
});
```

Convertir un objeto a un Mapa:

```js
const newMap = new Map(Object.entries(myObject));
console.log(newMap);
```

Convertir un Mapa a un array:

```js
console.log([...question]);
// console.log(question.entries());
console.log([...question.keys()]);
console.log([...question.values()]);
```

#### WeakMap

Sirven para mantener los datos como privados, pues no puedes acceder a los valores con la propiedad `.get()`, tampoco son iterables ni se puede conocer la extensión con `.size`.

```js
let key = { userId: 1 };
let key2 = { userId: 2 };
let weakmap = new WeakMap();

weakmap.set(key, 'Alex');
weakmap.has(key); // > true
weakmap.get(key); // > Alex
weakmap.delete(key); // > true
weakmap.get(key); // > undefined

weakmap.set(key2, 'Vicky');
weakmap.size; // > undefined
key2 = undefined;
weakmap.get(key2); // > undefined
```

### Symbol

Los símbolos son nuevos en ES6, te permiten crear propiedad única, el `Symbol` es creado con la siguiente sintaxis `Symbol()` y se agrega a una propiedad del objeto, `new Symbol()` enviará un error.

```js
const sym = Symbol('symbol');
const sym2 = Symbol('symbol');

// Los símbolos siempre son diferentes

console.log(Symbol() === Symbol()); // > false

// Llaves de objetos únicas
let nombre = Symbol();
let apellido = Symbol();

// Crear un objeto vació
let persona = {};
// Esto no va a servir

persona.datos;

// debe tener corchetes
persona[nombre] = 'Luis';
persona[apellido] = 'Cruz';
persona.tipoCliente = 'Premium';
persona.saldo = 500;
console.log(persona); // > { tipoCliente: 'Premium', saldo: 500, [Symbol()]: 'Luis', [Symbol()]: 'Cruz' }
console.log(persona[nombre]); // > Luis
```

Los símbolos no son iterables.

```js
// No se puede acceder al SYMBOL con un for.
for (let i in persona) {
  console.log(`${i} : ${persona[i]}`);
}

// También se puede crear una descripción del symbol

let nombreCliente = Symbol('Nombre del cliente');
let cliente = {};

cliente[nombreCliente] = 'Pedro';

// Probar
console.log(cliente); // > { [Symbol(Nombre del cliente)]: 'Pedro' }
console.log(cliente[nombreCliente]); // > Pedro
console.log(nombreCliente); // > Symbol(Nombre del cliente)
```

### ¿Qué tipo de estructura de datos utilizar?

Para poder identificar cual es la estructura más optima se debe identificar de donde es que provienen los datos, los cuales tienen generalmente tres fuentes:

- El programa en si mismo: Datos escritos directamente en el código, por ejemplo notificaciones, alertas, mensajes predeterminados.
- De la Interfaz de Usuario: Datos ingresados por el usuario o datos escritos en el DOM, por ejemplo formularios.
- De fuentes externas: Datos traídos desde alguna fuente externa, por ejemplo consultas a APIs (Application Programming Interface).

Sea cual sea la fuente de los datos con ella deben generarse colecciones de datos, los cuales eventualmente se convierten en estructuras de datos.

En caso de que se necesite alguna lista de valores lo mejor es optar por [arrays](#arreglos) o por [sets](#sets), mientras que si se requiere tener pares de valores que almacenen información de una manera más descriptiva se opta por los [objetos](#objetos) o los [maps](#maps).

- [Arrays](#arreglos):

  - Se usan cuando se necesite una lista ordenada de items la cual puede contener items duplicados.
  - También se utiliza cuando se necesitan manipular los datos.

- [Sets](#sets):

  - Se utilizan cuando se requiere trabajar con valores únicos.
  - Cuando el alto rendimiento realmente es importante.
  - Para eliminar valores duplicados de los arreglos.

- [Objetos](#objetos):

  - Son fáciles de escribir y te permiten acceder a sus propiedades con las notaciones de `.` y de `[]`.
  - Se usan cuando se necesita incluir funciones (métodos).
  - Se usa cuando se trabaja con archivos JSON (aunque estos pueden transformarse a un Map)

- [Maps](#maps):
  - Tienen mejor rendimiento que los objetos.
  - Sus llaves pueden ser de cualquier tipo.
  - Son fáciles de iterar.
  - Se utilizan cuando la llave no necesariamente deba ser un string, o se requiera asociar una llave a un valor.

## This

La palabra this en javascript sirve para poder hacer referencia a un contexto dentro de la ejecución de la aplicación, es importante tomar en cuenta que **la palabra this no es estática**, esta depende de como es que se llama a la función que crea el contexto y como es que el valor de dicho contexto se asigna cuando la función es llamada.

Existen 4 casos principales en los cuales funciona la ejecución por defecto de la palabra `this`:

- Método: En este caso `this` hace referencía al objeto que instancia el método.
- Ejecución de función: En este caso `this` devolverá el valor `undefined` si es que el script se encuentra en `'strict mode'`, de lo contrario devolverá el objeto `window`, en caso que se ejecute desde el navegador.
- Arrow Functions: Como se mencionó anteriormente, las funciones de flecha no modifican el scope al que `this` hace referencia, por lo que este contexto es heredado, a esto se le llama `lexical this`.
- Event Listener: En este caso `this` hace referencia al elemento del DOM al que se encuentra relacionada la ejecución de la instancía.

```js
// Ejemplo en método
const me = {
  year: 1999,
  calcAge: function () {
    console.log(this); // > { year: 1999, calcAge: [λ: calcAge] }
  },
};
me.calcAge();

// Ejemplo en función (console.log, no strict mode, al ejecutarse en el navegador)
console.log(this); // > Window {window: Window, self: Window, document: document, name: "", location: Location, …}

// Ejemplo con un método usando arrow function (ejecutada en el navegador sin strict mode)
const me = {
  year: 1999,
  calcAge: () => {
    console.log(this); // > Window {window: Window, self: Window, document: document, name: "", location: Location, …}
  },
};
me.calcAge();
// En este caso al ser una arrow function y no poseer un contexto propio hereda el contexto de un nivel superior (en este caso el objeto window)
```

### call

La propiedad `.call()` sirve para hacer una referencia explicita a donde es que la palabra `this` apuntará, pues como primer parámetro recibe el objeto al que `this` apunta, seguido de los parámetros requeridos por la función en si misma.

```js
const lufthansa = {
  airline: 'Lufthansa',
  iataCode: 'LH',
  bookings: [],
  book(flightNum, name) {
    console.log(
      `${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`
    );
    this.bookings.push({ flight: `${this.iataCode}${flightNum}`, name });
  },
};

const book = lufthansa.book;

// book(23, 'Sarah Williams'); -> En este caso no funciona pues al ser una función el contexto del this devuelve un undefined.
book.call(lufthansa, 23, 'Sarah Williams'); // > Sarah Williams booked a seat on Lufthansa flight LH23
```

### apply

`.apply()` funciona de manera similar a `.call()`, con la diferencia de que recibe un arreglo como parámetro en vez de parámetros de manera individual.

```js
// Utilizando el objeto anterior
const swiss = {
  airline: 'Swiss Air Lines',
  iataCode: 'LX',
  bookings: [],
};

const flightData = [583, 'George Cooper'];
book.apply(swiss, flightData);
console.log(swiss);
// > { airline: 'Swiss Air Lines', iataCode: 'LX', bookings: [ { flight: 'LX583', name: 'George Cooper' } ] }

// El metodo call puede sustituir al metodo apply si se utiliza el spread operator
book.call(swiss, ...[583, 'Danna Cooper']);
console.log(swiss);
// > { airline: 'Swiss Air Lines', iataCode: 'LX', bookings: [ { flight: 'LX583', name: 'George Cooper' }, { flight: 'LX583', name: 'Danna Cooper' } ] }
```

### bind

La diferencia con las anteriores es que `.bind()` no llama inmediatamente a la función, si no que regresa una nueva función con la palabra `this` ya asociada a aquello que se pase como parámetro a la función.

Esto es llamado **partial application pattern**, esto quiere decir que parte de los argumentos de la función ya están definidos, por lo que se trabaja ya sin necesidad de ser requerido.

```js
// Ejemplo con bind
const lufthansa = {
  airline: 'Lufthansa',
  iataCode: 'LH',
  bookings: [],
  book(flightNum, name) {
    console.log(
      `${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`
    );
    this.bookings.push({ flight: `${this.iataCode}${flightNum}`, name });
  },
};

const book = lufthansa.book;

const eurowings = {
  airline: 'Eurowings',
  iataCode: 'EW',
  bookings: [],
};

const bookEW = book.bind(eurowings);

bookEW(23, 'Steven Williams');
// > Steven Williams booked a seat on Eurowings flight EW23

// Utilizando el partial pattern application (se define el primer parámetro de la función, en este caso el número del vuelo)
const bookEW23 = book.bind(eurowings, 23);
bookEW23('Ángel Cruz');
// > Ángel Cruz booked a seat on Eurowings flight EW23
```

Ademas de esto el método bind permite trabajar con Event Listeners, de esta manera se cambia el contexto por defecto (que sería el elemento del [DOM](#dom-document-object-model)) por el contexto del objeto.

```js
lufthansa.planes = 300;
lufthansa.buyPlane = function () {
  console.log(this);

  this.planes++;
  console.log(this.planes);
};

document
  .querySelector('.buy')
  .addEventListener('click', lufthansa.buyPlane.bind(lufthansa));
```

## DOM (Document Object Model)

El DOM es el modelo de objetos del documento, en caso de HTML es la estructura de árbol que se crea mientras se crea el sitio.
Esto sirve para poder analizar la estructura del archivo HTML.

```js
const caja = document.getElementById('mi-caja');

caja.innerHTML = 'Texto desde JS';
```

`.innerHTML` sirve para mostrar el contenido del `.getElementById(“mi-caja”)`, y después modificarlo por `'Texto desde JS'`
`.prepend()` inserta el elemento como el primer hijo, mientras que `.append()` lo hace como el ultimo.
`.before()` inserta el elemento como hermano mayor, mientras que `.after()` lo inserta como hermano menor.

Es importante mencionar que a la hora de trabajar con estilos se pueden crear desde javascript, estos son llamados estilos de linea:

```js

```

## Testing

Los test sirven para corroborar que las funcionalidades de la aplicación trabajen de manera correcta, inclusive funcionando como documentación para personas que no han trabajado en el proyecto desde el inicio.

No se recomienda realizar pruebas de toda la aplicación ya que consume una gran cantidad de tiempo, más bien se recomienda probar como se integran las diferentes funcionalidades.

Se dividen en las siguientes:

- **End to End**: Es la más interactiva de todas, simula algunos clicks, llenar formularios y asegurarse de que se muestre en pantalla lo que se desea.
- **Integración**: Revisa que múltiples partes de nuestro proyecto funcionen de manera correcta.
- **Unit**: Revisa qeu cada parte por si sola funcione bien.
- **Static**: Revisa por errores en el código mientras vas escribiendo (por ejemplo [typescript](typescript.md)).

### Jest

`describe()` sirve para poder agrupar las pruebas dentro de una misma función
`.toHaveLength()` sirve para revisar si cuenta con la misma longitud de caracteres.

Los snapshots son datos que se almacenan como un string, sobre estos datos se puede comparar después si la data es la misma o ha sufrido alguna mutación

# Tipos de datos

- number
- string
- boolean
- null
- undefined
- object
- function

- Cadenas de texto
- Objetos
- DOM
- Asincronismo

# Functional JS

Es crear el código utilizando funciones, es importante destacar que hay ciertas reglas:

- Las funciones deben tomar una entrada y tener una salida de datos
- No se permite la modificación o mutación de los datos
- Se separan las funciones de los datos (pueden usarse también los array methods), de esta manera se cuenta con una función que entrega un resultado nuevo pero nunca modifica los datos
- First class functions: Es poder crear funciones que parezcan cualquier variable (como las function expressions)

```js
const suma = function(a + b){
  return a + b
}

const resultado = suma
```

Los Closures permiten hacer disponible un valor interno en el exterior en cuando a scopes se refiere

```js
function ask(question) {
  return function holdYourQuestion() {
    console.log(question);
  };
}

let myQuestion = ask('What is closure?');

myQuestion(); // What is closure?
```

# This

This apunta a distintos lugares de la aplicación dependiendo del contexto. Cuando se utiliza `this` dentro de un método la referencia apunta al objeto, cuando se utiliza dentro de una función la referencia apunta a window o a global.

# Hoisting

Es un termino para referirse al contexto de ejecución de javascript, la primera vez se leen y registran las funciones y en la segunda se manda a llamar

# Coercion

Es la conversion automática o implícita de un tipo de valor dado a otro (la coerción implícita se forza a que javascript lo maneje, lo haga y lo modifique ejemplo la suma de numero con string ), la explicita requiere utilizar una función (`Number('40')` o `JSON.Stringify([1,2,3])`)

# Event Loop

Es como se va ejecutando el código en javascript, que tiene mas prioridad, una función, una constante, etc. Al ser se un solo hilo se ejecuta una tarea a la vez, donde van los clg, promise, setTimeout, esto es por lo de la cola de ejecución

# Self

Es similar a la ventana global window, se utiliza mucho en los service workers

# Design patterns

Son soluciones típicas a problemas comunes en el desarrollo de software, cada patron es como un plano o herramienta la cual se utiliza para resolver un problema de diseño en el código.

Beneficios:

- Son soluciones a problemas de diseño de código
- Son soluciones probadas
- Son soluciones conocidas por todos y evitan la forma de escribir código como cada quien entiende

Existen distintas categorías de patrones:

De creación: Permite crear objetos y permiten la reutilización del código

De estructura: Explican como deben comunicarse los objetos y classes en grandes proyectos

De comportamiento: Como se comportan y comunican diferentes objetos

## Class pattern

Crear un objeto a partir de la instancia de una clase

## Constructor pattern

Se utiliza una clase base para que las demás clases hereden sobre ella (clases abstractas, que no se pueden instanciar)

## Singleton

Evita la creación de multiples instancias de una misma clase, en cambio siempre retornara el objeto instanciado

## Factory

Es una forma de crear objetos basados en cierta condición

## Module

El module pattern es un patron de organización de código, este llego con el soporte de módulos, creando funciones y módulos de manera independiente

## Mixin Pattern

Es una forma de agregar funciones a una clase una vez que ha sido creada

## Namespace

Es un patron de diseño para la organización de código, ayuda a evitar colisiones entre nombres en el scope global de js, siempre inicia como un objeto sobre el cual se agregaran los datos y funciones.

## Mediator

Es un patron de diseño que se comunica con diferentes objetos a la vez, el mediador tiene objetos localizados para objetivos específicos
