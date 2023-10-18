---
tags: framework, frontend
---
----

[Documentación](https://angular.io/docs)

----

# ¿Qué es Angular?

Angular es un framework de JavaScript el cual permite crear SPA (Single Page Applications), las cuales son aplicaciones que son traídas una ves desde el servidor y de ahí se ejecutan en el lado del cliente (su navegador).


# Creación de un proyecto de Angular

Para poder crear un nuevo proyecto de angular se debe tener instalado [NodeJS](https://nodejs.org/en/) y la interfaz de línea de comandos de Angular.
```bash
npm install -g @angular/cli
```

Ya teniendo ambos instalados se ejecuta el siguiente comando:
```bash
ng new <nombre del proyecto>
```

Estando dentro del directorio de la aplicación de Angular se ejecuta el siguiente comando para iniciar el servidor de desarrollo en el puerto 4200: 
```bash
ng serve -o
```


# Componentes

Angular funciona gracias a componentes, los componentes son bloques de código re-utilizables, estos componentes constan de tres archivos, un template (HTML), un archivo de estilos (CSS) y un archivo de TypeScript.

La manera de trabajar de Angular es inyectar la aplicación en una etiqueta `<app-root></app-root>` que esta contenida en el archivo `index.html`.

Para poder crear un componente se puede utilizar la siguiente instrucción en la consola:
```bash
ng g c components/home
```
Este comando sirve para generar un componente, este será colocado en la carpeta components con el nombre de `home`.

```bash
ng g c components/search -is
```
Genera el componente search sin hoja de estilos

```bash
ng g c components/navbar -is --skipTests
```
Genera el componente navbar sin hoja de estilos ni archivo de pruebas



## Ciclo de vida de un componente

| **Componente**            | **Ejecución**                                              |
| --------------------- | ------------------------------------------------------ |
| ngOnInit              | Después de que el componente sea inicializado          |
| ngOnChanges           | Cuando la data de propiedades relacionadas cambian     |
| ngDoCheck             | Durante cada revisión del ciclo de detección de cambio |
| ngAfterContentInit    | Después de insertar contenido                          |
| ngAfterContentChecked | Después de la revisión del contenido insertado         |
| ngAfterViewInit       | Después de la inicialización del componente/hijos      |
| ngAfterViewChecked    | Después de cada revisión de los componentes/hijos      |
| ngOnDestroy           | Justo antes que se destruya el componente o directiva  |



# Módulos

Un modulo es una clase con el decorador NgModule, cuyo propósito es organizar la aplicación en bloques

La propiedad bootstrap recibe un array cuyo valor por defecto es el AppComponent, este componente sera el root de nuestra aplicación de Angular por lo que siempre debe haber un componente.

Dentro de la propiedad declarations deben ir componentes, directivas o pipes, cuando estos estén declarados solo funcionaran al nivel de ese modulo, por lo que si se requiere en algún otro modulo deben ser re-declarados.

Para la propiedad de los imports todo modulo que este dentro de ese array estará disponible para todos los componentes y directivas de la aplicación.

En los exports se exportan los módulos, componentes, directivas o pipes que se van utilizar en alguna otra parte de la aplicación.

En la propiedad de los providers se declaran los servicios, los cuales serán disponibles para toda la aplicación (para un shared module no es necesario utilizar la propiedad provider).

Un componente es un template, una clase y metadata, el template es la vista creada con html, esta incluye el binding y directivas, la clase es creada con typescript y esta contiene toda la parte lógica del componente, la metadata contiene información extra para angular y esta definida por un decorador


```ts
// La metadata seria:

@component({
  selector:'app-vida',
  template:`<h1>Mi primer componente</h1>`,
  styleUrls:["./vida.component.css"]
})
```

```powershell
ng g m components/home
```
Sirve para generar un módulo, este será colocado en la carpeta components con el nombre de home


Adicionalmente se puede agregar la bandera --flat la cual sirve para crear un módulo en la raíz sin crear una carpeta:
```powershell
ng g m components/home --flat
```

Para un buen orden de los módulos en una aplicación de angular se recomienda crear primero los módulos y después los componentes, de esta manera la aplicación sera modularizada, lo que mejora la arquitectura de la información y hace mas re-utilizables los componentes.


```bash
ng g m modules/home --route home -m app.module
```










We don't put domain objects in DTO's, we need to create a mapping from the domain object and use it in DTO's


# Interceptors

Los interceptors inyectan propiedades en las peticiones HTTP