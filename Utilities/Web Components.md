---
tags: utilities
---
----

[Documentación]()

----

# ¿Qué son los web components?

Los web components son elementos [[HTML]] personalizados los cuales encapsulan la lógica y el apartado del UI en un mismo archivo, esto los hace re-utilizables no solo dentro de un mismo proyecto o framework de Javascript.

## Creación de un web component

Para ello en el archivo `index.html` se debe incluir un script en la parte del head del documento. En dicho script debe aparecer la siguiente estructura:

```js
class Tooltip extends HTMLElement {
  constructor() {
    super();
  }
}

customElements.define('uc-tooltip', Tooltip);
```

Esto nos permite poder utilizar en la etiqueta personalizada `<uc-tooltip></uc-tooltip>` dentro del `index.html`.

Al momento de crear un elemento html personalizado se utiliza más de una palabra por convención, pues los elementos nativos de HTML son solo de una palabra.

## Ciclo de vida de un web component

- Constructor:Se ejecuta cuando de instancia una clase, este es ideal para inicializar las propiedades de la clase, no se recomienda para manipular el DOM pues en este punto del componente aún no es posible.
- `connectedCallback()`: Este se ejecuta tras el constructor y es ideal para inicializar el DOM.
- `disconnectedCallback()`: Este se encarga de limpiar el trabajo.
- `attributeChangedCallback()`: Actualiza el DOM y la data.

## Atributos

Al igual que los elementos nativos de HTML los web components pueden poseer atributos, por ejemplo:

```js
// tooltip.js
class Tooltip extends HTMLElement {
  constructor() {
    super();
    this._tooltipContainer;
    this._tooltipText = 'Some dummy tooltip text.';
  }

  connectedCallback() {
    if (this.hasAttribute('text')) {
      this._tooltipText = this.getAttribute('text');
    }
    const tooltipIcon = document.createElement('span');
    tooltipIcon.textContent = ' (?)';
    tooltipIcon.addEventListener('mouseenter', this._showTooltip.bind(this));
    tooltipIcon.addEventListener('mouseleave', this._hideTooltip.bind(this));
    this.appendChild(tooltipIcon);
  }

  _showTooltip() {
    this._tooltipContainer = document.createElement('div');
    this._tooltipContainer.textContent = this._tooltipText;
    this.appendChild(this._tooltipContainer);
  }

  _hideTooltip() {
    this.removeChild(this._tooltipContainer);
  }
}

customElements.define('uc-tooltip', Tooltip);
```

```html
<!-- index.html -->
<body>
  <p>
    <uc-tooltip text="Web Components is a set of standards.">
      Web Components
    </uc-tooltip>
    are awesome!
  </p>
  <uc-tooltip>Here we go!</uc-tooltip>
</body>
```

En este caso el párrafo dentro del `body` tiene el elemento personalizado `<uc-tooltip>`, el cual posee el atributo `text`, por lo que el texto del web component será el que se mande en dicho atributo desde el HTML. En el caso de ambas instancias del elemento personalizado `<uc-tooltip>` el texto se colocará debajo del elemento o nodo que este encierra.

## Estilos

Para poder agregarle estilos encapsulados a nuestro custom element podemos usar el shadow DOM y templates de HTML.

```js

```
