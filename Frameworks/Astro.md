---
tags: framework
---
----

[Getting Started 🚀 Astro Documentation](https://docs.astro.build/en/getting-started/)

----

# ¿Qué es Astro?

Astro es un generador de sitios estáticos (aunque podemos agregarle interactividad). Esto quiere decir que solo muestra información sin permitir grandes interacciones con el usuario, por ejemplo:
- blogs
- portfolios
- landing page

Astro a diferencia de frameworks como [[Angular]], [[React]] o [[Vue]] se maneja como un MPA (Multiple Page Application), esto quiere decir que tendremos distintas páginas, asemejandose más a template engines.

La razón de este comportamiento es debido a que busca solucionar el problema que se presenta con las SPA respecto al tema de indexación, pues como bien sabemos en una SPA la aplicación como tal esta encapsulada dentro de javascript, esto quiere decir que al momento de indexarse no se cuenta con mucha información del sitio y esto genera que no se indexe correctamente, lo cual puede verse reflejado en el poco tráfico del sitio web.

Gracias al enfoque que tiene Astro se puede mejorar el SEO, esto facilita la indexación dentro de motores de búsqueda y se traduce en que pueda generar más tráfico el sitio como tal.

Además de esto mejora el rendimiento del sitio ya que la carga inicial es más rápida al procesarse desde el servidor.

Astro se basa en islas (componentes) al igual que los frameworks previamente mencionados, estos se cargan de manera aislada mejorando así el rendimiento de la aplicación.

## Interactive Island

Las islas interactivas son componentes que contienen interacción dentro de astro, de hecho es posible agregar componentes de frameworks como [[React]], [[Vue]] y [[Svelte]] dentro de las páginas diseñadas en Astro.

# Iniciando con Astro

Para iniciar un proyecto en Astro necesitamos ejecutar el siguiente comando
```bash
yarn create astro
```

El proyecto vacío de astro contiene dentro de la capeta source un index dentro de las páginas.


# Trabajando con otros frameworks

Para añadir un framework dentro de astro ejecutamos el siguiente comando:
```bash
yarn astro add <framework>
```

Por ejemplo para agregar React podemos utilizar:
```bash
yarn astro add react
```

Para utilizar svelte dentro de un proyecto de Astro necesitamos ejecutar el siguiente comando:
```bash
yarn astro add svelte
```

Esto también funciona para agregar frameworks de estilos como tailwind css con el siguiente comando:
```bash
yarn astro add tailwind
```

```bash
<Short client:load />
```


# Content Collections

Content collections permite agrupar las colecciones dentro de un mismo directorio (refiriendose a los archivos markdown).

Estos archivos se almacenan dentro de un directorio llamado content, dentro de este directorio los subdirectorios representaran una colección.

Dentro del archivo `config.ts` realizaremos las configuraciones de los esquemas de las colecciones, este archivo debe estar ubicado dentro del directorio content.

Por defecto para trabajar con estos schemas utilizamos zod, aunque para evitar errores de tipado necesitamos cambiar la referencia de las definiciones dentro del archivo

```tsx
/// <reference types="astro/client" />
/// <reference path="../.astro/types.d.ts" />
```

Ahora esto permitirá modificar los data types con los nuevos campos del content collections. Ahora dentro del archivo de config.ts colocamos el siguiente contenido:

```tsx
import {defineCollection, z} from 'astro:content'

const blog = defineCollection({
  schema: z.object({
      title: z.string().max(50, {message: 'Title must be less than 50 characters'}),
      description: z.string(),
      date: z.date(),
      active: z.boolean(),
      author: z.string(),
      tags: z.array(z.string()),
      image: z.string().url(),
      imageAlt: z.string(),
      category: z.string(),
    }
  )
})

export const collections = {blog}
```

Ahora dentro del index utilizamos el siguiente código para poder recuperar el contenido de estas colecciones:

```tsx
import {getCollection} from 'astro:content'

const blogPosts = await getCollection('blog')
```

Y podemos realizar la iteración de este contenido de manera normal:

```tsx
<ul role="list" class="link-card-grid">
  {
    blogPosts.map(({data: post}) => (
      <Card
        title={post.title}
        body={post.description}
        image={post.image}
      />
    ))
  }
</ul>
```

Para poder obtener un post especifico, ya sea de otra colección o de la misma pero cargado de manera individual:

```tsx
import {getCollection, getEntryBySlug} from 'astro:content'

const post1 = await getEntryBySlug('blog', 'post1');

<p class="instructions">
  <strong>Post destacado:</strong> {post1.data.title}
</p>
```

Podemos realizar consultas para cargar solo algún blog en especifico:

```tsx
import {getCollection, getEntryBySlug} from 'astro:content'

const blogPosts = await getCollection('blog', ({data}) => {
  return data.category === 'ts'
})
```