---
tags: framework
---
----

[Documentación]()

----

# ¿Qué es SASS?




# Funcionalidades de SASS


## Extends

Te permite extender selectores, aplicando reglas ya establecidas en un selector dentro de otro sin tener que reescribirlas. Para ello es necesario hacer uso de la notación `@extends` seguido del nombre del selector que se quiere extender.

```scss
@extends .example
```

Ejemplo:
```scss
.bg-dark {
  background: black;
  padding: 1rem;
}

.inverted-colors {
  @extend .bg-dark;
  color: white;
}

.call-to-action {
  @extend .bg-dark;
  color: yellow;
  text-transform: uppercase;
  font-weight: bold;
}
```

Esto se convierte en lo siguiente:
```css
.bg-dark,
.call-to-action,
.inverted-colors {
  background: black;
  padding: 1rem;
}

.inverted-colors {
  color: white;
}

.call-to-action {
  color: yellow;
  text-transform: uppercase;
  font-weight: bold;
}
```


### Placeholders

Además de la sintaxis anterior existe otra la cual agrupa un conjunto de reglas css dentro de un "selector" el cual al momento de ser convertido a css no existe, para definir este selector placeholder se utiliza un `%` antes del nombre del mismo, por ejemplo:
```scss
$clr-primary: dodgerblue;
$clr-secondary: firebrick;
$clr-accent: limegreen;

%uppercase-bold-text {
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 1px;
}

.accent-text {
  color: $clr-accent;
  @extend %uppercase-bold-text;
}

.eyebrow {
  color: $clr-primary;
  font-size: 0.8625rem;
  @extend %uppercase-bold-text;
}
```

Esto genera el siguiente css

```css
.eyebrow, .accent-text {
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 1px;
}

.accent-text {
  color: limegreen;
}

.eyebrow {
  color: dodgerblue;
  font-size: 0.8625rem;
}
```

## Mixins

Dentro de SASS existen distintas utilidades para css, pues este framework de css permite añadirle funciones a css, entre ellas están los mixins, los cuales son funciones para implementar a la hora de escribir el código css

```css
@mixin style-link($color){
  ...
  color: $color;
}

@include style-link
```

```css
@mixin clearfix {
  ...;
}

@include clearfix;
```


### Mixins vs Extends

As we can see, extends will combine common properties into a single CSS rule with multiple selectors, whereas mixins will repeat the code each time we use it.

Some people will see this as a big performance gain for extends, but as is stated in the Sass documentation (emphasis mine):

> Most web servers compress the CSS they serve using an algorithm that’s very good at handling repeated chunks of identical text. **This means that, although mixins may produce more CSS than extends, they probably won’t substantially increase the amount your users need to download**. So choose the feature that makes the most sense for your use-case, not the one that generates the least CSS!

This relates back to the optimizations servers will make on the CSS files they deliver to clients, which we saw an example of a little while back.

On top of this, mixins can do a lot more than extends can.

It’s very rare that I have a mixin like in the previous lesson. We can nest selectors inside them, add arguments, include error checking, and much more.

Some of that we’ll get to soon, other parts will come a little later in the course.


### Selectores dentro de mixins

Ejemplo:
```scss
@mixin small-uppercase-span {
  span {
    font-size: 0.875em;
    text-transform: uppercase;
  }
}

.page-title {
  font-size: 4rem;
  line-height: 1.1;
  @include small-uppercase-span;
}
```

```css
.page-title {
  font-size: 4rem;
  line-height: 1.1;
}
.page-title span {
  font-size: 0.875em;
  text-transform: uppercase;
}
```

Pero no se limitan únicamente a esto, también pueden hacer lo siguiente:
```scss
@mixin inverted-colors {
  color: white;
  background: black;

  a {
    color: yellow;
  }
}

.call-to-action {
  @include inverted-colors;
}
```

```css
.call-to-action {
  color: white;
  background: black;
}
.call-to-action a {
  color: yellow;
}
```


### Mixins con argumentos

Nosotros podemos darles valores por defecto a los argumentos:
```scss
@mixin linear-gradient($deg: -45deg, $clr-1: red, $clr-2: blue) {
  background-image: linear-gradient($deg, $clr-1, $clr-2);
}

.hero {
  padding: max(10vh, 5rem);
  @include linear-gradient;
}
```

```css
.hero {
  padding: max(10vh, 5rem);
  background-image: linear-gradient(-45deg, red, blue);
}
```

También puedes sobre-escribir el valor de únicamente un argumento de la siguiente manera:

```scss
@mixin linear-gradient($deg: -45deg, $clr-1: red, $clr-2: blue) {
  background-image: linear-gradient($deg, $clr-1, $clr-2);
}

.hero {
  padding: max(10vh, 5rem);
  @include linear-gradient;
}

.call-to-action {
  @include linear-gradient($clr-2: yellow);
}
```

```css
.hero {
  padding: max(10vh, 5rem);
  background-image: linear-gradient(-45deg, red, blue);
}

.call-to-action {
  background-image: linear-gradient(-45deg, red, yellow);
}
```

Al igual que las funciones en programación, en caso de no proveer argumentos que sean obligatorios los mixins tomaran los valores por defecto que estos tenga, pero estos deben siempre ser los últimos, dejando al inicio aquellos que sean requeridos
```scss
@mixin linear-gradient($clr-1, $clr-2, $deg: -45deg) {
  background-image: linear-gradient($deg, $clr-1, $clr-2);
}

.hero {
  @include linear-gradient(red, blue);
}

.call-to-action {
  @include linear-gradient(blue, yellow, 10deg);
}
```

```css
.hero {
  background-image: linear-gradient(-45deg, red, blue);
}

.call-to-action {
  background-image: linear-gradient(10deg, blue, yellow);
}
```


## Interpolación

La interpolación permite insertar un valor de una propiedad dentro de otra. Ejemplo:
```scss
$width: 100;

.width-#{$width * 2} {
  width: $width * 2;
}
```

```css
.width-200 {
  width: 200;
}
```


## Ejemplos

```scss
$red: red;
$green: green;
$blue: blue;
$width: 100px;

.box {
  &-#{$red}-100{
	background: $red;
	width: $width;
  	aspect-ratio: 1 / 1;
  }

  &-#{$red}-150{
	background: $red;
	width: $width*1.5;
  	aspect-ratio: 1 / 1;
  }

  &-#{$red}-200{
	background: $red;
	width: $width*2;
	aspect-ratio: 1 / 1;
  }
}
```

```css
.box-red-100 {
  background: red;
  width: 100px;
  aspect-ratio: 1/1;
}
.box-green-150 {
  background: green;
  width: 150px;
  aspect-ratio: 1/1;
}
.box-blue-200 {
  background: blue;
  width: 200px;
  aspect-ratio: 1/1;
}
```

Para obtener el mismo resultado podemos utilizar mixins de la siguiente manera:

```scss
$red: red;
$green: green;
$blue: blue;
$width: 100;

/* all your code below here */

@mixin box($color, $multiplier: 1) {
  .box-#{$color}-#{$multiplier*$width} {
	background: $color;
		width: $multiplier*2;
  	aspect-ratio: 1 / 1;
}

}

@include box($red);
@include box($green, 1.5);
@include box($blue, 2);
```


## Anidamiento y referencia al padre (&)

```scss
.hover-me {
  $parent: &;

  background: black;
  color: white;

  &__title {
    color: limegreen;
  }

  &:hover,
  &:focus {
    background: white;
    color: black;

    #{$parent}__title {
      color: black;
    }
  }
}
```

Lo que se convierte en lo siguiente:
```css
.hover-me {
  background: black;
  color: white;
}

.hover-me__title {
  color: limegreen;
}

.hover-me:hover,
.hover-me:focus {
  background: white;
  color: black;
}

.hover-me:hover .hover-me__title,
.hover-me:focus .hover-me__title {
  color: black;
}
```


# Loops

## @for

```scss
@for $i from 1 to 3 {
  .example {
    width: $i;
  }
}
```

To:
```css
.example {
  width: 1;
}

.example {
  width: 2;
}
```



```scss
@for $i from 1 through 3 {
  .example {
    width: $i;
  }
}
```

To:
```css
.example {
  width: 1;
}

.example {
  width: 2;
}

.example {
  width: 3;
}
```


### Creando un sistema de n columnas

Utilizando el loop @for crearemos un sistema basado en 12 columnas, sin embargo, esto aplica para n columnas, basta con cambiar el valor de la variable `$total-columns`
```scss
$total-columns: 12;

@for $column-count from 1 through $total-columns {
  .col-#{$column-count} {
    flex-basis: (calc(100% / $total-columns)) * $column-count;
  }
}
```

Esto nos da como resultado al compilar a CSS lo siguiente:
```css
.col-1 {
  flex-basis: 8.3333333333%;
}

.col-2 {
  flex-basis: 16.6666666667%;
}

.col-3 {
  flex-basis: 25%;
}

.col-4 {
  flex-basis: 33.3333333333%;
}

.col-5 {
  flex-basis: 41.6666666667%;
}

.col-6 {
  flex-basis: 50%;
}

.col-7 {
  flex-basis: 58.3333333333%;
}

.col-8 {
  flex-basis: 66.6666666667%;
}

.col-9 {
  flex-basis: 75%;
}

.col-10 {
  flex-basis: 83.3333333333%;
}

.col-11 {
  flex-basis: 91.6666666667%;
}

.col-12 {
  flex-basis: 100%;
}
```


### Creando variaciones de un color

```scss
@use 'sass:color';

$clr-primary: #2553db;
$clr-steps: 10;

@for $i from 0 to $clr-steps {
	.clr-primary-#{i + 1}{
		color: color.scale($clr-primary, $lightness: $i * 10%)
	}
}
```

Esto nos da como resultado lo siguiente:
```css
.clr-primary-i1 {
  color: #2553db;
}

.clr-primary-i1 {
  color: #3b64df;
}

.clr-primary-i1 {
  color: #5175e2;
}

.clr-primary-i1 {
  color: #6687e6;
}

.clr-primary-i1 {
  color: #7c98e9;
}

.clr-primary-i1 {
  color: #92a9ed;
}

.clr-primary-i1 {
  color: #a8baf1;
}

.clr-primary-i1 {
  color: #becbf4;
}

.clr-primary-i1 {
  color: #d3ddf8;
}

.clr-primary-i1 {
  color: #e9eefb;
}
```


## @each

```scss
$font-colors: (primary, #ff0000), (secondary, #00ff00), (tertiary, #0000ff);

@each $name, $color in $font-colors {
  .#{$name} {
    color: $color;
  }
}
```

```css
.primary {
  color: #ff0000;
}

.secondary {
  color: #00ff00;
}

.tertiary {
  color: #0000ff;
}
```


### Creando una clase base y modificadores para cada icono

```scss
$icons: mail, twitter, github;

.icon {
  width: 1.5rem;
  aspect-ratio: 1;
  background-size: contain;

  @each $icon in $icons {
    &--#{$icon}{
      background-image: url("/images/#{$icon}.svg");
  	}
  }
}
```

Lo que genera el siguiente código:
```css
.icon {
  width: 1.5rem;
  aspect-ratio: 1;
  background-size: contain;
}
.icon--mail {
  background-image: url("/images/mail.svg");
}
.icon--twitter {
  background-image: url("/images/twitter.svg");
}
.icon--github {
  background-image: url("/images/github.svg");
}
```


## @if - @else,

```scss
@mixin border($side: all, $color: #dedede, $width: 1px, $style: solid) {
  @if $side == 'all' {
    border: $color $width $style;
  } @else {
    border-#{$side}: $color $width $style;
  }
}

.tag {
  @include border;
}

.call-to-action {
  @include border(bottom);
}
```

```css
.tag {
  border: #dedede 1px solid;
}

.call-to-action {
  border-bottom: #dedede 1px solid;
}
```


## @else if

```scss
@mixin uppercase($font-size: 1rem, $text-transform: uppercase) {
  text-transform: $text-transform;
  @if ($font-size < 2rem) {
    line-height: 1.3;
  } @else if ($font-size >= 2rem and $font-size < 3rem) {
    line-height: 1.2;
  } @else if ($font-size >= 3rem and $font-size < 5rem) {
    line-height: 1.1;
  } @else {
    line-height: 1;
  }
}

h1 {
  @include uppercase(6rem);
}
h2 {
  @include uppercase(4rem);
}
h3 {
  @include uppercase(2.5rem);
}
.uppercase {
  @include uppercase(1rem);
}
```

```css
h1 {
  text-transform: uppercase;
  line-height: 1;
}

h2 {
  text-transform: uppercase;
  line-height: 1.1;
}

h3 {
  text-transform: uppercase;
  line-height: 1.2;
}

.uppercase {
  text-transform: uppercase;
  line-height: 1.3;
}
```


## if()

Utilizando la sintaxis de if else
```scss
@use "sass:color";

$colors:
  (primary, #4287f5),
  (secondary, #3a128a),
  (accent, #f59c42);

@each $name, $color in $colors {
  .clr-#{$name} {
    background: $color;

    @if(color.lightness($color) < 50) {
      color: white;
    } @else {
      color: black;
    }
  }
}
```

Utilizando la sintaxis de `if()`:
```scss
@use "sass:color";

$colors:
  (primary, #4287f5),
  (secondary, #3a128a),
  (accent, #f59c42);

@each $name, $color in $colors {
  .clr-#{$name} {
    background: $color;
    color: if(color.lightness($color) < 50, white, black);
  }
}
```

Con ambas sintaxis obtenemos lo siguiente:
```css
.clr-primary {
  background: #4287f5;
  color: black;
}

.clr-secondary {
  background: #3a128a;
  color: white;
}

.clr-accent {
  background: #f59c42;
  color: black;
}
```


## Creando un mixin para un componente ui

```scss
@mixin ui-component($size, $color, $bg, $hover-color: $color, $hover-bg: $bg) {
  display: inline-block;
  padding: $size ($size * 3);
  color: $color;
  background-color: $bg;
  @if $hover-color != $color or $hover-bg != $bg {
    &:hover,
    &:focus {
      color: $hover-color;
      background-color: $hover-bg;
    }
  }
}

.button {
  @include ui-component(1em, white, black, black, white);
}

.badge {
  @include ui-component(0.25em, red, blue);
}
```

```css
.button {
  display: inline-block;
  padding: 1em 3em;
  color: white;
  background-color: black;
}
.button:hover, .button:focus {
  color: black;
  background-color: white;
}

.badge {
  display: inline-block;
  padding: 0.25em 0.75em;
  color: red;
  background-color: blue;
}
```


# Funciones

```scss
@function add($numbers...) {
  $total: 0;
  @each $number in $numbers {
    $total: $total + $number;
  }
  @return $total;
}

body {
  width: add(100px, 200px, 500px);
}
```

```css
body {
  width: 800px;
}
```


Refactorizando con funciones una funcionalidad creada anteriormente:

```scss
@use 'sass:color';

$colors: (primary, #4287f5), (secondary, #3a128a), (accent, #f59c42);

@function high-contrast($color) {
  @if (color.lightness($color) < 50) {
    @return white;
  } @else {
    @return black;
  }
}

@each $name, $color in $colors {
  .clr-#{$name} {
    background: $color;
    color: high-contrast($color);
  }
}
```

```css
.clr-primary {
  background: #4287f5;
  color: black;
}

.clr-secondary {
  background: #3a128a;
  color: white;
}

.clr-accent {
  background: #f59c42;
  color: black;
}
```


# Maps

Son similares a objetos JSON, están compuestos por llave y valor.
```scss
@use 'sass:map';

$font-sizes: (
  '400': 1.125rem,
  '500': 1.25rem,
  '600': 1.5rem,
  '700': 2rem
);

body {
  font-size: map.get($font-sizes, '400');
}
```

```css
body {
  font-size: 1.125rem;
}
```

Esto es útil por ejemplo para trabajar con los breakpoints
```scss
$breakpoints: (
  small: 30em,
  medium: 45em,
  large: 65em,
  xl: 80em
);
```


### Creando una función para obtener colores

```scss
@use 'sass:map';

$colors: (
  'primary': #1b6db5,
  'secondary': #4a1ab0,
  'accent': #d97614
);

@function clr($color) {
  $output-color: map.get($colors, $color);

  @if $output-color != null {
    @return $output-color;
  } @else {
  	@error '"#{$color}" is not a valid color inside the "$colors" map';
  }
}

.example {
  color: clr(primary);
}

.example-2 {
  color: clr(purple);
}
```

En este caso al compilar dado que nos da un error no se finaliza la compilación y se obtiene el siguiente mensaje:
```css
'"purple" is not a valid color inside the "$colors" map'
   ╷
24 │   color: clr(purple);
   │          ^^^^^^^^^^^
   ╵
  - 24:10  root stylesheet
```

Pero al comentar esa línea de código o enviar como parámetro un color que este en la lista se obtiene lo siguiente:

```css
.example {
  color: #1b6db5;
}

.example-2 {
  color: #4a1ab0;
}
```


Ahora podemos crear una función más completa la cual nos genere clases basadas en condiciones más especificas:
```scss
@use 'sass:color';
@use 'sass:map';

$colors: (
  'primary': #1b6db5,
  'secondary': #4a1ab0,
  'accent': #d97614
);

// this is explained in the video above if you want more information
@function clr($color) {
  $output-color: map.get($colors, $color);

  @if $output-color != null {
    @return $output-color;
  } @else {
    @error '"#{$color}" is not a valid color inside the "$colors" map';
  }
}

@function high-contrast($input) {
  $color: clr($input);
  @if (color.lightness($color) < 50) {
    @return white;
  } @else {
    @return black;
  }
}

@mixin good-color($color) {
  background-color: clr($color);
  color: high-contrast($color);
}

.example {
  @include good-color(primary);
}
```

Lo que genera:
```css
.example {
  background-color: #1b6db5;
  color: white;
}
```


## Nested Maps

Estas listas anidadas lucen de la siguiente manera:
```scss
$colors: (
  primary: (
    light: lightblue,
    normal: blue,
    dark: darkblue
  ),
  secondary: (
    light: pink,
    normal: red,
    dark: firebrick
  )
);
```

Para acceder a los valores anidados lo realizamos de la siguiente manera:
```scss
map.get($colors, secondary, dark)
```

Actualizando la función que creamos arriba (aunque realizada de una manera más simplificada) observamos lo siguiente:
```scss
@use 'sass:map';

$colors: (
  primary: (
    light: lightblue,
    normal: blue,
    dark: darkblue
  ),
  secondary: (
    light: pink,
    normal: red,
    dark: firebrick
  ),
  accent: orange
);

@function clr($color...) {
  @return map.get($colors, $color...);
}

body {
  color: clr(primary, dark);
}

h1 {
  color: clr(accent);
}
```

Lo que genera el siguiente código css:
```css
body {
  color: darkblue;
}

h1 {
  color: orange;
}
```


Otro ejemplo de una función es la siguiente
```scss
$colors: (
  primary: (
    light: lightblue,
    normal: blue,
    dark: darkblue
  ),
  secondary: (
    light: pink,
    normal: red,
    dark: firebrick
  )
);

@each $color-name, $shade-map in $colors {
  @each $shade-name, $shade-value in $shade-map {
  	.clr-#{$color-name}-#{$shade-name} {
    	color: $shade-value;
  	}
		.bg-#{$color-name}-#{$shade-name} {
    	color: $shade-value;
  	}
  }
}
```

Lo que genera el siguiente código css:
```css
.clr-primary-light {
  color: lightblue;
}

.bg-primary-light {
  color: lightblue;
}

.clr-primary-normal {
  color: blue;
}

.bg-primary-normal {
  color: blue;
}

.clr-primary-dark {
  color: darkblue;
}

.bg-primary-dark {
  color: darkblue;
}

.clr-secondary-light {
  color: pink;
}

.bg-secondary-light {
  color: pink;
}

.clr-secondary-normal {
  color: red;
}

.bg-secondary-normal {
  color: red;
}

.clr-secondary-dark {
  color: firebrick;
}

.bg-secondary-dark {
  color: firebrick;
}
```


### map.has-key()

```scss
@function clr($color) {
  @if map.has-key($colors, $color) {
    @return map.get($colors, $color)
  } @else {
    @error '$colors does not have that color!'
  }
}
```


### Creando una función para breakpoints con todo lo aprendido

```scss
@use 'sass:map';
@use 'sass:math';
@use 'sass:meta';

$breakpoints: (
  small: 30em,
  medium: 45em,
  large: 65em,
  xl: 80em
);

@mixin mq($size) {
  @if map.has-key($breakpoints, $size) {
    $breakpoint: map.get($breakpoints, $size);
    @media screen and (min-width: $breakpoint) {
      @content;
    }
  } @else if meta.type-of($size) == number {
    @if math.is-unitless($size) {
      @error 'when using a number with @mq() make sure to include a unit';
    } @else {
      @media screen and (min-width: $size) {
        @content;
      }
    }
  } @else {
    @error 'the keyword #{$size} is not in the $breakpoints map. You must either use a keyword from the map or a specific number (e.g. 40em)';
  }
}

body {
  // all of these should work
  @include mq(medium) {
    background: pink;
  }
  @include mq(500px) {
    background: lightblue;
  }
  @include mq(100em) {
    background: #efefef;
  }

  // these should fail, with an error telling me why
  @include mq(reallysmall) {
    background: yellow;
  }
  @include mq(5000) {
    background: purple;
  }
}
```




---


Además de esto sass nos provee diferentes funciones para poder modificar el brillo y oscuridad de un color, las cuales son:

```css
background-color: darken(color, %);
background-color: lighten(color, %);
```

# BEM

El patron de diseño BEM (Block Element Modifier) sirve para poder escribir CSS, donde El bloque es un elemento importante por si mismo, el elemento es parte de un bloque, este elemento no tiene un significado por si mismo, y el modificador como su nombre lo indica nos permite modificar el bloque o algún elemento de este, un ejemplo de la implementación de este patron de diseño es:

```css
header__logo--text {
  ...;
}
```


# CUBE CSS



# 7 - 1 pattern

Este patrón de arquitectura para escribir css nos permite dividir el código en 7 carpetas, las cuales contendrán los archivos de estilos y un archivo principal para poder importar todos los otros

Las siete carpetas para implementar este patrón son

```sh
abstracts/    ->  En esta carpeta se incluirán las variables y los mixins del proyecto
base/         ->  Este directorio contendrá las definiciones básicas de los estilos del proyecto
components/   ->  En este directorio iran los componentes del diseño, como los estilos de los botones, formularios, etc, donde será un archivo por componente
layout/       ->  Es para la maquetación del layout
pages/        ->  Estilos para las paginas
themes/       ->  Es para temas visuales, en estos se puede incluir el dark mode
vendors/      ->  Aquí pueden incluirse librerías o códigos de terceros
