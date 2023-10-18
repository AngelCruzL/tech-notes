---
tags: framework
---

---

[Documentación](https://react.dev)

---

# ¿Qué es React?

[React](https://react.dev/) es una librería de interfaces de usuario creada por Facebook basada en componentes.

Un componente es una pequeña pieza de código encapsulada re-utilizable, esto permite agilizar el desarrollo de aplicaciones web.

## Entorno de desarrollo

Para poder utilizar React necesitamos [nodejs](https://nodejs.org) para poder ejecutar ciertos comandos, para poder confirmar la instalación de node en el equipo podemos usar el siguiente comandoÑ

```sh
node --version
```

Una vez teniendo instalado node en el equipo necesitamos un editor de texto o IDE para poder escribir las aplicaciones, personalmente recomiendo [VSCode](https://code.visualstudio.com/download), ya que este es gratuito y puede ser mejorado a traves de sus pluggins, de entre los cuales recomiendo los siguientes:

- [ES7 React/Redux](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

- [Simple React Snippets](https://marketplace.visualstudio.com/items?itemName=burkeholland.simple-react-snippets)

- [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

Ademas de esto necesitamos un navegador y ciertas herramientas de desarrollo para poder facilitar el desarrollo de aplicaciones de React, entre estas herramientas recomiendo:

- [Google Chrome](https://www.google.com/chrome/)

- [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=es&authuser=1)

- [Redux Devtools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=es)

El **useEffect** retorna una función cuando el componente se desmonta (deja de existir) para realizar una limpieza. En caso de no depender de alguna variable o proceso el useEffect se disparará solo la primera vez que cargue el componente.

El context es similar a Redux, sin embargo se trata de un hook.

El hook de **useLayoutEffect** se utiliza de manera similar al useEffect con la diferencia de que el primero se dispara de forma asincrona después de todas las mutaciones de DOM

React.memo (o solo memo) redibuja el componente solo en caso de que los props cambien.

Los reducers deben retornar un nuevo estado, generalmente reciben dos argumentos (estado y acción)

Al momento de implementar mas de un router en la aplicación se debe tomar en cuenta que solo se utiliza un BrowserRouter, en este caso se implementa en el router de mayor nivel.

Al utilizar el hook de `useNavigate` se puede hacer de la siguiente manera:

```jsx
const navigate = useNavigate();

const callbackFunction = () => {
  navigate('/route', { replace: true });
};
```

En este caso la opción `replace: true` ejecuta una nueva instancia del navegador y retorna a aquella que estaba detrás de la pantalla actual.

useRef es un hook que te permite referenciar a un elemento html, en caso de que este elemento sea un functional component esto cambia, pues en vez de utilizar `useRef` se debe utilizar lo siguiente:

```jsx
import { useRef, useImperativeHandle, forwardRef } from 'react';

// useImperativeHandle is a function that receives two params, the second is a funtion
// who returns a object with the data that will be accessible from the outside of the
// component, and the first will be the ref

export default forwardRef(function Input(props, ref){
  const inputRef = useRef();

  const active = () => {
    inputRef.current.focus();
  };

  useImperativeHandle(ref, () => {
    return {
      focus: active
    };
  };

  return (
    // Some JSX code...
  )
});
```

# Testing

## Setup Enzyme

Para ejecutar los test en react sobre la version 17 se deben instalar las siguientes dependencias

```bash
npm install --save-dev enzyme @wojtekmaj/enzyme-adapter-react-17 enzyme-to-json
```

```bash
yarn add --dev enzyme @wojtekmaj/enzyme-adapter-react-17 enzyme-to-json
```

En caso de querer probar los hooks se debe agregar la siguiente dependencia:

```bash
# if you're using npm
npm install --save-dev @testing-library/react-hooks
# if you're using yarn
yarn add --dev @testing-library/react-hooks
```

Para configurar las bibliotecas en el proyecto se debe colocar lo siguiente en el archivo `src/setupTests.js`

```jsx
import Enzyme from 'enzyme';
import Adapter from '@wojtekmaj/enzyme-adapter-react-17';
import { createSerializer } from 'enzyme-to-json';

Enzyme.configure({ adapter: new Adapter() });

expect.addSnapshotSerializer(createSerializer({ mode: 'deep' }));
```

el `mount` de enzyme se utiliza cuando se quiere probar toda la aplicación en contexto

## Setup Testing Library
