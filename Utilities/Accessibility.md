---
tags: utilities web mobile frontend
---
----

[Documentación]()

----

# ¿Qué es la accesibilidad?

La accesibilidad de la web es el diseño de un sitio pensando en que las personas con discapacidades puedan utilizarlo. Para esto se debe incluir en la web distintas herramientas.

Tipos de discapacidades

- Movilidad y física
- Cognitiva y neuronal
- Vidual
- Auditiva

## Accesibilidad en HTML

### Texto alternativo

Este consiste en una propiedad en el html con la cual cuando un lector de pantalla encuentre una imagen pueda describirla. En caso de que el sitio cuente con imágenes que son unicamente decorativas se puede implementar el atributo `alt` con un string vacío.

```html
<img
  src="https://pbs.twimg.com/media/KSJHECKJH983er.jpg"
  alt="A puppy in the park"
/>

<img src="https://pbs.twimg.com/media/KSJHECKJH983er.jpg" alt="" />
```

### Etiquetas

Las etiquetas (labels) se utilizan en los campos de los formularios, buscando con ello hacerlo más accesible cuando el campo sea enfocado.

Los labels pueden ser declarados tanto de forma explicita como implícita:

```html
<!-- Labels de forma explicita -->

<form>
  <label for="first">First Name</label>
  <input id="first" type="text" />
  <label for="last">Last Name</label>
  <input id="last" type="text" />
  <label for="password">Password</label>
  <input id="password" type="password" />
  <input type="submit" value="Submit" />
</form>

<!-- Labels de forma implícita -->

<form>
  <label>
    First Name
    <input id="first" type="text" />
  </label>
  <label>
    Last Name
    <input id="last" type="text" />
  </label>
  <label>
    Password
    <input id="password" type="password" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

Es importante mencionar que los labels solo funcionan con los siguientes elementos:

- `<button>`
- `<input>`
- `<keygen>`
- `<meter>`
- `<output>`
- `<progress>`
- `<select>`
- `<textarea>`

En caso de necesitar alguna etiqueta (label) para un elemento que no se incluya en la lista se utiliza la propiedad `aria-label`:

```html
<div aria-label="Interactive div">Hello</div>
```

### Contenido exclusivo para lectores de pantalla

También puedes colocar dentro del sitio web contenido al que solo es posible acceder utilizando un lector de pantalla, con esto puedes evitar saturar de manera visual el sitio web.

```css
.visuallyhidden {
  position: absolute;
  left: 0;
  top: -500px;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
```

### Ejemplo de accesibilidad en un botón

```html
<div
  aria-label="Alert the word hello"
  tabindex="0"
  role="button"
  onclick="alert('hello')"
  onkeyup="alert('hello')"
>
  Click me!
</div>
```

Este botón tiene como función lanzar una alerta cada vez que se accione, pensando en accesibilidad se añadieron distintas propiedades adicionales:

- `aria-label`: Este es el mensaje que leerá el lector de pantalla.
- `tabindex`: Esta propiedad permite que los usuarios puedan acceder al botón por medio de tabulaciones en el teclado.
- `role`: Esta propiedad permite a los lector de pantalla hacer saber que el elemento puede ser clickeado.
- `onclick`: Evento que se ejecuta al hacerle click al botón.
- `onkeyup`: Evento que se ejecuta al darle enter al botón.

## ARIA

[ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) (Accessible Rich Internet Applications) es un conjunto de [atributos](https://www.w3.org/TR/html-aria/#aria-table) que ayudan a desarrollar aplicaciones web que sean más accesibles para personas con discapacidades.

### aria-label

Permite especificar un string para usar como etiqueta accesible. Esto anula cualquier otro mecanismo de etiquetado nativo, como un elemento `label` — por ejemplo, si un button tiene contenido de texto y un `aria-label`, solo se usará el valor de `aria-label`.

```html
<button aria-label="menu" class="hamburger"></button>
```

### aria-labelledby

Esta propiedad permite especificar el ID de otro elemento del DOM como etiqueta de un elemento.

```html
<span id="rg-label">Drink options</span>
<div class="radiogroup" aria-labelledby="rg-label">
  <!-- Este grupo recibe el nombre de Drink options -->
</div>
```

### aria-describedby

Esta propiedad brinda una descripción haciendo referencia a elementos que de otra manera no son visibles.

```html
<label for="pw">Password</label>
<input type="password" id="pw" aria-labelledby="pw-help" />
<div id="pw-help" class="visuallyhidden">
  La contraseña debe contener al menos 12 caracteres
</div>
```

Se pueden consultar más propiedades en [este enlace](https://developers.google.com/web/fundamentals/accessibility/semantics-aria/aria-labels-and-relationships?hl=es).
