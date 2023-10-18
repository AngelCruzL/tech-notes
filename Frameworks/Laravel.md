---
tags: framework backend
---
----

[Documentación]()

----

# ¿Qué es


## POO en PHP

Getters → Son una convensión de la poo que sirven para obtener el valor que existe en un objeto, usualmente estos getters solo retornan el valor, no lo imprimen ni lo manipulan de ninguna manera

Usualmente se crea un getter para cada una de las propiedades del objeto

Un constructor es una función que se ejecuta al crear una nueva instancia del objeto

Los setters se utilizan para añadir o agregar un valor después de ser instanciada la clase y ser creada desde el constructor

Los métodos estáticos te permiten acceder a ellos sin necesidad de instanciar una clase, un ejemplo de uso de aplicación de ellos es en una conexión a base de datos:

```php
<?php include 'includes/header.php';

class DB
{
  public static function conectarDB()
  {
    echo 'Conectando a la BD...';
  }
}
class Email
{
  public static function enviarEmail()
  {
    echo 'Enviando Email...';
  }
}

DB::conectarDB();

echo '<br>';

Email::enviarEmail();
```

Las constantes no requieren ser instanciadas

```php
class Bebida extends MenuRestaurant
{
  public $medida;

  //   Definir constantes
  const CON_ALCOHOL = 1;
  const SIN_ALCOHOL = 0;

  public function __construct($nombre, $precio, $medida)
  {
    parent::__construct($nombre, $precio);
    $this->medida = $medida;
  }
  public function getPrecio()
  {
    return 'El precio es: ' . $this->precio;
  }
  public function getMedida()
  {
    return $this->medida;
  }
}

echo Bebida::CON_ALCOHOL;
```

Una clase abstracta es una clase que no se puede instanciar, sin embargo esta clase sirve como base para otras clases

```php
<?php include 'includes/header.php';

// Una clase astracta es una que no se puede instanciar, en cambio sirve como base para otras clases
// Nuestra clase de MenuRestaurant es un buen ejemplo de una clase que actua como base para otras clases.

abstract class MenuRestaurant // agregando abstract evitamos que alguien la utilice para instanciarla
{
  public $nombre;
  protected $precio;

  public function __construct($nombre, $precio)
  {
    $this->nombre = $nombre;
    $this->precio = $precio;
  }

  public function getNombre()
  {
    return $this->nombre;
  }

  public function getPrecio()
  {
    return $this->precio;
  }
}
```

Las interfaces son plantillas para las clases, no se recomienda colocar mucha logica en estas interfaces:

```php
<?php include 'includes/header.php';

// Las interfaces son plantillas para tus classes, no es buena idea poner abstración o lógica en ella, solo la forma que deberán tener las classes, como dije anteriormente, son solamente una plantilla

interface RestaurantInterface
{
  public function getNombre();
  public function getPrecio(): int;

  // Con parametros
  public function setPrecio($precio);
}

class MenuRestaurant implements RestaurantInterface // añadimos implements para añadir la interface
{
  public $nombre;
  protected $precio;

  public function __construct($nombre, $precio)
  {
    $this->nombre = $nombre;
    $this->precio = $precio;
  }

  public function getNombre()
  {
    return $this->nombre;
  }

  public function getPrecio(): int
  {
    return $this->precio;
  }

  public function setPrecio($precio)
  {
    // quitar y ponerlo
    $this->precio = $precio;
  }
}

$menu = new MenuRestaurant('Platillo', '12.9');

var_dump($menu);
```

Tiene como fin que sirva como estructura para la clase, estas interfaces solo funcionan con los metodos

Namespaces

Dos clases no pueden tener el mismo nombre, esto lo permite php

Web recomendado para vistas, api para respuestas json y crear apis para consumir contenido del proyecto

La url principal tiene que cargar una vista llamada index.blade.php

Controlador hace las consultas de las bases de datos y manda los resultados a las vistas

## Autenticacion

```php
composer require laravel/ui
```

```php
php artisan ui:auth
```

Las migraciones son el control de versiones para la base de datos

Te permiten modificar y compartir el Schema de tu base de datos en un equipo de trabajo

Se puede generar la migracion al crear el modelo

Para crear un modelo con una migracion puedes ejecutar el siguiente comando

```php
php artisan make:model Clientes --migration
php artisan make:model Clientes --m
```

Una vez creada la migracion esta puede ser ejecutada con artisan

## Serve

The comand `php artisan serve` turns on the development server, by default this server use the route: 127.0.0.1[:8000](http://127.0.0.1:8000/)

## Routes

The routes are defined inside the routes folder in the file api.php and web.php, here the web page have the routes and the api responds just the data in json format

To declare a route we use the next sintaxis:

```jsx
Route::get('/', function () {
    return view('welcome');
});
```

The first paramether here is the uri of the route, the second one is a contoller, a controller could be a method of the class or a function

To see the routes on the app you can une the next command `php artisan route:list`

### Custom Routes Names

You can add custom names to your routes using the next sintaxis:

```jsx
Route::get('/', function () {
    return 'Home page';
})->name('home.index');
```

### Route parameters and optional routes parameters

To create a route with pareamaters we can use the next sintaxis:

```jsx
Route::get('/post/{id}',function($id){
    return 'Blog post ' . $id;
})->name('post.show');
```

Also we can use optional parameters

```jsx
Route::get('/recent-posts/{days_ago?}',function($daysAgo=5){
    return 'Post from ' . $daysAgo . ' days ago.';
})->name('posts.recent.index');
```

To add some kind of validation at route we can use middlewares with the where function:

```jsx
Route::get('/post/{id}',function($id){
    return 'Blog post ' . $id;
})->where([
    'id'=>'[0-9]+'
])->name('post.show');
```

We can add the middlewares globaly on the app at `app/providers/RouteServiceProvider.php` file:

```jsx
public function boot()
    {
        $this->configureRateLimiting();

        $this->routes(function () {
            Route::prefix('api')
                ->middleware('api')
                ->namespace($this->namespace)
                ->group(base_path('routes/api.php'));

            Route::middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web.php'));
        });

        //  We add this line to applicate a pattern for id number validation
        Route::pattern('id', '[0-9]+');
    }
```

Doing this last step you can delete (or comment) the middleware on `web.php` file:

```jsx
Route::get('/post/{id}',function($id){
    return 'Blog post ' . $id;
})
// ->where([
    // 'id'=>'[0-9]+'
// ])
->name('post.show');
```

### Templating and views

By convention the templates are on `resources/views/**.blade.php` directory, this templates are common html files with an extra tags called directives

To load this views as return of routes controller we need use the next sintaxis:

```jsx
Route::get('/', function () {
    return view('home.index',[]);
})->name('home.index');
```

On this case the `view()` function have two parameters, the first one is the name of the template that the uri will be rendering, in this case the parameter is `'home.index'`, this means that on the default directory of the views `resources/views/**.blade.php` laravel will search a directory called `home/`, inside this directory it will use the file called `index.blade.php` to return to function.

And the second parameter is optional, just in case that you need send data to the view (or template), these data will be send as a object.

You can use the templates on your apps to minimize the code of the pages, its a standar make this templates on `resources/views/layouts/` directory

When we use a template we can use the directive `@yield`, this directive will be load dinamicly the param that we specifies, also you can use the `@extends('file')` directive, when file is the file witch you prettend extend

When we need extend a section (a multiline export) we can use the next sintaxis

```php
@section('file')

@endsection
```

Also for singular sections you can use the next sintaxis:

```php
@section('title', 'Contact Page')
```

The first parameter is the name of the property and the second one is the value.

### Passing and Rendering Data in Templates

To pass aditional data to the template from the routes declaration you can use the next sitaxis:

```php
Route::get('/post/{id}', function ($id) {
  $posts = [
    1 => [
      'title' => 'Intro to Laravel',
      'content' => 'This is a short intro to Laravel',
    ],
    2 => [
      'title' => 'Intro to PHP',
      'content' => 'This is a short intro to PHP',
    ],
  ];

  abort_if(!isset($posts[$id]), 404);

  return view('posts.show', ['post' => $posts[$id]]);
})
```

In the last code we can see a function called `abort_if()`, on this case that function help us to return a 404 state if the id on the url is unset.

And on the `posts/show.blade.php` we need to use the next code to render the data:

```php
@extends('layouts.app')

@section('title', $post['title'])

@section('content')
<h1>{{$post['title']}}</h1>
<p>{{$post['content']}}</p>
@endsection
```

### Simple views

You can refactor simple view with a diferent sintaxis, on these case you only call a `view()` function and the parameters of these function will be the uri and the page that will be rendered:

```php
Route::get('/', function () {
  return view('home.index', []);
})->name('home.index');

Route::view('/', 'home.index')->name('home.index');

-----------------------------

Route::get('/contact', function () {
  return view('home.contact', []);
})->name('home.contact');

Route::view('/contact', 'home.contact')->name('home.contact');
```

### Conditional rendering

```php
Route::get('/post/{id}', function ($id) {
  $posts = [
    1 => [
      'title' => 'Intro to Laravel',
      'content' => 'This is a short intro to Laravel',
      'is_new' => true,
    ],
    2 => [
      'title' => 'Intro to PHP',
      'content' => 'This is a short intro to PHP',
      'is_new' => false,
    ],
  ];

  abort_if(!isset($posts[$id]), 404);

  return view('posts.show', ['post' => $posts[$id]]);
})
  // ->where([
  // 'id'=>'[0-9]+'
  // ])
  ->name('post.show');
```

```php
@extends('layouts.app')

@section('title', $post['title'])

@section('content')

@if($post['is_new'])
<div>A new blog post! Using if</div>
@elseif(!$post['is_new'])
<div>A old post using elseif</div>
@endif

<h1>{{$post['title']}}</h1>
<p>{{$post['content']}}</p>
@endsection
```

Also we have a alternative conditional rendering, the `@unless()` directive, this directive will be render the content only if this condition is false and we dont have more alternatives

We have other two alternatives to make a conditional rendering

isset() → Is variable or array key defined

empty() → Is false, 0, empty array

[Blade Templates](https://laravel.com/docs/8.x/blade#if-statements)

### Loops in templates

The loops on laravels works as the same as php loops,

```php
$posts = [
  1 => [
    'title' => 'Intro to Laravel',
    'content' => 'This is a short intro to Laravel',
    'is_new' => true,
    'has_comments' => true,
  ],
  2 => [
    'title' => 'Intro to PHP',
    'content' => 'This is a short intro to PHP',
    'is_new' => false,
  ],
];

Route::get('/posts', function () use ($posts) {
  //   compact($posts) === ['posts' => $posts];
  return view('posts.index', ['posts' => $posts]);
});
```

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @foreach($posts as $key=> $post)
    <div>{{$key}}. {{$post['title']}}</div>
  @endforeach
@endsection
```

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @if(count($posts))
    @foreach($posts as $key=> $post)
      <div>{{$key}}. {{$post['title']}}</div>
    @endforeach
  @else
    No posts
  @endif
@endsection
```

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @if(count($posts))
    @foreach($posts as $key => $post)
      <div>{{$key}}. {{$post['title']}}</div>
    @endforeach
  @else
    No posts
  @endif
@endsection
```

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @forelse($posts as $key=> $post)
    @break($key == 2)
    <div>{{$key}}. {{$post['title']}}</div>
  @empty
    No posts
  @endforelse
@endsection
```

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @forelse($posts as $key=> $post)
    @continue($key == 1)
    <div>{{$key}}. {{$post['title']}}</div>
  @empty
    No posts
  @endforelse
@endsection
```

[Blade Templates](https://laravel.com/docs/8.x/blade#the-loop-variable)

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @forelse($posts as $key=> $post)
  @if ($loop->even)
  <div>{{$key}}. {{$post['title']}}</div>
  @else
  <div style='background-color:silver' >{{$key}}. {{$post['title']}}</div>
  @endif
  @empty
    No posts
  @endforelse
@endsection
```

### Partial Templates

[Blade Templates](https://laravel.com/docs/8.x/blade#including-subviews)

This is a part of a template to reduce the code on the files, you can reuse this codes depending the situation

```php
@if ($loop->even)
  <div>{{$key}}. {{$post['title']}}</div>
  @else
  <div style='background-color:silver' >{{$key}}. {{$post['title']}}</div>
  @endif
```

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
  @forelse($posts as $key=> $post)
    @include('posts.partials.post')
  @empty
    No posts
  @endforelse
@endsection
```

The variables of the partial will be available on the file where we include it.

When you use partials on loops you can use the each directive instead of loop directive combined with include

The each directive have four parameters

```php
@each('view.name', $jobs, 'job', 'view.empty')
```

1. Name of the view (partial)
2. The collection to render
3. The name of the iteration variable made available in the partial template
4. OPTIONAL: Partial template to render if the collection is empty

```php
@extends('layouts.app')

@section('title', 'Blog Posts')

@section('content')
	@each('posts.partials/post', $posts, 'post')
@endsection
```

```php
<div>{{$key}}. {{$post['title']}}</div>
```

## Request and response

The response function creates a response object, this object has methods like header or cookie

The response function reciebes three parameters (all optional)

1. The content
2. The status code
3. An array of response headers

The cookie is used to keep user data on the browser side, this is used to implement features like user sesion, remember me, or a logged in user

The cookie reciebe three parameters

1. The name of the cookie
2. Value name of the cookie
3. The time of expiration

200 ok

400 not found

500 internal server error

Use the response function it's different to use a view function, you should use the response function when you need set a cookie or change the response status code

### Redirections

You can redirect the users to another pages

```php
Route::get('/fun/redirect', function () {
  return redirect('/contact');
});
```

The back function redirect you to the past page were you in

```php
Route::get('/fun/back', function () {
  return back();
});
```

Redirecting to a named view

```php
Route::get('/fun/named-route', function () {
  return redirect()->route('posts.show', ['id' => 1]);
});
```

Redirecting to another external pages

```php
Route::get('/fun/away', function () {
  return redirect()->away('<https://google.com>');
});
```

Json responses

```php
Route::get('/fun/json', function () use ($posts) {
  return response()->json($posts);
});
```

Returning file downloads:

[HTTP Responses](https://laravel.com/docs/8.x/responses#file-downloads)

The first argument is the path of the file, the second its a name of the download (will need be different from the original), the third one is the optional headers (this is an array)

```php
Route::get('/fun/download', function () {
  return response()->download(public_path('/daniel.jpg'), 'face.jpg');
});
```

### Grouping routes

The routes can be grouped if share attributes like url prefix or name prefix or the same middleware

```php
Route::prefix('/fun')
  ->name('fun.')
  ->group(function () use ($posts) {
    Route::get('/responses', function () use ($posts) {
      return response($posts, 201)
        ->header('Content-Type', 'application/json')
        ->cookie('MY_COOKIE', 'Luis', 3600);
    })->name('responses');

    Route::get('/redirect', function () {
      return redirect('/contact');
    })->name('redirect');

    Route::get('/back', function () {
      return back();
    })->name('back');

    Route::get('/named-route', function () {
      return redirect()->route('posts.show', ['id' => 1]);
    })->name('named-route');

    Route::get('/away', function () {
      return redirect()->away('<https://google.com>');
    })->name('away');

    Route::get('/json', function () use ($posts) {
      return response()->json($posts);
    })->name('json');

    Route::get('/download', function () {
      return response()->download(public_path('/daniel.jpg'), 'face.jpg');
    })->name('download');
  });
```

### Reading user Input

```php
Route::get('/posts', function () use ($posts) {
  // dd(request()->all());
  dd((int) request()->input('page', 1));
  //   compact($posts) === ['posts' => $posts];
  return view('posts.index', ['posts' => $posts]);
});
```

### Retrieving Boolean Input Values

When dealing with HTML elements like checkboxes, your application may receive "truthy" values that are actually strings. For example, "true" or "on". For convenience, you may use the **`boolean`** method to retrieve these values as booleans. The **`boolean`** method returns **`true`** for 1, "1", true, "true", "on", and "yes". All other values will return **`false`**:

```
$archived = $request->boolean('archived');
```

[HTTP Requests](https://laravel.com/docs/8.x/requests#retrieving-boolean-input-values)

## Middleware

Is the code that runs before and after the controller, its a php class with a method or handler

Are three ways to apply a middleware:

1. Global: Applies to all routes
2. Web or api: Applies only on web or api routes
3. Route specific middleware

On `app/http/Kernel.php` directory, you should see the global middlewares, this middlewares will be apply on al the routes

```php
protected $middleware = [
  \\App\\Http\\Middleware\\TrustHosts::class,
  \\App\\Http\\Middleware\\TrustProxies::class,
  \\Fruitcake\\Cors\\HandleCors::class,
  \\App\\Http\\Middleware\\PreventRequestsDuringMaintenance::class,
  \\Illuminate\\Foundation\\Http\\Middleware\\ValidatePostSize::class,
  \\App\\Http\\Middleware\\TrimStrings::class,
  \\Illuminate\\Foundation\\Http\\Middleware\\ConvertEmptyStringsToNull::class,
];
```

Also you can see the middleware Groups, this will be applied for web and api's

```php
protected $middlewareGroups = [
  'web' => [
    \\App\\Http\\Middleware\\EncryptCookies::class,
    \\Illuminate\\Cookie\\Middleware\\AddQueuedCookiesToResponse::class,
    \\Illuminate\\Session\\Middleware\\StartSession::class,
    // \\Illuminate\\Session\\Middleware\\AuthenticateSession::class,
    \\Illuminate\\View\\Middleware\\ShareErrorsFromSession::class,
    \\App\\Http\\Middleware\\VerifyCsrfToken::class,
    \\Illuminate\\Routing\\Middleware\\SubstituteBindings::class,
  ],

  'api' => [
	  'throttle:api',
	  \\Illuminate\\Routing\\Middleware\\SubstituteBindings::class,
  ],
    ];
```

the function `middleware()` is to apply a middleware on a specific route, the param of this function could be on of this:

```php
protected $routeMiddleware = [
	'auth' => \\App\\Http\\Middleware\\Authenticate::class,
	'auth.basic' => \\Illuminate\\Auth\\Middleware\\AuthenticateWithBasicAuth::class,
	'cache.headers' => \\Illuminate\\Http\\Middleware\\SetCacheHeaders::class,
	'can' => \\Illuminate\\Auth\\Middleware\\Authorize::class,
	'guest' => \\App\\Http\\Middleware\\RedirectIfAuthenticated::class,
	'password.confirm' => \\Illuminate\\Auth\\Middleware\\RequirePassword::class,
	'signed' => \\Illuminate\\Routing\\Middleware\\ValidateSignature::class,
	'throttle' => \\Illuminate\\Routing\\Middleware\\ThrottleRequests::class,
	'verified' => \\Illuminate\\Auth\\Middleware\\EnsureEmailIsVerified::class,
];
```

The middlewares are precreated you only need add the param to use it

## Controllers

This files are located on `app/Http/Controllers` directory

You can create it using the terminal with the next command

```php
php artisan make:controller <Name of the Controller>
```

An action is a controller method bound to a root

For example, to make a HomeController

```php
<?php

namespace App\\Http\\Controllers;

use Illuminate\\Http\\Request;

class HomeController extends Controller
{
  public function home()
  {
    return view('home.index');
  }

  public function contact()
  {
    return view('home.contact');
  }
}
```

```php
Route::get('/', [HomeController::class, 'home'])->name('home.index');

Route::get('/contact', [HomeController::class, 'contact'])->name(
  'home.contact'
);
```

On this last file (web.php) we change the view function to get, an the second param was replaced by an array, on this array we send as a first parameter the Controller, and as a second one the name of the function of these controller

If you controller only will have one function you can use the `__invoke()` function

```php
<?php

namespace App\\Http\\Controllers;

use Illuminate\\Http\\Request;

class AboutController extends Controller
{
  public function __invoke()
  {
    return 'Single';
  }
}
```

```php
Route::get('/single', AboutController::class);
```

```php
php artisan make:controller PostsController --resource
```

The `--resource` flag is to create a controller with all the CRUD routes actions by default

Yo can use only some actions using the `only()` function

```php
Route::resource('posts', PostsController::class)->only(['index', 'show']);
```

Or use all except some of them using the `except()` function

```php
Route::resource('posts', PostsController::class)->except(['index', 'show']);
```

## Database Config

Are on `database.php` file

Yo need excecute the next command

```php
php artisan migrate
```

---

Juan

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/912bf700-35ef-42c9-a4fa-d437624a22b0/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17787479-af80-4119-994c-00092312ed88/Untitled.png)

Router model binding → El modelo esta asociado a una ruta, entonces laravel consulta ese modelo

Además se pueden manejar multiples modelos y variables dentro del route model binding, de esta manera Laravel será el encargado de gestionar las consultas a la base de datos de la manera más eficiente.

Method spoofing → Es la capacidad de agregar más verbos http a los que soporta el navegador, por defecto estos últimos son post y get, pero con este método se puede soportar put, patch y delete

```bash
sail artisan route:cache
```

```bash
sail artisan route:list
```

```bash
sail artisan make:model Follower -mc
```

attach se utiliza al crear relaciones con tablas pivote

Router tipo clousure

```php
Route::get('/', function () {
  return view('main-page');
});
```

```bash
sail artisan make:component PostList
```

```bash
sail artisan view:clear
```

```bash
sail artisan optimize
```



**create model with migration, controller, and factory**

```sh
sail artisan make:model --migration —factory —controller <ModelName>
```

To enable the factory functionality you need to execute the next command:

```sh
sail artisan tinker
```
This open’s the tinker shell and now you can execute the next:


```sh
Post::factory()->times(200)->create();
```
This will create 200 registers based on the PostFactory file params, the factory uses faker to fill the db registers with fictitious information.

  
```sh
sail artisan migrate:rollback --step=1
```
Allows us to rollback the last migration in order to fix the errors or clean the last created tables

-------

**Commands to work on Laravel**

  

  

./vendor/bin/sail up  -> Wakes up the docker container

  

./vendor/bin/sail npm run dev  -> Executes the npm run dev command on the package.json

  

./vendor/bin/sail mysql  -> Enable the mysql command service on the docker container

  

  

  

To create migrations on Laravel you need to execute the next command:

  
```sh
sail artisan make:migration <migration name>
```
This command will create a migration file when you’ll specify the actions on up and down (execution or role back), you can thing on it as some version control.

Once you finished to write the code you need to execute the migration with the next command:
```sh
sail artisan migrate
```

After execute the migration you need to go to your model and on the $fillable associative array add the field(s) added to the table with the migration, otherwise the framework throw an error.

If you are working with the user model or want to hash some text you’ll need to import the Hash class and use the static method make:
```php
‘field_to_hash’ => Hash::make($field_to_hash’)
```

If you made changes on the migration you need to rollback with the next command:
```sh
sail artisan migrate:rollback --step=1
```

And after that make a migration again with:
```sh
sail artisan migrate
```
  

If you want to delete the data you can execute the next command:
```sh
sail artisan migrate:refresh
```


To create new controllers we can use the CLI:

```sh
sail artisan make:controller <controller name>
```
In the controllers Laravel has defined his own name convention to functions, this special named functions do an specific task:

  

- index: The presentation view of the model.
- store: Stores information





----------

corepack@0.10.0

├── gatsby-cli@4.4.0

├── nodemon@2.0.15

├── npm@8.1.2

├── typescript@4.5.5

└── yarn@1.22.17



-----

```sh
sail artisan make:model Category --migration --controller

sail artisan migrate

sail artisan make:seeder CategorySeeder

sail artisan db:seed
```

To define api routes you can use two approaches:
```php
Route::get('/categories', [CategoryController::class, 'index']);
```

```php
Route::apiResource('/categories', CategoryController::class);
```

On the last one the functions will be associated to an HTTP verb
- index -> get

To return a JSON object you need serialized as one:
```php
<?php  
  
namespace App\Http\Controllers;  
  
class CategoryController extends Controller  
{  
  public function index()  
  {  
    return response()->json([  
      'message' => 'Hello World!',  
    ]);  
  }  
}
```

But Laravel also has a resource to return a JSON response:
```php
<?php  
  
namespace App\Http\Controllers;  
  
use App\Http\Resources\CategoryCollection;  
use App\Models\Category;  
  
class CategoryController extends Controller  
{  
  public function index(): CategoryCollection  
  {  
    return new CategoryCollection(Category::all());  
  }  
}
```

To execute the above code you need to create the api resource to handle it

The api resources can custom your responses, to only include the necessary information. This resources can be used with Eloquent and its methods to get more control about the response and the information returned on it.
```shell
php artisan make:resource <resourceName>
```

```sh
sail artisan make:resource CategoryResource

sail artisan make:resource CategoryCollection
```

This will be created inside the `app/Http/Resources`

The collection will not be touched, it's an implementation abstraction, but the resource itself will be modified to return only certain fields
```php
<?php  
  
namespace App\Http\Resources;  
  
use Illuminate\Http\Request;  
use Illuminate\Http\Resources\Json\JsonResource;  
  
class CategoryResource extends JsonResource  
{  
  /**  
   * Transform the resource into an array.   *   * @return array<string, mixed>  
   */  public function toArray(Request $request): array  
  {  
    return [  
      'id' => $this->id,  
      'name' => $this->name,  
      'icon' => $this->icon,  
    ];  
  }  
}
```

You can create a model, resouce, migration and add the route endpoint with one single command:
```sh
sail artisan make:model Product --resource --api --migration
```

To make a new seed but refreshing the old data we need execute:
```sh
sail artisan migrate:refresh --seed
```

```sh
sail artisan make:resource ProductCollection
sail artisan make:resource ProductResource
```


## auth

```sh
sail artisan make:controller AuthController
```

Create auth request
```sh
sail artisan make:request RegisterRequest
```


## cors

This configuration needs to be applied on `config/cors.php` file:
```php
'supports_credentials' => true,
```




