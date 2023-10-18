---
tags: programming, frontend
---

---

[CSS: Cascading Style Sheets | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/CSS)

---

# ¿Qué es CSS?

Cascading Style Sheets (Hojas de Estilo en Cascada) es el lenguaje de estilos utilizado para describir la presentación de documentos HTML o XML (incluyendo varios languages basados en XML como SVG, MathML o XHTML). CSS describe como debe ser renderizado el elemento estructurado en la pantalla, en papel, en el habla o en otros medios.

La sintaxis de CSS3 se compone de un selector y de las propiedades que se le aplican a este.

```css
selector {
  propiedad: valor;
}

/* Ejemplo: */

h1 {
  font-size: 18px;
}
```

# Selectores

Los selectores definen sobre qué elementos se aplicará un conjunto de reglas CSS, estos pueden ser etiquetas de HTML, clases o ID's.

De la misma manera existen selectores más avanzados, algunos ejemplos de estos son:

Selector universal: Todos los elementos

```css
* {
  ...;
}
```

Selector adyacente: Elemento precedido por otro elemento

```css
h1 + h2 {
  ...;
}
```

Selector de hermanos: Elemento hermano de otro elemento sin necesidad de estar precedido por él

```css
h1 ~ h2 {
  ...;
}
```

Selector descendente: Aplica a elementos contenidos por el otro elemento indicado sin ser estrictamente hijo directo

```css
section p {
  ...;
}
```

Selector de hijo directo: Aplica la propiedad al hijo directo

```css
section > p {
  ...;
}
```

## **Selectores de atributo**

Adicionalmente a esto existen selectores de atributos, por ejemplo:

Este aplica estilos a todas las etiquetas a que contengan una propiedad title.

```css
a[title] {
  ...;
}
```

Aplica a todas las etiquetas a con propiedad href que terminen con html.

```css
a[href$='html'] {
  ...;
}
```

Aplica a todos los que comiencen con la palabra página.

```css
a[href^='pagina'] {
  ...;
}
```

Aplica a todas las etiquetas title que contengan el valor “dos” separado por espacios.

```css
a[title~='dos'] {
  ...;
}
```

Aplica a los que contienen social separados por un guion.

```css
a[title|='social'] {
  ...;
}
```

Aplica a todas las etiquetas a que tengan la propiedad href y un valor página sin importar si este está separado por espacios o guiones.

```css
a[href*='pagina'] {
  ...;
}
```

# Posicionamiento

## Static
Es el valor por defecto de `position`. En este caso el elemento continua con el flujo normal del documento HTML. Las propiedades top, right, bottom, left y z-index no tienen efecto.

## Relative
El elemento en este caso mantiene el espacio dentro del flujo del documento HTML. En este caso el elemento puede modificar su posición con top, ... 

## Absolute
En este caso el elemento es removido del flujo del documento HTML y ahora es relativo al padre que tenga la posición relativa, en caso de no tener alguno es relativo al documento.

## Fixed


## Sticky


## z-index
Especifica la elevación de un elemento relativo a otra, esto es un stack


# Pseudoselectores y pseudoelementos

Las _**pseudoclases**_ son palabras clave que se le añaden a los selectores y que especifican un estado especial del elemento seleccionado.

```css
a:hover {
  ...;
}
```

Los _**pseudoelementos**_ añaden características solo a una parte especificada por el selector.

```css
a::first-line {
  ...;
}
```

Para evitar un estilo general a todos los selectores estos pueden hacer uso de un id, el cual al ser único permite que cada etiqueta (o selector) tenga un diseño más personalizable.

```css
<p
  id='titulo'
  > </p
  >                                                        
  #titulo {
  ...;
}
```

También se puede hacer uso de estilos para clases, lo que permitirá aplicar CSS a distintos elementos simultáneamente.

```css
<p
  class='text'
  > </p
  >              
  <h2
  class='text'
  > </h2
  >                
  .text {
  ...;
}
```

# Unidades

## Relativas

Porcentajes: Estas dependen del tamaño del padre, o en ocasiones del elemento en sí mismo. Ejemplo:
- width: 50%; Toma el valor de la mitad del padre
- line-height: 50%; Toma la mitad del tamaño de la fuente del elemento al que se le aplica.

## Root EMs

Relativo al tamaño de fuente del root html. Si el tamaño de la fuente 


em
Son unidades relativas al tamaño de la fuente del elemento al que se le aplican


vw
Hace referencia al ancho del viewport (pantalla del dispositivo), de tal manera que 1vw es igual al 1% del ancho

- ch
- px

[New Viewport Units - Ahmad Shadeed (ishadeed.com)](https://ishadeed.com/article/new-viewport-units/)

100vh
100dvh

# Propiedades más comunes

## Bordes

```css
selector {
  border-width: 10px;
  border-style: solid;
  border-color: red;
}
```

Este código se puede resumir de la siguiente manera:

```css
selector {
  border: 10px solid red;
}
```

## Fondos

```css
selector {
  background-color: lightgray;
  background-image: url('...');
  background-repeat: no-repeat;
  background-position: center;
}
```

Este código se puede resumir de la siguiente manera:

```css
selector {
  background: lightgray url('...') no-repeat center;
}
```

```css
selector {
  background-size: contain; /* Alto del elemento */
  background-size: auto; /* Tamaño de la imagen */
  background-size: cover; /* Cubre el elemento con la proporción de la imagen */
}
```

## Box Shadow

```css
selector {
  box-shadow: horizontal-position vertical-position blur spread/radius(
      how far out from the element is
    ) color;
}
```

Adicionalmente a eso se le puede agregar la propiedad `inset` para aplicar el efecto por dentro del modelo de caja.

```css
selector {
  box-shadow: inset horizontal-position vertical-position blur spread/radius
    color;
}
```

# Fuentes y textos en CSS

Para poder importar fuentes en CSS podemos hacer lo siguiente:

```css
@font-face {
  font-family: 'FontName';
  src: url('...') format('truetype');
}
```

```css
selector {
  font-size: 1.2rem;
  font-weight: 700;
  font-style: italic;

  line-height: 1.3;

  text-decoration: none;
  text-align: justify;
  text-transform: uppercase;

  letter-spacing: -1px;
  word-spacing: 10px;
}
```

```css
selector {
  display: block;
  display: inline;
  display: inline-block;
  visibility: hidden;
  opacity: 0;
}
```

```css
selector {
  padding-top: 10px;
  padding-right: 15px;
  padding-bottom: 10px;
  padding-left: 15px;
}
```

Este código se puede resumir de la siguiente manera:

```css
selector {
  padding: 10px 15px 10px 15px;
}
```

```css
selector {
  width: 60vw;
  min-height: 300px;
  max-height: 500px;
}
```

# Arquitecturas en CSS

Estas tienen como finalidad el crear un código que sea predecible, reutilizable, mantenible y escalable.

Entre las buenas prácticas para implementar estas arquitecturas se encuentra:

- Establecer reglas o lineamientos de diseño
- Explicar la estructura base
- Establecer estándares de codificación
- Evitar largas hojas de estilo
- Documentación

## **OOCSS**

El CSS Orientado a Objetos separa el diseño del contenido pudiendo utilizar de mejor manera el código por medio de clases dedicadas a una utilidad especifica.

## **BEM**

La arquitectura de Bloque-Elemento-Modificador separa estos componentes en sí mismos

## **SMACSS**

Arquitectura de CSS Escalable y Modular, para ello se deben seguir 5 pasos:

- Dividir el CSS en componentes básicos (como los botones o títulos)
- Dividir el contenido en los layouts (como el header o el footer)
- Módulos, estos son componentes que se utilizan en la aplicación más de una vez
- Estado, este hace referencia a las acciones de los elementos ante la interacción con el usuario (como un botón con el estado hover)
- Temas

## **ITCSS**

Esta metodología consiste en dividir los archivos CSS evitando que se combinen entre sí, por ejemplo:

- Ajustes
- Herramientas
- Generico
- Elementos
- Objetos
- Componentes
- Utilidades

Esto ayuda también a evitar el tema de la especificidad.

## Atomic Design

Consiste en la creación de componentes (átomos) que crearán layouts (moléculas) y estos generarán organismos (páginas).

Puedes ver una variante personal entre ITCSS y Atomic Design en la siguiente página:

[[Estructura de estilos CSS]]

# Flexbox

Para poder aplicar la propiedad de [flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) es necesario que el padre del elemento al cual le quieres aplicar esta propiedad contenga la propiedad de `display: flex;` el cual tomará toda la fila del contenedor, mientras que si el padre utiliza la propiedad `display: inline-flex;` tomará solo el ancho del espacio disponible.

La propiedad de `flex-direction` tiene cuatro posibles valores; `row`, `row-reverse`, `column`, `column-reverse`. El valor por defecto que tiene esta propiedad es `row`

El eje cruzado va perpendicular al eje principal, por lo tanto si `flex-direction` es `row` o `row-reverse` el eje cruzado ira por las columnas, si el eje principal es `column` o `column-reverse` entonces el eje cruzado corre a lo largo de las filas

Entre otras propiedades que se pueden usar son:

- `display: flex;` El cual utiliza toda la fill
- `inline-flex;` Toma el ancho del elemento

A nivel contenedor `flex-grap: wap` es la propiedad por defecto, el tener la propiedad wrap hace que el contenido de desborde o salga del contenedor, mientras que la propiedad no-wrap limita el contenido al ancho del contenedor, en el cual al alcanzar este ancho colocara los elementos por debajo de otros, también existe la propiedad row-reverse

Existe la propiedad `flex-flow: column no-wrap` , en esta propiedad se pueden resumir las propiedades de flex-direction y flex-wrap

La propiedad `flex-grow: 1;` recibe un numero el cual indica que crecen los números y toman todo el espacio disponible

- `flex-shrink: 0;` hace mas pequeño el elemento
- `flex-basis: 50%;` crece el elemento el 50% (como valor mínimo) del contenedor del padre

```css
sector {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: wrap;
  align-content: stretch;
}
```

Todos los elementos por defecto tienen orden 0, por lo que al agregarle a un elemento orden 1 lo situará al inicio y -1 al final

La propiedad flex-grow por defecto también contiene el valor de 0, por lo que al incrementarlo él elemento que contenga esta propiedad y este valor tomara todo el espacio disponible dentro del elemento padre.

# Grid

[Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) es una herramienta que ayuda en la maquetación de los sitios web, esta pensado para ser usado en el layout de la página a diferencia de flexbox que se utiliza para los componentes.

Para poder habilitar las propiedades de grid debes hacer uso de `display: grid;`, lo cual habilita grid en los estilos que se aplique.

```css
selector {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(3, auto);

  grid-template-areas:
    'articleheader articleheader'
    'firstp thevideo'
    'theimage secondp';
}

article header {
  grid-area: articleheader;
}
```

```css
selector {
  grid-row-start: 1;
  grid-row-end: 2;

  grid-column-start: 1;
  grid-column-end: 6;

  grid-row: 1 / 2;
  grid-column: 1 / 6;
}
```

# Tips

Para el scroll

```css
html {
  scroll-behavior: smooth;
}
```

---

Crear un wrapper, esto te ayuda a mantener delimitado el ancho de tu contenido y tenerlo centrado, se recomienda crear el wrapper por sección, por ejemplo:

```html
<main>
  <div class="wrapper">
    <div class="main-content">
      <!-- Contenido del main -->
    </div>
  </div>
</main>
```

De esta manera `main` puede seguir fluyendo normalmente dentro del sitio, mientras el wrapper delimita los contenidos de este.

---

Para poder ocultar un elemento con ayuda de CSS se tienen varias alternativas:

```css
display: none;
/* ------ */
opacity: 0;
```

El problema con utilizar `display: none;` es que esta transición no se puede animar, en lugar de esto existe otra alternativa, la cual es utilizar `opacity: 0;`, lo cual permite realizar la transición pero tiene el problema de que el contenido aun es accesible desde el mouse, el teclado y desde los lectores de pantalla.

Para poder hacer los elementos de el selector inaccesibles para el mouse y el teclado se utiliza `pointer-events: none;`, mientras que para ocultarlo de los lectores de pantalla se puede utilizar `visibility: hidden;`. De esta manera el selector queda de la siguiente manera:

```css
hidden {
  opacity: 0;
  pointer-events: none;
  visibility: hidden;
}
```

---

Es posible implementar un scroll animado como se hace en el primer tip de la lista previniendo el comportamiento por defecto de los enlaces, para esto se necesita javascript y se hace de la siguiente manera:

---

`:target` es un pseudo selector con el cual puedes agregar estilos al elemento que va como objetivo

`:only-child` solo funciona si es el unico hijo del elemento

`:only-of-type` solo funciona si es el unico elemento de su tipo

`:pseudoclass` deal with state, select the entire element

`::pseudoelement` select part of the element

```css
.p {
  max-width: 45ch;
}
```

```css
.component .myComponent .myComponent-part .myComponent-anotherPart .is-active;
```

---

El elemento con la clase `main-header` se quedará pegado todo el tiempo:

```css
.main-header {
  position: sticky;
  top: 0;
}
```

Para realizar un efecto paralax

```css
.section-1 {
  background: url('...');
  background-size: cover;
  background-position: center center;
  background-attachment: fixed;
}
```

El posicionamiento `sticky` mantiene el elemento fijo unicamente en el contenedor padre.

Para que el posicionamiento `sticky` funcione es necesario que el contenedor tenga un alto establecido o que su contenido establezca su alto.

---

SVG

```scss
Teniendo en el HTML:

<svg class="feature__icon">
	<use xlink:href="img/sprite.svg#icon-lock"></use>
</svg>

En CSS se realiza lo siguiente:

.feature {
  &__icon {
    fill: $color-primary;
    width: 4.5rem;
    height: 4.5rem;
  }
}
```

---

# Orden recomendado para elementos

## Nivel 1

- position
- z-index
- overflow
- top
- bottom
- left
- right
- transform
- transform-origin

## Nivel 2

- display
- flex
- flex-direction
- align-items
- align-self
- justify-content
- grid-template-columns
- grid-template-rows
- grid-template-areas

## Nivel 3

- width
- min-width
- max-width
- height
- min-height
- max-height
- line-height
- margin
- padding

## Nivel 4

- color
- cursor
- border
- border-radius
- background
- font-size
- font-weight
- font-family
- letter-spacing

## Nivel 5

- filter
- opacity
- visibility
- backface-visibility
- appearance
- outline

## Nivel 6

- animation
- transition

---

display
position
box-model
typography
animations
miscellaneous


# Medidas comunes para media queries

```css
@media only screen and (min-width: 768px){}


```

- Mobile: 0 - 480px
- Tablet: 481px - 768px
- Desktop (small): 769 - 1024px
- Desktop (large): 1025px

Estos breakpoints son los más comunes, pero cada proyecto y cada layout necesita ser revisado y ajustado para que sea visible desde cualquier medida de pantalla



## Especidad
[The CSS Cascade (wattenberger.com)](https://2019.wattenberger.com/blog/css-cascade)

