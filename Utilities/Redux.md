---
tags: utilities
---
----

[Getting Started with Redux | Redux](https://redux.js.org/introduction/getting-started)

----

# ¿Qué es redux?

Es un patron para el manejo de la información, este notifica cual es el estado de la aplicación, como cambio la información, quien cambio alguna variable y cual es el estado de estas variables

- Toda la data de la información se encuentra en una estructura previamente definida
- Toda la información se encuentra almacenada en un único lugar (store)
- El store jamas se modifica de forma directa
- Las interacciones entre usuario y/o código disparan acciones que describen lo que sucedió
- El valor actual de la información se la aplicación se llama state
- Un nuevo estado es creado en base a la combinación del viejo estado y la acción por una función llamada reducer

La acción es la única fuente de información que se envía por interacciones de usuario programa, por lo general se busca que estas acciones sean lo mas simples posibles

Una acción tiene unicamente dos propiedades

- type → Es el tipo de tarea a realizar o describe la acción que se quiere hacer
- payload → Es la cantidad minima de información para realizar dicha tarea (opcional)

El reducer es una función que recibe dos argumentos `(oldState, action)`, esta función siempre debe retornar un estado

Donde oldState es el estado actual de la aplicación y el action es un objeto plano que indica aquello que se debe de hacer

El stage - storage es el lugar donde se guardara la información

- El state es solo de lectura
- Nunca se mutara el state se forma directa
- Hay funciones prohibidas de javascript como push y todas aquellas que modifiquen de manera directa el objeto oldState

El store es un objeto que tiene las siguientes responsabilidades

- Contiene el estado de la aplicación
- Permite el estado de la lectura del estado via: `getState()`
- Permite crear un nuevo estado utilizando `dispatch(ACTION)`
- Permite notificar cambios de estado via `subscribe()`

En resumen las acciones describen que es lo que vamos a hacer, el reducer toma un estado anterior y una acción para producir un nuevo estado, el state es como se encuentra la información actualmente y el store se encarga de realizar la gestión de la aplicación

---

Redux necesita tres cosas para poder funcionar, la primera es un lugar para poner la data llamado store, también es necesario que mínimo cuente con un reducer (este decide que hacer con la data), la ultima son las actions que indican las acciones que se necesitan

Una acción trae la data de una api, después esa acción se los entrega al reducer, el cual decide como manejar esa data encargandose asi el reducer de modificar el store para que el store sea el encargado de almacenar esa data

De esta manera toda la data estará disponible a lo largo de la aplicación siempre y cuando se comuniquen por medio de acciones

Los ducks son patrones de redux en el cual los reducers, el store y las actions se encuentran en el mismo archivo

Un reducer se dedica solo a una parte del store

```jsx
import {createStore,combineReducers,compose} from 'redux';

createStore          Se encarga de crear el store
combineReducers      Combina diferentes reducers que provienen de diferentes ducks
compose              Instala las herramientas de debuggin
applyMiddleware      Aplicaciones extra que se requiere que soporte el store
```

A la función de create store se le mandan tres parametros, el reducer, la data inicial y los middlewares

thunk sirve para promesas, para poder hacer consumos al backend

dispatch ejecuta las acciones y getState entrega el store

A la hora de implementar redux en la HomePage se importa connect, esto es una función que devuelve una función, connect se puede usar de dos formas distintas, para poder pedir datos que a tine el reducer en store, la otra es para despachar acciones distintas

```jsx
Sacandole los datos a REDUX

import React, { useState, useEffect } from "react";
import { connect } from "react-redux";

import Card from "../card/Card";
import styles from "./home.module.css";

function Home({ chars }) {
  function renderCharacter() {
    let char = chars[0];
    return <Card {...char} />;
  }

  return (
    <div className={styles.container}>
      <h2>Personajes de Rick y Morty</h2>
      <div>{renderCharacter()}</div>
    </div>
  );
}

function mapStateToProps(state) {
  return {
    chars: state.characters.array,
  };
}

export default connect(mapStateToProps)(Home);
```

Para conectar un componente a redux tenemos que bajar connect de react redux

---

El reducer siempre regresa un estado aunque no haga nada