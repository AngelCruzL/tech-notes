---
tags: framework
---
----

[Documentation | NestJS - A progressive Node.js framework](https://docs.nestjs.com/)

----

# ¿Qué es Nest?


Para poder generar un recurso (conjunto de servicios, endpoints, etc) con nest podemos utilizar el siguiente comando:

```bash
nest g res <resourceName>
```

Para agregar un prefijo global es necesario configurar lo siguiente dentro del archivo `main.ts`:
```ts
app.setGlobalPrefix('<global-prefix>');
```

![[MongoDB - Docker#Dockerizar el servicio de MongoDB]]

# Validaciones

Es necesario instalar las siguientes dependencias
```sh
yarn add -E class-validator class-transformer
```

Para los datos se recomienda trabajar con las clases en lugar de interfaces, ya que estas permiten la adición de lógica de negocio, aunque esto queda abierto dependiendo de la arquitectura que se vaya a utilizar.

Para agregar validaciones globales ejecutar lo siguiente:
```ts
app.useGlobalPipes(
	new ValidationPipe({
		whitelist: true,
		forbidNonWhitelisted: true
	})
);
```


Una entidad es la representación de una tabla en la base de datos
Una instancia de la entidad representa un registro dentro de su respectiva tabla en la base de datos.


# Add joi validation
[Configuration | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/configuration)

```bash
yarn add -E @nestjs/config joi
```


Create a new validation schema on `src/config/joi-validation.ts`, at this time I use this the next validations:
```ts
import * as Joi from 'joi';  
  
export const JoiValidationSchema = Joi.object({  
	PORT: Joi.number().default(3000),  
	STAGE: Joi.string().default('dev'),  
	DB_HOST: Joi.string().required(),  
	DB_PORT: Joi.number().default(5432),  
	DB_USER: Joi.string().required(),  
	DB_PASS: Joi.string().required(),  
	DB_NAME: Joi.string().required(),  
});
```


Add the next code to `app.module.ts`:
```ts
import { ConfigModule } from '@nestjs/config';  
  
import { JoiValidationSchema } from './config/joi-validation';   
  
@Module({  
	imports: [  
		ConfigModule.forRoot({  
			envFilePath: '.env',  
			validationSchema: JoiValidationSchema,  
		}),  
		...  
	],  
	controllers: [],  
	providers: [],  
})  
export class AppModule {}
```


## Add database connection (PostgreSQL)
[Database | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/database)

Install the dependencies with the next command:
```bash
yarn add -E @nestjs/typeorm typeorm pg
```

Ahora se realiza la configuración en el archivo `app.module.ts`:
```ts 
import { TypeOrmModule } from '@nestjs/typeorm';  

@Module({  
	imports: [  
		...
		TypeOrmModule.forRoot({  
			type: 'postgres',  
			host: process.env.DB_HOST,  
			port: +process.env.DB_PORT,  
			username: process.env.DB_USER,  
			password: process.env.DB_PASS,  
			database: process.env.DB_NAME,  
			synchronize: process.env.STAGE !== 'prod',  
			autoLoadEntities: true,  
		}),
		...
	],  
	controllers: [],  
	providers: [],  
})  
export class AppModule {}
```


# Instalar GraphQL en Nest
[GraphQL + TypeScript | NestJS - A progressive Node.js framework](https://docs.nestjs.com/graphql/quick-start)

Tras realizar la creación de un proyecto con el comando `nest new <projectName>`.

Ejecutar el comando:
```bash
yarn add @nestjs/graphql @nestjs/apollo @apollo/server graphql -E
```


Tras realizar la instalación de las dependencias anteriores es necesario agregar el siguiente contenido dentro del archivo `app.module.ts`:
```ts
import { Module } from '@nestjs/common';  
import { GraphQLModule } from '@nestjs/graphql';  
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';  
  
@Module({  
	imports: [  
		GraphQLModule.forRoot<ApolloDriverConfig>({  
			driver: ApolloDriver,  
		}),  
	],  
	controllers: [],  
	providers: [],  
})  
export class AppModule {}
```

En caso de ejecutar la aplicación en este momento en la consola obtendremos un error similar al siguiente:
![[error-query-root.png]]

Este error nos menciona que debemos proveer un query inicial para poder utilizar la aplicación de manera satisfactoria,  para ello agregamos el siguiente contenido:
```typescript
GraphQLModule.forRoot<ApolloDriverConfig>({
  driver: ApolloDriver,
  autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
}),
```

Esto no soluciona del todo el error, pues estamos indicando cual es la localización del schema de la aplicación, pero aún no lo hemos creado, 

Ahora necesitamos crear un módulo, con el fin de simplificar la explicación comenzaremos abordando los conceptos de manera progresiva, entonces haremos primero la parte más sencilla con un ejemplo clásico de programación, para ello creamos un módulo con el siguiente comando:
```bash
nest g mo helloWorld
```

Esto creará el módulo hello-world y ademas actualizará el archivo app.module agregando la referencia a dicho módulo.

Ahora para crear el resolver y deshacernos del error anterior necesitamos crear un resolver, para ello podemos utilizar el CLI de nest y ejecutar el siguiente comando:
```bash
nest g r helloWorld --no-spec
```


El resolver lo podemos ver como un controlador o un dto por el cual pasará la información y será procesada para poder ejecutarse.

Dentro del resolver ahora colocaremos el siguiente contenido:
```ts
import { Query, Resolver } from '@nestjs/graphql';  
  
@Resolver()  
export class HelloWorldResolver {  
	@Query(() => String)  
	async helloWorld() {  
		return 'Hello World!';  
	}  
}
```

Ahora en caso de tener corriendo el servidor de desarrollo podemos ver que el error del Query raíz ha desaparecido y que se ha creado un archivo llamado `schema.gql` con el siguiente contenido:
```gql
# ------------------------------------------------------  
# THIS FILE WAS AUTOMATICALLY GENERATED (DO NOT MODIFY)  
# ------------------------------------------------------  
  
type Query {  
helloWorld: String!  
}
```

Aquí podemos observar como el resolver que declaramos fue convertido a un query de graphQL, es importante resaltar lo que dice el archivo auto-generado y es que no se realicen modificaciones en este, ya que al auto-generarse de nuevo estas se perderán, lo mejor es crear distintos schemas dependiendo del tipo de solicitudes o información que queramos manejar.

## Configuración de Apollo Server

Podemos cambiar el playground que viene incorporado por defecto con graphQL por Apollo, para hacerlo es necesario desactivar el playground y en los plugins activar el de Apollo dentro del archivo app.module.ts
```ts
import { join } from 'path';  
import { Module } from '@nestjs/common';  
import { GraphQLModule } from '@nestjs/graphql';  
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';  
import { ApolloServerPluginLandingPageLocalDefault } from '@apollo/server/plugin/landingPage/default';  
  
import { HelloWorldModule } from './hello-world/hello-world.module';  
  
@Module({  
	imports: [  
		GraphQLModule.forRoot<ApolloDriverConfig>({  
			driver: ApolloDriver,  
			autoSchemaFile: join(process.cwd(), 'src/schema.gql'),  
			playground: false,  
			plugins: [ApolloServerPluginLandingPageLocalDefault()],  
		}),  
		HelloWorldModule,  
	],  
	controllers: [],  
	providers: [],  
})  
export class AppModule {}
```


Ahora que tenemos un query a este podemos realizarle anotaciones, lo cual puede ayudar a agregar datos adicionales, entre estas bondades esta la generación de documentación o la capacidad de re-nombrar los queries del endpoint:
```ts
@Query(() => String, {  
	name: 'hello',  
	description: 'A simple Hello World GraphQL query',  
})  
helloWorld(): string {  
	return 'Hello World!';  
}
```

Por ejemplo dentro de este query le cambiamos el nombre al endpoint de `helloWorld` a `hello`. Además de eso se agrega una descripción al endpoint que puede consultarse desde el sitio de Apollo.
![[apollo-hello-query.png]]

Es importante destacar que todo esto se realiza bajo el approach code first, el cuál implica que de la lógica de negocio se generarán los queries de manera automática. También existe el approach de schema first donde se desarrollará basado en los queries de graphQL.

Ahora vamos a agregar otro endpoint, en este caso será para obtener un número aleatorio.
```ts
@Query(() => Int, {  
	name: 'randomNumberFromZeroTo',  
	description:  
		'Get a random number from 0 to the specified number (exclusive - default 8)',  
})  
getRandomNumberFromZeroTo(  
	@Args('to', { type: () => Int, nullable: true, defaultValue: 8 })  
	to: number,  
): number {  
	return Math.floor(Math.random() * to);  
}
```

Ahora podemos ver nuevos conceptos, como el hecho de recibir argumentos dentro de los queries creados desde typescript. En este caso recibiremos un argumento llamado `to`, el cuál será de tipo `Int`, que puede ser nulo (en otras palabras es opcional) y también contendrá un valor por defecto.
También por parte de typescript a este argumento le asignamos el nombre de `to` y que este será de tipo `number`.

# Endpoint todo

Ahora crearemos un nuevo módulo y resolver para gestionar los todos, para ello ejecutaremos los siguientes comandos:
```bash
nest g mo todos
```

```bash
nest g r todos --no-spec
```

Es necesario crear un tipo de dato personalizado para facilitar el desarrollo, esto se logra gracias a las entidades. Una entidad es la representación de una entrada en una base de datos, en caso de ser NoSQL será la representación de un documento, y en caso de ser SQL será un registro. Para este caso puntual crearemos un archivo `todos/entity/todos.ts` con el siguiente contenido:
```ts
import { ObjectType } from '@nestjs/graphql';  
  
@ObjectType()  
export class Todo {  
	id: number;  
	description: string;  
	isCompleted: boolean;  
}
```

Sin embargo aquí solo estamos indicando que es un tipo dentro de graphQL pero no que tipo de datos maneja, para eso es necesario implementar decoradores para indicar más información sobre estas propiedades.
```ts
import { Field, Int, ObjectType } from '@nestjs/graphql';  
  
@ObjectType()  
export class Todo {  
	@Field(() => Int)  
	id: number;  
	  
	@Field(() => String)  
	description: string;  
	  
	@Field(() => Boolean)  
	isCompleted: boolean;  
}
```

Ahora tenemos que crear dentro del resolver los queries para poder implementar estas funcionalidades en el endpoint de graphQL.
```ts
import { Query, Resolver } from '@nestjs/graphql';  
  
import { Todo } from './entity/todo';  
  
@Resolver()  
export class TodosResolver {  
	@Query(() => [Todo], { name: 'todos' })  
	findAll(): Todo[] {  
		return [];  
	}  
}
```

Al realizar esto podemos observar lo siguiente dentro de Apollo:
![[todos-base.png]]

Ahora implementaremos la funcionalidad para obtener todos los todos, esto lo realizaremos creando un servicio para manejar toda la lógica de negocio dentro de este archivo. Para ello es necesario crear el servicio con el siguiente comando:
```bash
nest g s todos --no-spec
```

Dentro del servicio implementaremos lo siguiente:
```ts
import { Injectable } from '@nestjs/common';  
  
import { Todo } from './entity/todo';  
  
@Injectable()  
export class TodosService {  
	#todos: Todo[] = [  
		{ id: 1, description: 'Buy milk', isCompleted: false },  
		{ id: 2, description: 'Buy eggs', isCompleted: true },  
		{ id: 3, description: 'Buy bread', isCompleted: false },  
	];  
  
	findAll(): Todo[] {  
		return this.#todos;  
	}  
}
```

Ahora implementamos este método dentro del resolver (el cuál funciona como un controlador tradicional), por ello implementaremos lo siguiente:
```ts
import { Query, Resolver } from '@nestjs/graphql';  
  
import { Todo } from './entity/todo';  
import { TodosService } from './todos.service';  
  
@Resolver()  
export class TodosResolver {  
	constructor(private readonly todosService: TodosService) {}  
	  
	@Query(() => [Todo], {  
		name: 'todos',  
		description: 'Get all todos',  
	})  
	findAll(): Todo[] {  
		return this.todosService.findAll();  
	}  
}
```

Ahora tras implementar esto dentro de Apollo veremos lo siguiente:
![[todos-with-service.png]]

# Manejo de errores dentro de GraphQL

Ahora agregaremos el query para traer un todo basado en su id, la lógica como siempre irá en el servicio y dentro del resolver la mandaremos a llamar
`todos.service.ts`:
```ts
findOne(id: number): Todo {  
	const todo = this.#todos.find((todo) => todo.id === id);  
	  
	if (!todo) throw new NotFoundException(`Todo with id ${id} not found`);  
	  
	return todo;  
}
```

`todos.resolver.ts`:
```ts
@Query(() => Todo, {  
	name: 'todo',  
	description: 'Get a todo by id',  
})  
findOne(  
	@Args('id', { type: () => Int })  
	id: number,  
): Todo {  
	return this.todosService.findOne(id);  
}
```

En caso de no encontrar un todo con el ID proporcionado nos mostrará el siguiente mensaje:
![[not-found-exception.png]]

# Fragments

Los fragments dentro de GraphQL son unidades reutilizables para construir grupos de campos y así evitar re-escribir todo varias veces. Un ejemplo de esto es el siguiente:
![[todos-without-fragment.png]]

Al implementar un fragmento podemos observar lo siguiente:
![[todos-with-fragment.png]]


# Mutations

Son queries usados para modificar la data almacenada y retornar valores



# Inputs

Dentro de una mutación es el equivalente al body dentro de una petición REST tradicional


Estos dos conceptos son importantes, pues nos ayudan a realizar la creación de un DTO, el cual nos permitirá validar los tipos de datos y la forma que tendrán estos datos y/o objetos. Para realizar estas validaciones al igual que con nestjs tradicional podemos utilizar class validator y class transformer:
```sh
yarn add class-validator class-transformer
```

Y agregamos el siguiente contenido dentro del archivo `main.ts`:
```ts
app.useGlobalPipes(
	new ValidationPipe({
		whitelist: true,
		forbidNonWhitelisted: true
	})
);
```

Tras realizar esto podemos decorar el DTO, en este caso esta ubicado dentro del archivo `todos/dto/inputs/create-todo.ts` y cuenta con el siguiente contenido:
```ts
import { Field, InputType } from '@nestjs/graphql';  
import { IsNotEmpty, IsString, MaxLength } from 'class-validator';  
  
@InputType()  
export class CreateTodoInput {  
	@Field(() => String, { description: 'The todo description' })  
	@IsString()  
	@IsNotEmpty()  
	@MaxLength(20)  
	description: string;  
}
```


Al verlo en el playground de Apollo podemos observar lo siguiente:
![[create-todo-successfully.png]]

En caso de no cumplir con alguna validación como dejar el campo en blanco o exceder el límite de caracteres vemos lo siguiente:

![[create-todo-error-empty.png]]

![[create-todo-error-length.png]]


# Actualizar TODO

Para ello es necesario crear un nuevo DTO, en este caso dentro del archivo `update-todo`:

```ts
import { Field, InputType, Int } from '@nestjs/graphql';  
import {  
	IsBoolean,  
	IsInt,  
	IsNotEmpty,  
	IsOptional,  
	IsString,  
	MaxLength,  
	Min,  
} from 'class-validator';  
  
@InputType()  
export class UpdateTodoInput {  
	@Field(() => Int)  
	@IsInt()  
	@Min(1)  
	id: number;  
	  
	@Field(() => String, {  
		description: 'The todo description',  
		nullable: true,  
	})  
	@IsString()  
	@IsNotEmpty()  
	@MaxLength(20)  
	@IsOptional()  
	description?: string;  
	  
	@Field(() => Boolean, {  
		description: 'The todo completion status',  
		nullable: true,  
	})  
	@IsBoolean()  
	@IsOptional()  
	isCompleted?: boolean;  
}
```

Para este DTO existe un campo obligatorio y ese es el id, ya que con este es con el que vamos a realizar la búsqueda del todo que vaya a ser actualizado y de ahí el resto de propiedades son opcionales.

También es necesario implementar el método en el servicio:
```ts
update({ id, description, isCompleted }: UpdateTodoInput): Todo {  
	const todoToUpdate = this.findOne(id);  
	  
	if (description) todoToUpdate.description = description;  
	if (isCompleted !== undefined) todoToUpdate.isCompleted = isCompleted;  
	  
	this.#todos = this.#todos.map((todo) => {  
		return todo.id === id ? todoToUpdate : todo;  
	});  
	  
	return todoToUpdate;  
}
```
En este caso buscamos el todo a actualizar basado en el id, en caso de no encontrarlo manda un error y en caso de encontrarlo actualiza la propiedad que se recibe además del ID, actualiza el nuevo todo dentro del arreglo de todos y regresa como respuesta el que ha sido modificado.

Y por último implementamos este método dentro del resolver:
```ts
@Mutation(() => Todo, { name: 'updateTodo' })  
updateTodo(@Args('updateTodoInput') updateTodoInput: UpdateTodoInput): Todo {  
	return this.todosService.update(updateTodoInput);  
}
```

Tras realizar estas implementaciones dentro del playground de Apollo vemos lo siguiente:
![[Frameworks/images/nest/update-todo.png]]


# Eliminación de un todo

Creación de la mutación dentro del resolver:
```ts
@Mutation(() => Boolean, { name: 'removeTodo' })  
removeTodo(  
	@Args('id', { type: () => Int })  
	id: number,  
): boolean {  
	return this.todosService.remove(id);  
}
```

Creación del método del servicio:
```ts
remove(id: number): boolean {  
	const todoToRemove = this.findOne(id);  
	this.#todos = this.#todos.filter((todo) => todo.id !== todoToRemove.id);  
	return true;  
}
```

Con ello dentro de Apollo playground obtenemos la siguiente funcionalidad:
![[delete-todo.png]]


# Filter todos based on it's status

To implement this you need to create a new DTO, this time will be inside the `src/todos/dto/args/status` and will have the next content:
```ts
import { ArgsType, Field } from '@nestjs/graphql';  
import { IsBoolean, IsOptional } from 'class-validator';  
  
@ArgsType()  
export class StatusArgs {  
	@Field(() => Boolean, {  
		description: 'The todo completion status',  
		nullable: true,  
	})  
	@IsBoolean()  
	@IsOptional()  
	status?: boolean;  
}
```

Now we implement the service functionality inside the findAll method:
```ts
findAll(statusArgs: StatusArgs): Todo[] {  
	const { status } = statusArgs;  
	  
	if (status !== undefined)  
		return this.#todos.filter((todo) => todo.isCompleted === status);  
	  
	return this.#todos;  
}
```

And finally we implement the argument inside the resolver query:
```ts
@Query(() => [Todo], {  
	name: 'todos',  
	description: 'Get all todos',  
})  
findAll(@Args() statusArgs: StatusArgs): Todo[] {  
	return this.todosService.findAll(statusArgs);  
}
```


# Aggregations

Ahora vamos a agregar ciertas funcionalidades, como el total de todos, o los totales resultantes después de haber realizado su filtrado, para ello agregamos getters en el servicio para la longitud de los todos:
```ts
get totalTodos(): number {  
	return this.#todos.length;  
}  
  
get completedTodos(): number {  
	return this.#todos.filter(({ isCompleted }) => isCompleted).length;  
}  
  
get pendingTodos(): number {  
	return this.#todos.filter(({ isCompleted }) => !isCompleted).length;  
}
```

Y se realiza la implementación del resolver:
```ts
@Query(() => Int, { name: 'totalTodos' })  
totalTodos(): number {  
	return this.todosService.totalTodos;  
}  
  
@Query(() => Int, { name: 'completedTodos' })  
completedTodos(): number {  
	return this.todosService.completedTodos;  
}  
  
@Query(() => Int, { name: 'pendingTodos' })  
pendingTodos(): number {  
	return this.todosService.pendingTodos;  
}
```

![[todos-aggregation-individual.png]]

Existe otra manera de recuperar estas cantidades, en este caso podemos manejar un único query para obtener todas las aggregations a la vez, para esto necesitamos declarar un `type`:
```ts
import { Field, Int, ObjectType } from '@nestjs/graphql';  
  
@ObjectType({ description: 'Todo query aggregations' })  
export class AggregationsType {  
	@Field(() => Int)  
	total: number;  
	  
	@Field(() => Int)  
	completed: number;  
	  
	@Field(() => Int)  
	pending: number;  
	  
	@Field(() => Int, { deprecationReason: 'Most use completed instead' })  
	totalTodosCompleted: number;  
}
```

Ahora realizamos la implementación de este `type` dentro del resolver:

```ts
@Query(() => AggregationsType)  
aggregations(): AggregationsType {  
	return {  
		total: this.todosService.totalTodos,  
		completed: this.todosService.completedTodos,  
		pending: this.todosService.pendingTodos,  
		totalTodosCompleted: this.todosService.totalTodos,  
	};  
}
```

Esto dentro del playground de Apollo muestra lo siguiente:
![[todos-aggregation-query.png]]

Como se puede ver desde la implementación del type el campo `totalTodosCompleted` se encuentra deprecado, y en lugar de este se recomienda utilizar `completed`. Cabe señalar que dentro del playground que viene por defecto con graphQL las propiedades deprecadas no se mostrarán en el auto-completado, pero traerá la información si se utiliza en el query y además se mostrará dentro de la documentación.



# GraphQL with PostgreSQL

Al crear una entidad se puede utilizar el decorador `@ObjectType` y pueden convivir tranquilamente, por ejemplo:
```ts
import { Field, Float, ID, ObjectType } from '@nestjs/graphql';  
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';  
  
@Entity({ name: 'items' })  
@ObjectType()  
export class Item {  
	@PrimaryGeneratedColumn('uuid')  
	@Field(() => ID)  
	id: string;  
	  
	@Column()  
	@Field(() => String)  
	name: string;  
	  
	@Column()  
	@Field(() => Float)  
	quantity: number;  
	  
	@Column()  
	@Field(() => String)  
	quantityUnits: string;  
}
```

Ahora es necesario registrar esta entidad dentro del módulo de items en el archivo `items.module.ts`:
```ts
import { Module } from '@nestjs/common';  
import { TypeOrmModule } from '@nestjs/typeorm';  
  
import { ItemsService } from './items.service';  
import { ItemsResolver } from './items.resolver';  
import { Item } from './entities/item.entity';  
  
@Module({  
	providers: [ItemsResolver, ItemsService],  
	imports: [TypeOrmModule.forFeature([Item])],  
})  
export class ItemsModule {}
```

Ahora instalamos las dependencias para poder realizar validaciones dentro de los DTO's
```bash
yarn add -E class-validator class-transformer
```

Y agregamos el siguiente contenido dentro del archivo `main.ts`:
```ts
app.useGlobalPipes(
	new ValidationPipe({
		whitelist: true,
		forbidNonWhitelisted: true
	})
);
```

Tras realizar esto podemos decorar el DTO, en este caso esta ubicado dentro del archivo `items/dto/inputs/create-item.ts` y cuenta con el siguiente contenido:
```ts
import { Field, Float, InputType } from '@nestjs/graphql';  
import { IsNotEmpty, IsOptional, IsPositive, IsString } from 'class-validator';  
  
@InputType()  
export class CreateItemInput {  
	@Field(() => String, { description: 'Item name' })  
	@IsNotEmpty()  
	@IsString()  
	name: string;  
	  
	@Field(() => Float, { description: 'Item quantity' })  
	@IsPositive()  
	quantity: number;  
	  
	@Field(() => String, {  
	description: 'Item quantity units',  
	nullable: true,  
	})  
	@IsString()  
	@IsOptional()  
	quantityUnits?: string;  
}
```

Ahora es momento de implementar la funcionalidad para crear un nuevo item, para ello primero comenzaremos con el `items.service.ts`:
```ts
import { Injectable } from '@nestjs/common';  
import { InjectRepository } from '@nestjs/typeorm';  
import { Repository } from 'typeorm';  
  
import { Item } from './entities/item.entity';  
import { CreateItemInput, UpdateItemInput } from './dto/inputs';  
  
@Injectable()  
export class ItemsService {  
	constructor(  
		@InjectRepository(Item)  
		private readonly itemsRepository: Repository<Item>,  
	) {}  
	  
	async create(createItemInput: CreateItemInput): Promise<Item> {  
		const newItem = this.itemsRepository.create(createItemInput);  
		return await this.itemsRepository.save(newItem);  
	}  
	...
}
```

Y ahora implementamos la funcionalidad dentro del resolver para poder mandar a llamar a este método:
```ts
import { Args, Int, Mutation, Query, Resolver } from '@nestjs/graphql';  
  
import { Item } from './entities/item.entity';  
import { ItemsService } from './items.service';  
import { CreateItemInput, UpdateItemInput } from './dto/inputs';  
  
@Resolver(() => Item)  
export class ItemsResolver {  
	constructor(private readonly itemsService: ItemsService) {}  
	  
	@Mutation(() => Item)  
	async createItem(  
		@Args('createItemInput') createItemInput: CreateItemInput,  
	): Promise<Item> {  
		return this.itemsService.create(createItemInput);  
	}  
	...
}
```

Si ahora realizamos el procedimiento para crear un nuevo item pese a no tener ningún error en consola nos vamos a encontrar con la siguiente situación:
![[create-item-error.png]]

Para solventar este error es necesario modificar la entidad de item para hacer el campo `quantityUnits` opcional como en el DTO:
```ts
import { Field, Float, ID, ObjectType } from '@nestjs/graphql';  
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';  
  
@Entity({ name: 'items' })  
@ObjectType()  
export class Item {  
	@PrimaryGeneratedColumn('uuid')  
	@Field(() => ID)  
	id: string;  
	  
	@Column()  
	@Field(() => String)  
	name: string;  
	  
	@Column()  
	@Field(() => Float)  
	quantity: number;  
	  
	@Column({ nullable: true })  
	@Field(() => String, { nullable: true })  
	quantityUnits?: string;  
}
```

Ahora al ejecutar el mismo procedimiento de creación de un item desde el playground obtenemos lo siguiente:
![[create-item-successfull.png]]

## Get items

Ahora para obtener todos los items realizamos los siguientes cambios en el `item.service.ts`:
```ts
async findAll(): Promise<Item[]> {  
	return this.itemsRepository.find();  
}
```

Y ahora modificamos el resolver:
```ts
@Query(() => [Item], { name: 'items' })  
async findAll(): Promise<Item[]> {  
	return this.itemsService.findAll();  
}
```

Esto nos da el siguiente resultado:
![[get-all-items.png]]

Ahora para implementar el get item by id modificamos el servicio:
```ts
findOne(id: string): Promise<Item> {  
	const item = this.itemsRepository.findOneBy({ id });  
	  
	if (!item) throw new NotFoundException(`Item with id "${id}" not found`);  
	  
	return item;  
}
```
Dentro de este método encontramos que en caso de que no exista un item con el ID proveído.

Y el resolver:
```ts
import { ParseUUIDPipe } from '@nestjs/common';

@Query(() => Item, { name: 'item' })  
findOne(  
	@Args('id', { type: () => ID }, ParseUUIDPipe) id: string,  
): Promise<Item> {  
	return this.itemsService.findOne(id);  
}
```
Ahora dentro de este resolver implementamos el `ParseUUIDPipe` para validar que el ID proveído concuerde con el formato de un UUID.

Esto nos da 3 escenarios tras la llamada de este query, el primero es el caso en el que se ha proveído un ID que no concuerda con el formato de un UUID:
![[uuid-item-validation.png]]

![[get-item-by-id.png]]


## Update item

Primero que nada realizamos la actualización del `update-item.input.ts`:
```ts
import { Field, ID, InputType, PartialType } from '@nestjs/graphql';  
import { IsUUID } from 'class-validator';  
  
import { CreateItemInput } from './create-item.input';  
  
@InputType()  
export class UpdateItemInput extends PartialType(CreateItemInput) {  
	@Field(() => ID)  
	@IsUUID()  
	id: string;  
}
```

Actualizamos el servicio y después el resolver:
```ts
async update(id: string, updateItemInput: UpdateItemInput): Promise<Item> {  
	const item = await this.itemsRepository.preload(updateItemInput);  
	  
	if (!item) throw new NotFoundException(`Item with id "${id}" not found`);  
	  
	return this.itemsRepository.save(item);  
}
```

```ts
@Mutation(() => Item)  
updateItem(  
	@Args('updateItemInput') updateItemInput: UpdateItemInput,  
): Promise<Item> {  
	return this.itemsService.update(updateItemInput.id, updateItemInput);  
}
```

Esto nos da como resultado lo siguiente
![[update-item.png]]

Al realizar la consulta total vemos reflejados los cambios:
![[get-all-items-full.png]]

## Delete item

```ts
async remove(id: string): Promise<Item> {  
	const item = await this.findOne(id);  
	await this.itemsRepository.remove(item);  
	  
	return { ...item, id };  
}
```

```ts
@Mutation(() => Item)  
removeItem(@Args('id', { type: () => ID }) id: string): Promise<Item> {  
	return this.itemsService.remove(id);  
}
```

![[delete-item.png]]









