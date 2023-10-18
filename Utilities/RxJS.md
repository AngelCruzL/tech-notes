---
tags: utilities, frontend, javascript
---

---

[RxJS - Introduction](https://rxjs.dev/guide/overview)

---

# ¿Qué es RxJS?

RxJS es una librería de programación reactiva cuyo fin es simplificar la composición de código asíncrono y basado en eventos a través de secuencias observables.

RxJS provee una estructura de datos llamada **Observable**, estructuras derivadas como **Observer**, **Scheduler**, **Subject** y **Operators** para manipular estas estructuras, inspirados en los métodos que podemos encontrar en Array.prototype.

En pocas palabras las extensiones reactivas nos permiten disponer de la data de una aplicación web en tiempo real.

# Cuando usar extensiones reactivas?

- Eventos en la interfaz del usuario
- Cuando es necesario notificar sobre cambios en un objeto(s)
- Comunicación por sockets
- Cuando se trabaja con flujos (streams) de información

La programación reactiva cuenta con tres aspectos fundamentales, los observables, los subscribers y los operators

# ¿Cómo funciona RxJS y la programación reactiva?

La programación reactiva cuenta con tres aspectos fundamentales, los **Observables**, los **Subscribers** y los **Operators**

## Observables

- Son la fuente de información
- Pueden emitir multiples valores, solo uno o ninguno
- Pueden emitir errores
- Pueden ser infinitos, o finitos en caso de llegar a completarse
- Pueden ser síncronos o asíncronos

## Subscribers

- Se suscriben a un observable (están pendientes de todo lo que realiza el observable)
- Consumen/observan la data del observable
- Pueden recibir los errores y eventos del observable
- Desconocen lo que se encuentra detrás del observable (es decir como se maneja la data y si es que esta viene filtrada o no, etc)

## Operators

- Son utilizados para transformar los observables (como por ejemplo map, group, scan)
- Son utilizados para filtrar observables (filter, distinct, skip, debounce)
- Se utilizan para combinar observables
- Crear nuevos observables

# ReactiveX

Entre los beneficios de las extensiones reactivas se encuentran:

- Evitar el "Callback Hell"
- Trabajar de forma simple tareas sincronas y asincronas
- Uso de operadores para reducir y simplificar el trabajo
- Es fácil transformar los flujos (streams) de información
- Código mas limpio de mantener y mas fácil de leer
- Fácil de interpretar
- Fácil de anexar procedimientos sin alterar el producto final

ReactiveX es la combinación de las mejores ideas de el patron Observer, el patron Iterator y de la programación funcional

RxJS combina patrones de diseño de software como el Patrón del Observador, el Patrón de Iterador y conceptos de programación funcional, utilizando colecciones para modelar una forma ideal de manejar secuencias de eventos.

## Observer pattern

Es un patron de diseño de software que define una dependencia del tipo uno a muchos entre objetos, de manera que cuando uno de los objetos cambia su estado, notifica este cambio a todos los dependientes.

## Iterator Pattern

En la POO el patron iterador define una interfaz que declara los métodos necesarios para acceder secuencialmente a un grupo de objetos de una colección

## Programación funcional

Es crear un conjunto de funciones que tengan un objetivo especifico, es decir que siempre que se llamen a esas funciones realizaran una única función ya definida, sin efectos secundarios y sin mutar la data.

En resumen:

- El patron Observer notifica cuando suceden cambios en relación uno a muchos
- El patron Iterador Ejecuta las operaciones secuenciales
- Programación funcional: Tener funciones con tareas especificas que reciban argumentos y no muten la información

Como estándar y una buena práctica a la hora de definir un observable se utiliza el símbolo `$` al final de la declaración del mismo para indicar que es un observable.
Por ejemplo: `observable$`

![[observer-values.png]]

![[observer-values-2.png]]

## Observer y subscriber

Para que un observable se ejecute debe tener una suscripción

El método `.next()` emitirá el valor que yo quiero a las personas suscritas al observer

Al usar el método `.complete()` se finalizara la suscripción al observable, por lo que ninguna emisión de data posterior sera notificada

Existen tres posibles argumentos que se le pueden mandar a un subscribe

- next (recibe un valor)
- error (recibe un error)
- complete (no recibe argumentos)

```js
observable$.subscribe(
  valor => console.log('Next:', valor),
  error => console.warn('Error:', error),
  () => console.log('Completado')
);
```

Adicionalmente a esta, hay otra manera para poder mandarle estos argumentos al subscriber, haciendo uso de una propiedad llamada observer

```js
const observer: Observer<any> = {
  next: value => console.log('siguiente [next]:', value),
  error: error => console.warn('error [obs]:', error),
  complete: () => console.info('completado [obs]')
};

observable$.subscribe(observer);
```

## Subscription y unsubscribe

Al suscribirse a un observable se crea una nueva instancia del observable ejecutando todo el código de este.

El return de un observable puede regresar una función, dentro de dicha función es posible ejecutar las tareas que se deseen para cuando se realice el `.unsubscribe()` del observable.

El unsubscribe y complete son diferentes, pues al llamar el `.complete()` inmediatamente se ejecuta el return del observable.

También se pueden encadenar suscripciones a la suscripción original, para poder ejecutar el return de diferentes observables llamando solo a uno, eso se consigue con el método `.add()` , por ejemplo:

```js
const subs1 = intervalo$.subscribe(observer);
const subs2 = intervalo$.subscribe(observer);
const subs3 = intervalo$.subscribe(observer);

subs1.add(subs2).add(subs3);

subs1.unsubscribe();
```

Esto llama el return de los tres observables pero solo emite el `.complete()` del primero.

## Subject

El subject es una especie de observer y observable, ya que contiene características de ambos, como observer este puede suscribirse a uno o mas observables y como observable este puede realizar el casteo multiple.

En este el subject tendrá suscritos varios observables a el y se encargara de distribuir la misma información entre todas estas.

Adicionalmente el subject es capaz de emitir nuevos eventos.

Cuando la data es producida por el observable en si mismo es considerado un **cold observable**, pero cuando la data es producida fuera del observable es llamado **hot observable**.

El subject nos permite transformar un cold observable en un cold observable.

# Funciones para crear observables

Existen distintas funciones para crear observables, de esta manera se evita gran parte del trabajo visto anteriormente, pues RxJS ya trae cubierta esa parte, los aquí mostrados son los más comunes, en caso de querer seguir investigando más se pueden encontrar en [la siguiente documentación](https://rxjs-dev.firebaseapp.com/api/index)

## of

Es una función que nos permite crear observables en base a un listado de elementos, pues este observable convierte los argumentos que recibe en una secuencia de valores.

Este operador emite los valores en secuencia de manera síncrona, tras emitir el ultimo valor o elemento del listado el observable se termina de manera automática.

Por ejemplo:

```js
import { of } from 'rxjs';

const obs$ = of(1, 2, 3, 4, 5, 6);
obs$.subscribe(console.log);
```

Otra manera alternatva al mismo ejercicio es:

```js
import { of } from 'rxjs';

const obs$ = of(...[1, 2, 3, 4, 5, 6]);
obs$.subscribe(console.log);
```

## from

Crea un observable de un array, por ejemplo usando el segundo caso del of con el from queda de la siguiente manera

```js
import { from } from 'rxjs';

const obs$ = from([1, 2, 3, 4, 5, 6]);
obs$.subscribe(console.log);
```

Esto nos genera el mismo resultado que al usar el Spread Operator previamente al array mandado como argumento a la función of.

## fromEvent

Permite crear observables en base a un event target (es decir de cierto tipo que provienen del event target).

Por ejemplo:

```js
fromEvent < Event > (document, 'scroll');
```

Aquí se emitirán eventos cada vez que se realice el scroll, pues es el evento que se esta escuchando.

## range

Emite una secuencia de números en base a un rango especifico, esta función recibe como parámetros

```js
range(start: number = 0, count?: number, scheduler? SchedulerLike): Observable<number>
```

Por ejemplo

```js
const src$ = range(1, 5, asyncScheduler);

src$.subscribe(console.log);
```

## interval

Crea un observable que emite una secuencia de números en un intervalo de tiempo especificado

```js
import { interval } from 'rxjs';

const observer = {
  next: (val: any) => console.log('next:', val),
  complete: () => console.log('complete')
};

const interval$ = interval(1000);

interval$.subscribe(observer);
```

Es importante destacar que aquí nunca se emite el complete, pues nunca se completa el observable.

## timer

Crea un observable que empieza a emitir valores después de cierto tiempo, una vez emitido el valor el observable se completa pero este continua emitiendo valores cada cierto tiempo indicado.

```js
import { timer } from 'rxjs';

const observer = {
  next: (val: any) => console.log('next:', val),
  complete: () => console.log('complete')
};

const hoyEn5 = new Date();
hoyEn5.setSeconds(hoyEn5.getSeconds() + 5);

const timer$ = timer(hoyEn5);

timer$.subscribe(observer);
```

De esta manera se puede programar en que momento se desea la ejecución del valor en base al timer y su respectivo metodo complete del observable.

## asyncScheduler

Este no crea un observable sino una subscription, esta function puede tener la funcionalidad de un setTimeout o de un setInterval pero con bondades adicionales como la manipulación de la información como si se tratase de cualquier suscripción de rxjs.
Por ejemplo:

```js{7}
import { asyncScheduler } from "rxjs";

const subs = asyncScheduler.schedule(
  function(state) {
    console.log("state", state);

    this.schedule(state + 1, 1000);
  },
  3000,
  0
);

asyncScheduler.schedule(() => subs.unsubscribe(), 6000);
```

En este caso el `asyncScheduler` funciona como setInterval gracias a la línea que se encuentra resaltada, pues a esta se le manda la instruccion de que vuelva a ejecutar el metodo `schedule()` cada segundo.
Adicionalmente a esto se le agrega la desuscripción del intervalo a los 6 segundos (3 segundos después de haber iniciado el intervalo).

# Operators

### map

Este operador transforma la data

![[map-operator.png]]

### pluck

Este operador sirve para extraer un valor del objeto que estamos recibiendo y que este sea la nueva salida del observable

![[pluck-operator.png]]

### mapTo

Transforma la entrada en una salida especifica (pueden ser numeros u objetos)

![[mapTo-operator.png]]

### filter

Filtra los datos

![[filter-operator.png]]

### tap

Este operador nos ayuda a ver como fluye la información a través de nuestros observables

![[tap-operator.png]]

Este operador es útil para depurar y mostrar los errores

### reduce

No se realiza ninguna emisión hasta que se complete el observable

![[reduce-operator.png]]

### Scan

![[scan-operator.png]]

### take

Limita la cantidad de emisiones que un observable puede tener

![[take-operator.png]]

### first

![[first-operator.png]]

### takeWhile

### takeUntil

Recibe como argumento otro observable

![[takeUntil-operator.png]]

### distinct

Permite pasar los valores que no han sido emitidos previamente por el observable

### debounceTime

Nos permite contar cuantas milésimas de segundo han pasado desde la ultima emisión de nuestro observable, en caso de que estas milésimas de segundo separen las milésimas de segundo que se encuentran en los paréntesis se emitirá el siguiente valor

![[debounceTime-operator.png]]

### throttleTime

Similar al debounceTime este emite valores cada cierto tiempo

![[throttleTime-operator.png]]

Se pueden usar para controlar valores de observables que emiten demasiados valores en un periodo muy corto de tiempo

### sampleTime

Nos permite obtener el ultimo valor emitido en un intervalo de tiempo

![[sampleTime-operator.png]]

### sample

Emite el ultimo valor emitido por el observable hasta que el otro observable que tenemos dentro del operador sample emita un valor

![[sample-operator.png]]

### auditTime

Emite el ultimo valor que ha sido emitido por el observable en un periodo de tiempo determinado

![[auditTime-operator.png]]

### catchError

Sirve para obtener errores en observables (comúnmente es utilizado para las peticiones http), este catch error debe retornar un observable

![[catchError-operator.png]]

### ajax

Se pueden mandar parametros por un objeto como segundo argumento, siendo el primero la url a la cual se le realiza la peticion

# Operadores de transformacion

### mergeAll

Sirve para trabajar con observables que internamente llevan observables,

![[mergeAll-operator.png]]

Este observable se completa hasta haber completado todas las suscripciones del observable source, a esto se le conoce como _Flattering Operator_ u operador de aplanamiento

### mergeMap

Este operador no tiene limite de suscripciones internas y todas pueden estar activas simultáneamente

![[mergeMap-operator.png]]

Este operador se suscribe a cuantos observers emita o reciba

### switchMap

Recibe un callback que retorna un observable, es similar al mergeMap salvo que en este ultimo se pueden mantener multiples observables con suscripciones, mientras que en el switchMap solo puedes estar suscrito a un solo observable interno

Tambien al ser llamado el switchMap este va a cancelar la peticion anterior, esto es util al realizar peticiones ajax, ya que cancelara todas salvo la ultima ahorrando transferencia de datos o procesamiento adicional.

![[switchMap-operator.png]]

### concatMap

Sirve para concatenar los observadores resultantes que pueden fluir a traves del operador

![[concatMap-operator.png]]

### exhaustMap

Es util cuando tenemos muchos objetos o eventos que estan spameando mucha informacion(se emiten muchos valores que se pueden ignorar)

![[exhaustMap-operator.png]]

# Operadores de combinación

### startWith

Permite realizar una emisión antes de que un observable comience a emitir un valor síncrono.

![[startWith-operator.png]]

### endWith

Antes de que se complete el observable se emite un ultimo valor

![[endWith-operator.png]]

## concat (function)

El operador esta obsoleto, pero la función concat recibe observables o iterables (arrays) como argumento, con estos argumentos se crea un nuevo observable

![[concat.png]]

### merge

Es una función que recibe observables, cuyo resultado va a ser el resultado de estos observables combinados simultáneamente

![[merge.png]]

### combineLatest

Esta funcion nos permite mandar observables o argumentos, combinarlos y emitir los valores de todos los observables internos simultaneamente, esta funcion regresa un observable, el cual va a emitir valores hasta que todos los observables internos hayan emitido por lo menos un valor

![[combineLatest.png]]

### forkJoin

Los observables pasados como parametros deben de ser finitos

![[forkJoin.png]]
