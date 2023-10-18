---
tags: frontend, programming
---

---

[HTML: HyperText Markup Language | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTML)

---

# ¿Qué es HTML?

HTML (HyperText Markup Language), es un lenguaje de marcado el cual describe el contenido de una página web gracias a sus elementos o etiquetas.

La página de inicio o la raíz de cualquier aplicación web es `index.html`, este es el archivo que será solicitado al servidor por el navegador para poder ingresar al dominio o página web.

![[request.png]]

# Anatomía de un elemento HTML

Los elementos de HTML se componen de etiquetas y su contenido, por ejemplo:

```html
<h1>Hola Mundo</h1>
```

Para este ejemplo el elemento se compone de la etiqueta de apertura `<h1>`, el contenido `Hola Mundo` y por último al etiqueta de cierre `</h1>`.

Los elementos generalmente poseen atributos dentro de la etiqueta de apertura:

```html
<h1 class="header">Hola Mundo</h1>
```

Para este ejemplo `class` es un atributo de las etiquetas de HTML, mientras que `header` es el valor de dicho atributo.

# Estructura de un documento HTML

Un documento HTML es una estructura de etiquetas que describen el contenido de una página web.

La estructura más básica y general es la siguiente:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Aquí va la descripción de la página" />
    <title>Titulo</title>
  </head>
  <body></body>
</html>
```

El bloque de código pasado contiene las siguientes etiquetas:

`<!DOCTYPE html>` Indica al navegador que se trata de un documento de HTML 5.

`<html lang='en'></html>` Inicio del documento (la propiedad lang nos indica el language de la página).

Después de estas etiquetas se encuentran dos importantes:

- head
- body

## Head

La etiqueta `<head></head>` contiene los datos visibles para el navegador e invisibles para el usuario como la descripción del sitio o el titulo que tendrá la pestaña del navegador al estar dentro de la página.

`<meta charset="UTF-8">` Etiqueta para caracteres especiales (como la ñ o acentos).

`<meta name="viewport" content="width=device-width, initial-scale=1.0">` Esta etiqueta habilita la funcionalidad del responsive design y declara que la escala inicial del sitio web será de 1.

`<meta name="description" content="Aquí va la descripción de la página" />` Con esta etiqueta se añade la descripción de la página.

`<title></title>` Título o nombre que tendrá la página en la pestaña o ventana del navegador.

Esta es la configuración más básica de la etiqueta `head`, existen más configuraciones que pueden consultarse en esta [sección](./head-tags.md).

## Body

`<body></body>` Etiqueta que contendrá el desarrollo del sitio web, todo su contenido será visible para el usuario.

Dada la extensión de la lista de este tipo de etiquetas la información se encuentra en este otra [sección](./body-tags.md).

# Configuración avanzada dentro de head

## Favicon

El favicon es el icono o símbolo que acompañará a tu URL en los navegadores. Esta pequeña imagen tiene como función principal ayudarte a identificar más fácilmente una determinada URL.

```html
<head>
  <link rel="icon" href="favicon.ico" />
</head>
```

### Favicon para dispositivos Apple

`<link rel="apple-touch-icon" href="/custom_icon.png"/>` Este icono es para poderse mostrar en los dispositivos de iOS, pues puede llegar a ser útil a la hora de visualizarse desde un equipo iOS, entro los tamaños más óptimos están 72x72, 114x114 y 144x144

```html
<head>
  <link rel="apple-touch-icon" href="touch-icon-iphone.png" />
  <link rel="apple-touch-icon" sizes="72x72" href="ipad.png" />
  <link rel="apple-touch-icon" sizes="114x114" href="iphone4.png" />
  <link rel="apple-touch-icon" sizes="144x144" href="retina-ipad.png" />
</head>
```

# Etiquetas dentro de Body

Ya que la etiqueta `<body></body>` será la encargada de contener y mostrar todo el contenido de la página web, cada componente que sea incluido dentro de esta etiqueta tiene sus propias etiquetas, entre las más comunes se encuentran las siguientes:

## Etiquetas para títulos

Estas etiquetas definen el título de una sección de la página web. Un ejemplo de como se puede utilizar estas etiquetas es el siguiente:

```html
<h1>Esto es un título</h1>
```

Existen seis tipos de títulos, donde el número indica la profundidad del mismo.

| Etiqueta    | Descripción                            |
| ----------- | -------------------------------------- |
| `<h1></h1>` |  Etiqueta para título de primer nivel  |
| `<h2></h2>` |  Etiqueta para título de segundo nivel |
| `<h3></h3>` |  Etiqueta para título de tercer nivel  |
| `<h4></h4>` |  Etiqueta para título de cuarto nivel  |
| `<h5></h5>` |  Etiqueta para título de quinto nivel  |
| `<h6></h6>` |  Etiqueta para título de sexto nivel   |

Se recomienda hacer uso de una sola etiqueta `h1` por página del sitio web, de lo contrario podría generar problemas al indexar el sitio al buscador.

## Etiquetas para textos

```html
<p>
  Lorem ipsum dolor sit <strong>amet consectetur</strong> adipisicing elit.<br />
  Fugit laboriosam nesciunt totam enim accusamus eum suscipit impedit sequi,
  quidem, consequuntur illum excepturi dolorum molestiae, porro natus a. Eaque,
  facilis eum!
</p>
```

El bloque de código pasado contiene las siguientes etiquetas:

| Etiqueta                        | Descripción                                                           |
| ------------------------------- | --------------------------------------------------------------------- |
| `<p></p>`                       | Esta etiqueta se utiliza para definir un párrafo.                     |
| `<span></span>`                 |  Para agregarle estilos a una parte de un texto posteriormente en CSS |
| `<br/>`                         |  Salto de línea (no necesita cierre)                                  |
| `<hr/>`                         |  Crea un salto de línea y una línea horizontal (no necesita cierre)   |
| `<strong></strong>` / `<b></b>` |  Para el texto en negritas                                            |
| `<em></em>` / `<s></s>`         |  Para el texto en cursiva                                             |

## Etiquetas para listas

Estas etiquetas deben ir dentro del body y como su nombre lo indica sirven para crear listas de manera semántica, dichas etiquetas ofrecen estilos por defecto pero estos estilos pueden ser modificados con CSS.

Principalmente existen dos tipos de listas, las ordenadas y las no ordenadas:

```html
<ol>
  <li></li>
  <li></li>
</ol>

<ul>
  <li></li>
  <li></li>
</ul>
```

`<ol></ol>` Lista ordenada
`<ol></ol>` Lista no ordenada
`<li></li>` Elemento de la lista

Además de estas dos existe otra lista llamada lista de definición cuya sintaxis es la siguiente:

```html
<dl>
  <dt></dt>
  <dd></dd>
</dl>
```

`<dl></dl>` Lista de definición
`<dt></dt>` Término a definir
`<dd></dd>` Definición del término

## Etiquetas para enlaces

Generalmente en las páginas web se encuentran ciertos textos, los cuales reaccionan al evento click y con esto abren o redirigen a otra web o a otra sección dentro de la misma página, estos elementos son conocidos como anchor tags.

```html
<a href="...">Texto del enlace</a>
```

Se le pueden agregar atributos a esta etiqueta, como `target=”_blank”` para abrir en enlace en una nueva pestaña. También se puede dirigir en una single page a un título de una sección desde el menú

```html
<a href="#quehacemos">Que hacemos</a>
```

Lo que redirigirá a:

```html
<h2 id="quehacemos">Que hacemos</h2>
```

Se pueden incrustar enlaces en imágenes y descargar las mismas desde un enlace.

`<a href="enlace"> <img src="..." alt="..."> </a>`
`<p> <a href="enlace" download="enlace">Descargar imagen</a> </p>`

## Etiquetas para imágenes

```html
<img src="..." alt="..." title="..." width="..." height="..." />
```

Donde **_“src”_** es la ruta de la imagen (esta puede ser de la carpeta del proyecto o de otra página web), **_“alt”_** es el nombre que nos mostrará en caso de que no cargue la imagen, **_“title”_** será el nombre que aparezca al colocar el cursor sobre la imagen.

**_“width”_** y **_“height”_** son para el ancho y el alto de una imagen, con esto se ajustará a un tamaño estricto dado en pixeles, se puede dar solo un dato y el otro se ajustará manteniendo las proporciones originales de la imagen.

### SVG

Para poder incrustar imagenes en formato SVG se tiene que hacer de la siguiente manera:

```html
<svg class="feature__icon">
  <use xlink:href="img/sprite.svg#icon-lock"></use>
</svg>
```

Donde la referencia va a apuntar al archivo que contiene todos los iconos en formato SVG, se usara el # para poder acceder de manera particular a un ícono especifico.

## Etiquetas para tablas

```html
<table>
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td></td>
      <td></td>
      <td colspan="3">Atributo para usar un número de 3 columnas</td>
    </tr>
  </tbody>
  <tfoot></tfoot>
</table>
```

`<table></table>` Etiqueta de apertura y cierre de la tabla
`<thead></thead>` Cabecera de la tabla
`<tbody></tbody>` Cuerpo de la tabla
`<tfoot></tfoot>` Pie de la tabla
`<tr></tr>` Representa una fila de celdas en la tabla
`<th></th>` Representa una celda encabezado en una tabla

## Etiquetas para formularios

```html
<form action="/some/dir" method="POST">
  <fieldset>
    <legend></legend>
    <label for="nombre"></label>
    <input type="text" name="nombre" id="nombre" placeholder="..." />
  </fieldset>
</form>
```

`<form></form>` Apretura y cierre del formulario

`action` es el lugar a donde la información del formulario se va a enviar

`method` es el método por como se mandará la información, con get va por parámetro de url y por post viaja dentro de la petición

`<fieldset></fieldset>` Para colocar el formulario en un cuadro

`<legend></legend>` Titulo del formulario

`<label for="nombre"></label>` Dato que se solicita(en este caso el nombre)

`<input type="text" name="nombre" id="nombre" placeholder="..." />` Dato que se ingresa al formulario (en este caso el nombre)

Donde ***“type”*** es el tipo de dato que se solicita, ***name*** es información que se le dará al servidor, ***id*** es la etiqueta que se la da al dato del formulario, esto se conecta con el for del label y ***placeholder*** será el texto predeterminado en el formulario, este se puede cambiar con ***value***.

Los tipos de input para los formularios más comunes son:

- text
- password
- date
- datetime-local
- file
- month
- number
- range
- search

Adicionalmente a la etiqueta input se le pueden agregar más atributos como ***disabled*** para desactivar la etiqueta, maxlength=”#” para agregar un límite de caracteres, max=”#” y min=”#” para delimitar los valores máximos y mínimos. step=”#” se puede utilizar para ascender de # en #.

El atributo `required` es para que un campo del formulario sea requerido, de no ser introducido no se podrá enviar el formulario, `autofocus` es para que al momento de actualizar o ingresar a la página del navegador el cursor aparezca automáticamente en el campo al cual se le asigne este atributo.

También existen etiquetas para realizar una selección usando los formularios:

```html
<form>
  <fieldset>
    <legend>Dropdown</legend>
    <label for="dd">Options</label>
    <select id="dd">
      <option value="optOne">Option One</option>
      <option value="optTwo">Option Two</option>
    </select>
  </fieldset>
</form>
```

También existe la etiqueta `<textarea></textarea>`, la cuál habilita una caja de texto que permite enviar un mensaje más amplio

```html
<form>
  <label for="area">Mensaje</label>
  <textarea name="" id="area" cols="30" rows="10"></textarea>
</form>
```

Para enviar los formularios se puede hacer de dos maneras, haciendo uso de un `input` de tipo `submit` o de un `button` también de tipo `submit`, ambos deben estar dentro del formulario y generalmente en la parte final del mismo.

```html
<form>
  <input type="submit" value="Enviar Formulario" />

  <button type="submit">Enviar formulario</button>
</form>
```

## Etiquetas para enlaces

```html
<a href="...">Texto del enlace</a>
Se le pueden agregar atributos a esta etiqueta, como target=”_blank” para abrir
en enlace en una nueva pestaña. También se puede dirigir en una siglepage a un
título de una sección desde el menú

<a href="#quehacemos">Que hacemos</a>
Lo que redirigirá a:
<h2 id="quehacemos">Que hacemos</h2>

Se pueden incrustar enlaces en imágenes y descargar las mismas desde un enlace.

<a href="enlace"> <img src="..." alt="..." /> </a>
<p><a href="enlace" download="enlace">Descargar imagen</a></p>
```

## Etiquetas semánticas

Estas ayudan a mejorar la indexación a los buscadores describiendo cada apartado de la página de manera más especifica

`<header></header>` Esta etiqueta funciona para los encabezados del sitio, puede ir como cabecera de la página, de un artículo, de una tarjeta etc.

`<footer></footer>` Esta etiqueta funciona para poner el pie de una sección dentro de la página o la misma página en sí.

Tanto el header como el footer pueden estar presentes varias veces dentro de la página web, dependiendo de los componentes o secciones de ésta, la única condición es que una etiqueta no puede anidar a otra del mismo tipo.

```html
Uso correcto de las etiquetas:

<header>
  <h1>Título</h1>
</header>

<header>
  <h2>...</h2>
</header>

<footer>...</footer>

Uso incorrecto de las etiquetas:

<header>
  <header>
    <h1>Título</h1>
  </header>
</header>
```

`<nav></nav>` Esta etiqueta se utiliza para encapsular el menú de navegación (el cuál generalmente va en el header).

```html
<nav>
  <ul>
    <li>
      <a href="home.html">Home</a>
    </li>
  </ul>

  <ul>
    <li>
      <a href="about.html">About</a>
    </li>
  </ul>

  <ul>
    <li>
      <a href="contact.html">Contact</a>
    </li>
  </ul>
</nav>
```

`<article></article>` Estas etiquetas pueden ir más allá de blogs o artículos de noticias, ya que se usan para relacionar contenido de una sección de la página cuando se involucre un encabezado en dicha sección. Ejemplo:

```html
<article>
  <header>
    <h3>Related Content</h3>
  </header>

  <ul>
    <li>
      <a href="#">Another Article</a>
    </li>

    <li>
      <a href="#">A Web Page</a>
    </li>

    <li>
      <a href="#">Interesting Link</a>
    </li>
  </ul>
</article>
```

`<section></section>` Para dividir el sitio en secciones, es utilizado para el contenido en sí mismo

```html
<article>
  <section>
    <header>
      <h2>An Article</h2>
    </header>

    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Fugit laboriosam
      nesciunt totam enim accusamus eum suscipit impedit sequi, quidem,
      consequuntur illum excepturi dolorum molestiae, porro natus a. Eaque,
      facilis eum!
    </p>

    <img src="assets/ultimate.svg" alt="The pink U logo for Ultimate Courses" />
  </section>

  <section>
    <h3>Article Subheading</h3>

    <p>
      Lorem ipsum, dolor sit amet consectetur adipisicing elit. Veritatis,
      voluptas rerum? Neque officiis accusamus blanditiis quae nemo animi
      assumenda ipsum.
    </p>
  </section>

  <footer>
    <p>Written by Ángel Cruz</p>
  </footer>
</article>
```

El siguiente código se utiliza para activar la accesibilidad dentro de un elemento section

```html
<section aria-labelledby="youtube-title">
  <header>
    <h2 id="youtube-title" class="section__title">Youtube</h2>
    <p class="section__subtitle">learn for free</p>
  </header>
</section>
```

`<main></main>` Contenido principal de la página (solo un elemeno por página).

`<aside></aside>` Para contenidos generalmente relacionados con el contenido principal de la página pero sin ser parte de el, generalmente utilizado en blogs para contenidos o barras laterales.

```html
<!DOCTYPE html>
<html lang="en">
  <head> </head>
  <body>
    <header>...</header>

    <main>
      <article>...</article>
    </main>

    <aside>
      <article>...</article>
    </aside>

    <footer>Made by Ángel Cruz - 2021</footer>
  </body>
</html>
```

`<figure></figure>` Esta etiqueta sirve para el contenido autónomo, como ilustraciones, diagramas, fotos, listas de códigos, videos, etc.

`<figcaption></figcaption>` Funciona para colocar un título de la imágen.

```html
<figure>
  <img src="..." alt="..." />
  <figcaption>Título de la imágen</figcaption>
</figure>
```

Un ejemplo de una correcta implementación de estas etiquetas bajo un apartado semántico esta en la página de [MDN](https://developer.mozilla.org/en-US/)

![[semantic-html.png]]

Existen otras [etiquetas semánticas](https://developer.mozilla.org/en-US/docs/Glossary/Semantics), como:

- time → Representa un formato de tiempo, se puede utilizar el atributo `datetime` para convertir el formato de fecha a uno legible.

```html
<p>
  The Cure will be celebrating their 40th anniversary on
  <time datetime="2018-07-07">July 7</time> in London's Hyde Park.
</p>

<p>
  The concert starts at <time datetime="20:00">20:00</time> and you'll be able
  to enjoy the band for at least <time datetime="PT2H30M">2h 30m</time>.
</p>

<p>The concert starts at <time datetime="2018-07-07T20:00:00">20:00</time>.</p>
```

## Etiquetas multimedia

Para el vídeo se puede utilizar la siguiente estructura:

```html
<video controls loop poster="<route>">
  <source src="..." type="video/mp4" />
  <source src="..." type="video/webm" />
</video>
```

Dicha estructura permite agregar video con su respectivo reproductor, se pueden agregar más atributos después de controls como width o height, loop, autoplay o poster, siendo esta última la imagen de la previsualización.

Soporta archivos mp4, webM, ogg

Para el audio se puede usar lo siguiente:

```html
<audio controls loop autoplay>
  <source src="..." type="video/wav" />
  <source src="..." type="video/mpeg" />
</audio>
```

Esto permite agregar audio y mostrar el control del reproductor, de no ser mostrado el control no se verá nada, pues el audio no contiene información visual.

Solo soporta archivos .mp3 .ogg y .wav

Para mejorar el uso de las imagenes se puede hacer dentro de la etiqueta `picture`, esta etiqueta ofrece diferentes opciones de imágenes para que el navegador decida cual es la más óptima.

```html
<picture>
  <source srcset="..." media="(min-width: 800px)" type="image/png" />
  <img src="..." alt="" />
</picture>
```

Por último esta la etiqueta de `<iframe></iframe>`, la cual sirve para insertar o incrustar contenido de otros sitios web o servidores a nuestra página como videos de youtube o alguna otra plataforma.

## **Etiquetas varias**

`<abbr></abbr>` Esta etiqueta sirva para poder agregar un texto descriptivo a un acronimo.

```html
<p>
  <abbr title="Hypertext Markup Language">HTML</abbr> es un lenguaje de marcado
</p>
```

La etiqueta `<bdo></bdo>` te permite especificar la dirección de un texto.

```html
<bdo dir="rtl">Esto es un texto invertido</bdo>
```

La etiqueta `<kbd></kbd>` sirve para estilizar el contenido y que este se asemeje a una tecla.

```html
<p>Presionar <kbd>Win</kbd> + .</p>
```

La etiqueta `<samp></samp>` sirve para estilizar el contenido y que este se asemeje a el output de una computadora, pues es una fuente mono-espaciada.

```html
<samp>Esto es una fuente monoespaciada</samp>
```

La etiqueta `<details></details>` sirve para contraer y expandir contenido.

```html
<details>
  <summary>Esto se muestra sin contraer los detalles</summary>
  Esto ya no 😲
</details>
```

# **Accesibilidad**

La accesibilidad ayuda a personas con algún tipo de discapacidad van a poder hacer uso de la Web.

Esto ayuda a describir que es lo que hacen los elementos, por ejemplo dentro de un link:

# **Performance**

Existen distintas estrategías que pueden mejorar y optimizar el rendimiento de los sitios web, esto se hace con la finalidad de que estos sitios sean más rápidos, o no requieran tanto procesamiento.

## L**azy Loading**

El lazy loading o la carga perezosa ayuda al performance de la página, pues los elementos se cargarán solo de ser necesarios dentro de la página, así estos no cargan desde el inicio.

Para poder implementar el lazy loading en imágenes se utiliza el siguiente atributo dentro de la etiqueta `img`:

```html
<img loading="lazy" />
```

Este atributo no solo funciona con las etiquetas `img`, pues también se puede implementar para imágenes con `srcset`, `picture` y en `iframes`.

```html
<!-- Lazy-loading en imágenes con picture. Se implementa dentro de <img> como fallback. -->
<picture>
  <source
    media="(min-width: 40em)"
    srcset="img-big.jpg 1x, img-big-hd.jpg 2x"
  />
  <source srcset="img-small.jpg 1x, img-small-hd.jpg 2x" />
  <img src="img-fallback.jpg" loading="lazy" />
</picture>

<!-- Lazy-loading en imágenes que tienen un srcset -->
<img
  src="small.jpg"
  srcset="img-large.jpg 1024w, img-medium.jpg 640w, img-small.jpg 320w"
  sizes="(min-width: 36em) 33.3vw, 100vw"
  loading="lazy"
/>

<!-- Lazy-loading en iframes  -->
<iframe src="video-player.html" loading="lazy"></iframe>
```

## **Preload**

Esta técnica consiste en cargar de manera prioritaria ciertos elementos, los cuales se cree son indispensables para el funcionamiento de la página web, es por ello que deben priorizarse, ya que de otra manera estos elementos serán cargados hasta el momento que sean necesitados según la jerarquización en el documento HTML.

Para poder aplicar la técnica del preload se necesita realizar lo siguiente:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Preload -->
    <link rel="preload" href="css/normalize.css" as="style" />
    <link rel="stylesheet" href="css/normalize.css" />

    <link
      rel="preload"
      href="<https://fonts.googleapis.com/css2?family=Open+Sans&family=PT+Sans:wght@400;700&display=swap>"
      crossorigin="crossorigin"
      as="font"
    />
    <link
      href="<https://fonts.googleapis.com/css2?family=Open+Sans&family=PT+Sans:wght@400;700&display=swap>"
      rel="stylesheet"
    />

    <link rel="preload" href="css/style.css" as="style" />
    <link rel="stylesheet" href="css/style.css" />
  </head>
  <body></body>
</html>
```

## **Prefetch**

Prefetch es una técnica encargada de cargar la siguiente pagina que esperamos que el usuario visite para dar una mejor experiencia de usuario y agilizar la navegación dentro del sitio.

Para poder aplicar la técnica del prefetch se necesita realizar lo siguiente:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Prefetch -->
    <link rel="prefetch" href="nosotros.html" as="document" />
  </head>
  <body></body>
</html>
```

Puedes determinar la página a la que le aplicarás el prefetch con ayuda de herramientas como Google analytics, así pudiendo determinar cuál es la página de tu sitio en el que los usuarios pasan más tiempo.

## **Imágenes webp**

Estas son imágenes con formato .webp, este formato ayuda a que las imágenes pierdan peso sin perder calidad.

```html
<picture>
  <source loading="lazy" srcset="img/blog1.webp" type="image/webp" />
  <img loading="lazy" src="img/blog1.jpg" alt="imagen blog" />
</picture>
```

Es importante colocar una imagen en caso de que el navegador no soporte este tipo de formatos, de esta manera podrá acceder a la imagen original.
