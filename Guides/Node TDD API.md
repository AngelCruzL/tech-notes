---
tags: guide javascript node test tdd api backend
---
----

# Creación del proyecto

Necesitamos crear un directorio y colocarnos en el, en este caso será `node-tdd`, estando en este directorio ejecutamos el siguiente comando:
```sh
node init -y
```

Ahora por preferencia personal me gusta configurar el linter y prettier para mejorar el formateo del código y mantener un estilo en el mismo, para eso me base en la guía de configuración creada anteriormente, simplemente dado que el proyecto no utiliza typescript obviamos los plugins referentes a este en la primera línea de instalación de dependencias:

![[Setup Eslint - Prettier#Backend]]


En mi caso estaré utilizando `pnpm` para el manejo de las dependencias, por lo que instalaré estas con ese comando, de momento lo primero que necesitamos es instalar express:
```sh
pnpm i express -E
```

Ahora creamos el archivo `app.js` con el siguiente contenido:
```js
const express = require('express');

const app = express();
const port = 3000;

app.listen(port, () => console.log(`Server listening on port ${port}`))
```

Ahora al ejecutar el comando `node app.js` obtenemos lo siguiente:
![[node-api-initial.png]]

Esto quiere decir que la API se esta ejecutando de manera correcta en el puerto especificado.

Ahora realizaremos la instalación de JEST para realizar los test, al igual que SUPERTEST, la cual es una dependencia que nos permite inicializar la aplicación de express en el entorno de pruebas y ejecutar las solicitudes HTTP a la API en ese estado:
```sh
pnpm i -DE jest supertest
```

Ahora para trabajar más cómodamente creamos los scripts personalizados para iniciar la aplicación y para las pruebas, para ello agregamos el siguiente contenido en el archivo `package.json`:
```json
{
...
"scripts": {
"start": "nodemon app.js",
"test": "jest --watch"
},
...
}
```

Ahora al ejecutar el comando `pnpm start` podemos ver que se levanta la aplicación y al ejecutar el comando `pnpm test` observamos lo siguiente:

![[no-test-found.png]]

Esto quiere decir que la configuración de jest fue exitosa.


---
```sh
npx http-server -c-1 -p 8080 -P http://localhost:3000
```
Ejecuta http-server sin caches en el puerto 8080 con un proxy apuntando a nuestro servicio REST.


# Creación de los test

Por defecto Jest esta configurado para escanear la carpeta `__test__`, por lo que ese directorio será el que contenga nuestros archivos de pruebas. Para dichos archivos se utiliza el sufijo test o el sufijo spec, personalmente prefiero el segundo pues es para un escenario especifico.


# Integración de una base de datos

```sh
pnpm i -E sequelize sqlite3 dotenv
```



```sh
pnpm i -E config
```

```sh
pnpm i -DE cross-env
```





```js
const router = require('express').Router();

const UserService = require('./user.service');

function validateUsername(req, res, next) {
  const user = req.body;

  if (user.username === null) {
    req.validationErrors = {
      username: 'Username cannot be null',
    };
  }
  next();
}

function validateEmail(req, res, next) {
  const user = req.body;

  if (user.email === null) {
    req.validationErrors = {
      ...req.validationErrors,
      email: 'Email cannot be null',
    };
  }
  next();
}

router.post('', validateUsername, validateEmail, async (req, res) => {
  if (req.validationErrors) {
    return res.status(400).send({ validationErrors: req.validationErrors });
  }

  await UserService.save(req.body);

  return res.send({ message: 'User created successfully' });
});

module.exports = router;
```

```sh
pnpm i -E express-validator
```


```js
const router = require('express').Router();
const { check, validationResult } = require('express-validator');

const UserService = require('./user.service');

router.post(
  '',
  check('username').notEmpty().withMessage('Username cannot be null'),
  check('email').notEmpty().withMessage('Email cannot be null'),
  async (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      const validationErrors = {};
      errors
        .array()
        .forEach(error => (validationErrors[error.path] = error.msg));

      return res.status(400).send({ validationErrors });
    }

    await UserService.save(req.body);

    return res.send({ message: 'User created successfully' });
  },
);

module.exports = router;
```




# Internationalization

Install
```sh
pnpm i -E i18next i18next-fs-backend i18next-http-middleware
```





# Email

```sh
pnpm i -E nodemailer
```

```sh
pnpm i -DE nodemailer-stub
```









