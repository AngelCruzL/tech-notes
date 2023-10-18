---
tags: utilities, frontend, react, javascript
---
----

[Home v6.14.1 | React Router](https://reactrouter.com/en/main)

----

# ¿Qué es React Router DOM?


# Configuración

Después de iniciar el proyecto instalamos la librería

```bash
yarn add react-router-dom -E
```

Después de la instalación podemos proveer las rutas de distintas maneras, my favorite approach is with this syntax:

```jsx
import {createBrowserRouter, RouterProvider} from "react-router-dom";

import HomePage from "./pages/Home.jsx";
import ProductsPage from "./pages/Products.jsx";

const router = createBrowserRouter([
  {path: '/', element: <HomePage/>},
  {path: '/products', element: <ProductsPage/>}
])

const App = () => <RouterProvider router={router}/>

export default App
```

On this case we use the `createBrowserRouter` to generate a router, and we use the `RouterProvider` component to provide this router to our application. As I comment previously this is my favorite approach, but only works on >6.3 version, the most common implementation is the old one:

```jsx
import {createBrowserRouter, createRoutesFromElements, Route, RouterProvider} from "react-router-dom";

import HomePage from "./pages/Home.jsx";
import ProductsPage from "./pages/Products.jsx";

const routeDefinitions = createRoutesFromElements(
  <Route>
    <Route path="/" element={<HomePage/>}/>,
    <Route path="/products" element={<ProductsPage/>}/>,
  </Route>
)

const router = createBrowserRouter(routeDefinitions)

const App = () => <RouterProvider router={router}/>

export default App
```

# Navegación entre componentes

Esto se logra gracias al componente `Link`, el cual funciona como un anchor tag pero únicamente dentro de la misma aplicación, permitiéndonos una navegación entre los componentes sin la necesidad de recargar la página.

# Layouts

Podemos implementar layouts basados en las rutas, en este caso me basare en mi implementación preferida (la primera que se mostró):

```jsx
import {createBrowserRouter, RouterProvider} from "react-router-dom";

import HomePage from "./pages/Home.jsx";
import ProductsPage from "./pages/Products.jsx";
import ErrorPage from "./pages/Error.jsx";
import RootLayout from "../Root.jsx";

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout/>,
		errorElement: <ErrorPage/>,
    children: [
      {path: '/', element: <HomePage/>},
      {path: '/products', element: <ProductsPage/>}
    ]
  },
])

const App = () => <RouterProvider router={router}/>

export default App
```

En este caso se renderizará como común denominador el componente `<RootLayout/>`, el cual tiene el siguiente contenido:

```jsx
import {Outlet} from "react-router-dom";

import MainNavigation from "./src/components/MainNavigation.jsx";

const RootLayout = () => {
  return (
    <>
      <MainNavigation/>
      <Outlet/>
    </>
  );
};

export default RootLayout;
```

Este componente carga el componente `<MainNavigation/>`, y a su vez utiliza el componente `<Outlet/>` para renderizar el contenido de las rutas hijas.

## Active state

React router dom nos permite determinar el estilo de una clase si es que esta se encuentra activa o no, para ello utilizamos el componente `<NavLink/>` de la siguiente manera:

```jsx
<li>
	<NavLink 
		to='/'
		className={({isActive}) => isActive ? 'active' : ''}
		end
	>
		Home
	</NavLink>
</li>
```

# Navigate programmaticaly

```jsx
import {useNavigate} from 'react-router-dom';

const ProductsPage = () => {
  const navigate = useNavigate();

  const navigateHandler = () => {
    navigate('/');
  }

  return (
    <>
      <h1>Products!</h1>
      <button onClick={navigateHandler}>Go to HomePage</button>
    </>
  );
};

export default ProductsPage;
```

# Navigate with params

```jsx
import {createBrowserRouter, RouterProvider} from "react-router-dom";

import RootLayout from "../Root.jsx";
import HomePage from "./pages/Home.jsx";
import ProductsPage from "./pages/Products.jsx";
import ProductDetailPage from "./pages/ProductDetail.jsx";

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout/>,
    children: [
      {path: '/', element: <HomePage/>},
      {path: '/products', element: <ProductsPage/>},
      {path: '/products/:productId', element: <ProductDetailPage/>}
    ]
  },
])

const App = () => <RouterProvider router={router}/>

export default App
```

```jsx
import {useParams} from "react-router-dom";

const ProductDetailPage = () => {
  const params = useParams();

  return (
    <h1>{params.productId}</h1>
  );
};

export default ProductDetailPage;
```

# Relative routes

Es posible agregar rutas relativas

```jsx
import {Link, useParams} from "react-router-dom";

const ProductDetailPage = () => {
  const params = useParams();

  return (
    <>
      <h1>{params.productId}</h1>
      <p><Link to='..' relative='path'>Back</Link></p>
    </>
  );
};

export default ProductDetailPage;
```

En este caso se utiliza `..` como wildcard y se establece el `relative='path'` (por defecto es `route`), para poder hacer esta ruta relativa al path de la url en vez de a la de la ruta, de esta manera regresa a `products`, en vez de a `/`

# Index routes

```jsx
import {createBrowserRouter, RouterProvider} from "react-router-dom";

import RootLayout from "../Root.jsx";
import HomePage from "./pages/Home.jsx";
import ProductsPage from "./pages/Products.jsx";
import ProductDetailPage from "./pages/ProductDetail.jsx";

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout/>,
    children: [
      {index: true, element: <HomePage/>},
      {path: '/products', element: <ProductsPage/>},
      {path: '/products/:productId', element: <ProductDetailPage/>}
    ]
  },
])

const App = () => <RouterProvider router={router}/>

export default App
```