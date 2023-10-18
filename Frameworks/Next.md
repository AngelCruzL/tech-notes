---
tags: framework
---
----

[Documentación]()

----

# ¿Qué es

# getStaticProps

Se ejecuta únicamente del lado del servidor y se ejecuta solo en build time una única vez. Solo se puede utilizar dentro de las páginas.
Nada de eso llega al cliente con excepción de las props.

Se suele utilizar cuando se sabe de antemano que esos parámetros son necesarios para el funcionamiento de esa página.



Se recomienda manejar el SSG (Static Site Generation) hasta donde sea posible, en ocasiones esto no solucionará la necesidad por lo que usaremos SSR (Server Side Rendering)
ISR (Incremental Static Regeneration) se utiliza si existen cambios que no suceden tan frecuentemente, cuando se realice una solicitud después de cierto tiempo next va a regenerar el contenido de la página, de modo que las próximas solicitudes siempre tengan el mismo contenido.


# getStaticPaths

Se utiliza para páginas que contienen un argumento dinámico. 


Las cookies viajan bajo request del usuario hacia el servidor, aunque tiene menor capacidad que el localStorage


# ISR
[Data Fetching: Incremental Static Regeneration | Next.js (nextjs.org)](https://nextjs.org/docs/pages/building-your-application/data-fetching/incremental-static-regeneration)

Esta funcionalidad te permite que de manera incremental puedas crear o actualizar páginas creadas anteriormente, para activar esta opción necesitas que el return de la función `getStaticProps` tenga la propiedad `revalidate: <tiempo en segundos>`


# ISG

La Incremental Static Generation permite crear una página en file system justo después de que se haya solicitado por primera vez, esto en caso de que dicha página no forme parte del build inicial, de tal manera que al ser solicitada por una segunda ocasión ahora el servidor responderá con el archivo que se genero en la primera petición.

# SSR

El server side rendering permite crear páginas del lado de servidor ante cada solicitud, esto quiere decir que cuando se visite una url especifica en ese momento el servidor se encargará de crear la página, pues esta no se almacena en file system.

# getInitialProps

No es tan recomendable