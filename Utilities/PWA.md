---
tags: frontend utilities
---
----

# ¿Qué son las PWA?



# Creación de una PWA

Uno de los archivos necesarios para poder crear una Progressive Web Application es `manifest.json` , este archivo contendrá los datos de la aplicación.

```json
{
  "name": "APV ",
  "short_name": "APV",
  "start_url": "/index.html",
  "display": "standalone", 
  "background_color": "#D41872",
  "theme_color": "#D41872",
  "orientation": "portrait-primary",
  "icons": [
    {
      "src": "img/icons/icon-72",
      "type": "image/png",
      "sizes": "72x72"
    },
    {
      "src": "img/icons/icon-120.png",
      "type": "image/png",
      "sizes": "120x120"
    },
    {
      "src": "img/icons/icon-128.png",
      "type": "image/png",
      "sizes": "128x128"
    },
    {
      "src": "img/icons/icon-144.png",
      "type": "image/png",
      "sizes": "144x144"
    },
    {
      "src": "img/icons/icon-152.png",
      "type": "image/png",
      "sizes": "152x152"
    },
    {
      "src": "img/icons/icon-196.png",
      "type": "image/png",
      "sizes": "196x196",
			"purpose": "any maskable"
    },
    {
      "src": "img/icons/icon-256.png",
      "type": "image/png",
      "sizes": "256x256"
    },
    {
      "src": "img/icons/icon-512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ]
}
```

Después de haber creado este `manifest.json` lo que se hace es agragarlo al html en la parte del head de la siguiente manera:

```html
<link rel="manifest" href="manifest.json">
```

Con esto se registrará el manifest dentro de la app, sin embargo aún falta registrar el service worker.

Para poder instalar el service worker se hace lo siguiente en un archivo `sw.js`

```jsx
const nombreCache = 'apv-cache'; // Nombre del cache...
const assets = [
  '/',
  '/index.html',
  '/error.html',
  '/js/app.js',
  '/css/bootstrap.css',
  '/css/styles.css',
];

// console.log(self);

// Instalar el service Worker, no se puede utilizar window, por lo tanto se utiliza self
self.addEventListener('install', e => {
  console.log('Instalado el Service Worker');

  e.waitUntil(
    // Buen lugar para cachear -
    caches.open(nombreCache).then(cache => {
      // Esta función es asincrona...
      console.log('cacheando...');
      cache.addAll(assets);
    })
  );

  // Revisar en App (Chrome) en Firefox en Almacenamiento
});

// Activar el service worker...
self.addEventListener('activate', e => {
  console.log('Service Worker Activado');

  // Actualizar la PWA //
  e.waitUntil(
    caches.keys().then(keys => {
      console.log(keys);

      return Promise.all(
        keys.filter(key => key !== nombreCache).map(key => caches.delete(key)) // borrar los demás
      );
    })
  );
});

// Fetch events para el CSS, HTML, imagenes JS, y hasta llamados a fetch..
self.addEventListener('fetch', e => {
  console.log('Fetch.. ', e);

  e.respondWith(
    caches
      .match(e.request)
      .then(respuestaCache => {
        return respuestaCache || fetch(e.request);
      })
      .catch(() => caches.match('/error.html'))
  );
});

// Al tener este fetch, un nombre de app, y una página de inicio, ya podremos agregar nuestra PWA a la página de inicio...

// clICK EN LOS 3 PUNTOS

//  More Tools -> Remote Devices y escribir.

// <http://127.0.0.1:5500/index.html>

// Presionar Open.
```

---

---

Para poder enviar notificaciones a los usuarios por medio de js estos deben de haber aceptado recibirlas

Un set permite crear una lista de valores sin duplicados (add size has delete, clear, forEach)

Set cualquier valor, weakset solo objetos y no puedes conocer su extension con .size