---
tags: guide frontend css
---
----

La mayoría de las arquitecturas de estilos en CSS para mayor escalabilidad viene en la implementación del `SCSS`.

[https://www.loom.com/share/29e0bf422ad641dc99eb5b0d36a66b89](https://www.loom.com/share/29e0bf422ad641dc99eb5b0d36a66b89)

[https://codebeautify.org/scss-to-css-converter](https://codebeautify.org/scss-to-css-converter)

```scss
%btn {
  display: flex;
  padding: 0.5em;
}

.btn-primary {
  @extend %btn;
  background-color: blue;
  color: #000;
}

.btn-secondary {
  @extend %btn;
  background-color: red;
  color: #fff;
}

.btn-tertiary {
  @extend %btn;
  background-color: green;
  color: #fff;
}
```

Esto al compilarse a CSS se convierte en lo siguiente:
```scss
.btn-primary, .btn-secondary, .btn-tertiary {
  display: flex;
  padding: 0.5em;
}
.btn-primary {
  background-color: blue;
  color: #000;
}
.btn-secondary {
  background-color: red;
  color: #fff;
}
.btn-tertiary {
  background-color: green;
  color: #fff;
}
```

Como se puede observar el placeholder `%btn` se convierte a las clases `.btn-primary`, `.btn-secondary`, `.btn-tertiary`. Esto a la larga puede traer problemas de escalabilidad, por ende la solución más viable pensando en una mejor implementación de una arquitectura es realizar una clase independiente con una responsabilidad única:
```scss
.btn {
  display: flex;
  padding: 0.5em;
}

.btn-primary {
  background-color: blue;
  color: #000;
}

.btn-secondary {
  background-color: red;
  color: #fff;
}

.btn-tertiary {
  background-color: green;
  color: #fff;
}
```

De esta manera al implementar la clase dentro del archivo de `HTML` obtendremos un resultado similar al siguiente:
```html
<a href="#" class="btn btn-primary">Some Link with button styles</a>
```

Es posible utilizar mixin’s para tratar de facilitar la implementación de estilos en el código, estos mixin’s funcionan como funciones dentro de cualquier lenguaje de programación:
```scss
@mixin justify($vertical-align: center) {
  display: flex;
  justify-content: center;
  align-items: $vertical-align;
}

.hero {
  @include justify;
  background-color: #444;
  color: #fff;
}

.footer {
  @include justify($vertical-align: flex-end);
  background-color: #333;
  color: #eee;
}

.section {
  @include justify(flex-start);
  background-color: #ccc;
  color: #333;
}
```

Esto al compilarse se convierte en lo siguiente:
```scss
.hero {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #444;
  color: #fff;
}
.footer {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  background-color: #333;
  color: #eee;
}
.section {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  background-color: #ccc;
  color: #333;
}
```


## Implementación con BEM

```css
header__logo{ Element block }
header--main{ modifier of block }
```

Un patrón común al crear la estructura de la arquitectura de los estilos de CSS es imitar la estructura del archivo HTML:

```html
<article class="card">
  <img src="#" alt="#" class="card__img" />

  <div class="card__info">
    <h3 class="card__title">Some title</h3>
    <strong class="card__meta">Ángel Cruz</strong>
  </div>

  <button class="button button--primary">CTA</button>
</article>

<style>
  .card {
    &__info{}
    &__title{}
    &__meta{}
    &__img{}
  }
</style>
```

En caso de querer realizar una tarjeta con un modificador se puede realizar lo siguiente:

```html
<article class="card card-dark">
  <img src="#" alt="#" class="card__img" />
  <h3 class="card__title">Some title</h3>
  <strong class="card__meta">Ángel Cruz</strong>
</article>
```

```scss
.card {
  $self: &;
  &__img { }
  &__title { }
  &__meta { }

  &--dark {
    #{$self}__title { }
    #{$self}__meta { }
  }
}
```

También se puede hacer uso de utilidades, estas se recomienda que solo realicen un trabajo en concreto:

```html
<article class="card card-dark">
  <img src="#" alt="#" class="card__img" />
  <h3 class="card__title is-centered">Some title</h3>
  <strong class="card__meta is-centered">Ángel Cruz</strong>
</article>
```

```scss
.card {
  $self: &;
  &__img { }
  &__title { }
  &__meta { }

  &--dark {
    #{$self}__title { }
    #{$self}__meta { }
  }
}

.is-centered {
  text-align: center !important;
}
```

A las utilidades se recomienda incluirles el `!important`, pues se debe cerciorar que este estilo se aplique sobre todos los demás.


## Atomic Design

Existen los átomos, estos se tratan de piezas ya indivisibles dentro del diseño, por ejemplo:

- botones
- pills
- modal

Moleculas: Estos se tratan de componentes del layout, los cuales se conforman de diferentes átomos, por ejemplo:

- cards
- call to action’s
- forms

Organismos: Estos se tratan ya más de secciones dentro del sitio web, y estos se conforman de moléculas, obedeciendo más al layout, por ejemplo:

- hero
- section

Estos suelen organizarse de la siguiente manera

```
styles/
├── atoms/
│   ├── _button.scss
│   ├── _image.scss
│   └── _pill.scss
├── molecules/
│   ├── _card.scss
│   └── _form.scss
├── organisms/
│   ├── _gallery.scss
│   └── _form.scss
└── index.scss
```

Esta estructura conlleva ciertos problemas de organización con algunos archivos como:

- _normalize.scss
- _ul-list.scss

Para solventar estos inconvenientes ahora podemos abordar la estructura ITCSS:

```
styles/
├── settings/
│   ├── _colors.scss
│   └── _typography.scss
├── tools/
│   └── _mixins.scss
├── generic/
│   ├── _normalize.scss
│   └── _box-sizing.scss
├── elements/
│   ├── _headings.scss
│   └── _links.scss
├── objects/
│   ├── _container.scss
│   └── _ui-list.scss
├── components/
│   ├── _button.scss
│   ├── _card.scss
│   ├── _forms.scss
│   ├── _header.scss
│   └── ...
├── utilities/
│   ├── _typography.scss
│   └── _error.scss
└── index.scss
```

Objetos: solo dan estructura, no colores ni theme como containers, ui-list, etc

Atoms: Elementos pequeños que no se pueden dividir en algo más pequeño

Combinando lo mejor de los dos mundos podemos encontrar lo siguiente:

```
styles/
├── settings/
│   ├── _colors.scss
│   └── _typography.scss
├── tools/
│   └── _mixins.scss
├── generic/
│   ├── _normalize.scss
│   └── _box-sizing.scss
├── elements/
│   ├── _headings.scss
│   └── _links.scss
├── objects/
│   ├── _container.scss
│   └── _ui-list.scss
├── atoms/
│   ├── _button.scss
│   ├── _image.scss
│   └── _pill.scss
├── molecules/
│   ├── _card.scss
│   └── _form.scss
├── organisms/
│   ├── _gallery.scss
│   └── _header.scss
├── utilities/
│   ├── _typography.scss
│   └── _error.scss
└── index.scss
```

```scss
// Generic
@use "generic/normalize";
@use "generic/box-sizing";

// Elements
@use "elements/body";
@use "elements/headings";
@use "elements/img";
@use "elements/links";

// Objects
@use "objects/container";
@use "objects/ui-list";

// Atoms
@use "atoms/box";
@use "atoms/btn";
@use "atoms/cover-background";
@use "atoms/pill";
@use "atoms/typography";

// Molecules
@use "molecules/card";
@use "molecules/cta-box";
@use "molecules/dropdown";
@use "molecules/quote";
@use "molecules/video-thumbnail";

// Organisms
@use "organisms/business-callout";
@use "organisms/courses";
@use "organisms/cta";
@use "organisms/footer";
@use "organisms/header";
@use "organisms/hero";
@use "organisms/partners";
@use "organisms/subscribe";
@use "organisms/testimonials";

// Utilities
@use "utilities/error";
```

## Especificidad

![[especificidad.png]]

## Breakpoint values

- Screen wide: 1920
- Screen desktop: 1280
- Tablet landscape: 1024
- Tablet portrait: 768
- Phone XL: 414
- Phone LG: 411
- Phone MD: 375
- Phone SM: 360
- Phone XS: 320

![[min-max.png]]

![[break-point.png]]