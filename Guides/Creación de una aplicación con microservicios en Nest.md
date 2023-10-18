---
tags: guide backend microfrontend
---
----

Para esta guía se utilizará como gestor de dependencias [pnpm](https://pnpm.io/) aunque tú eres libre de usar el que sea de tu preferencia.

Primero necesitamos crear un nuevo proyecto de [[Nest]]:
```sh
nest new sleepr
```

Ahora dentro del directorio `sleepr` ejecutamos la aplicación:
```sh
pnpm run start
```

Ahora pensando en la arquitectura del proyecto tenemos el siguiente diagrama:

![[microservices-app-diagram.png]]

# Common library

Con la finalidad de no repetir el código a lo largo del proyecto comenzaremos creando una biblioteca para las funcionalidades comunes:
```sh
nest g library common
```

Ahora instalaremos las dependencias para poder realizar la conexión con la base de datos, en este caso se utilizará [[MongoDB]]:
```sh
pnpm i -E @nestjs/config @nestjs/mongoose mongoose
```

En este caso utilizaremos [[Docker]] para crear un contenedor de la base de datos:
![[MongoDB - Docker#Dockerizar el servicio de MongoDB]]

# Database connection

Ahora crearemos los módulos de configuración para la base de datos dentro del proyecto `common` (que es la librería que creamos anteriormente):
```sh
nest g mo database -p common
nest g mo config -p common
```

Adicionalmente instalaremos [Joi](https://www.npmjs.com/package/joi) para realizar las validaciones:
```sh
pnpm i -E joi
```

Dentro del archivo `libs/common/src/config/config.module.ts` colocamos el siguiente código:
```ts
import { Module } from '@nestjs/common';
import {
  ConfigModule as NestConfigModule,
  ConfigService,
} from '@nestjs/config';
import * as Joi from 'joi';

@Module({
  imports: [
    NestConfigModule.forRoot({
      validationSchema: Joi.object({
        MONGO_DB_URI: Joi.string().required(),
      }),
    }),
  ],
  providers: [ConfigService],
  exports: [ConfigService],
})
export class ConfigModule {}
```

Ademas de esto ahora crearemos un archivo de barril para poder acceder más fácilmente a los módulos (`libs/common/src/config/index.ts`):
```ts
export * from './config.module';
```

Ahora dentro del archivo `libs/common/src/database/database.module.ts` colocaremos el siguiente código:
```ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { ConfigService } from '@nestjs/config';

import { ConfigModule } from '@app/common/config';

@Module({
  imports: [
    MongooseModule.forRootAsync({
      imports: [ConfigModule],
      useFactory: (configService: ConfigService) => ({
        uri: configService.get('MONGO_DB_URI'),
      }),
      inject: [ConfigService],
    }),
  ],
})
export class DatabaseModule {}
```

También para este módulo crearemos un archivo de barril (`libs/common/src/database/index.ts`):
```ts
export * from './database.module';
```

Ahora creamos el archivo `.env.example` con la cadena de conexión para la base de datos:
```
MONGO_DB_URI=mongodb://localhost:27017/sleep
```

Dentro del archivo `libs/common/src/common.module.ts` colocamos el siguiente código:
```ts
import { Module } from '@nestjs/common';

import { ConfigModule, DatabaseModule } from '@app/common';

@Module({
  providers: [],
  exports: [],
  imports: [DatabaseModule, ConfigModule],
})
export class CommonModule {}
```

Y ahora dentro del `src/app.module.ts` colocamos lo siguiente:
```ts
import { Module } from '@nestjs/common';

import { DatabaseModule } from '@app/common';

import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [DatabaseModule],
  controllers: [AppController],
  providers: [AppService],
})
```

# Creación de un Schema

Ahora crearemos un Schema abstracto, de este heredaran todos los demás schemas, por ende debe contener las propiedades comunes, en este caso se tratará del ID únicamente dentro del archivo `libs/common/src/database/abstract.schema.ts`:
```ts
import { Prop, Schema } from '@nestjs/mongoose';
import { SchemaTypes, Types } from 'mongoose';

@Schema()
export abstract class AbstractDocument {
  @Prop({ type: SchemaTypes.ObjectId })
  _id: Types.ObjectId;
}
```

# Creación de un AbstractRepository

Al igual que con el schema, el repositorio abstracto será la base para el resto de repositorios, con la finalidad de juntar todos los métodos comunes. Este se creará en la siguiente ruta `libs/common/src/database/abstract.repository.ts`:
```ts
import { Logger, NotFoundException } from '@nestjs/common';
import { FilterQuery, Model, Types, UpdateQuery } from 'mongoose';

import { AbstractDocument } from './abstract.schema';

export abstract class AbstractRepository<TDocument extends AbstractDocument> {
  protected abstract readonly logger: Logger;

  constructor(protected readonly model: Model<TDocument>) {}

  async create(document: Omit<TDocument, '_id'>): Promise<TDocument> {
    const createdDocument = new this.model({
      ...document,
      _id: new Types.ObjectId(),
    });

    return (await createdDocument.save()).toJSON() as unknown as TDocument;
  }

  async findOne(filterQuery: FilterQuery<TDocument>): Promise<TDocument> {
    const document = await this.model.findOne(filterQuery, {}, { lean: true });

    if (!document) {
      this.logger.warn(
        `Document not found with filterQuery "${JSON.stringify(filterQuery)}"`,
      );
      throw new NotFoundException('Document not found');
    }

    return document;
  }

  async findOneAndUpdate(
    filterQuery: FilterQuery<TDocument>,
    update: UpdateQuery<TDocument>,
  ) {
    const document = await this.model.findOneAndUpdate(filterQuery, update, {
      lean: true,
      new: true,
    });

    if (!document) {
      this.logger.warn(
        `Document not found with filterQuery "${JSON.stringify(filterQuery)}"`,
      );
      throw new NotFoundException('Document not found');
    }

    return document;
  }

  async find(filterQuery: FilterQuery<TDocument>) {
    return this.model.find(filterQuery, {}, { lean: true });
  }

  async findOneAndDelete(filterQuery: FilterQuery<TDocument>) {
    return this.model.findOneAndDelete(filterQuery, { lean: true });
  }
}
```

# Microservicio de reservaciones

Ahora crearemos la aplicación de reservaciones:
```sh
nest g app reservations
```

Tras crear esta aplicación debemos realizar ciertas modificaciones en el archivo `nest-cli.json`, esto debido a que vamos a eliminar el directorio `apps/sleepr`, por lo que el archivo ce configuración de nest nos debe quedar con el siguiente contenido:
```json
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "sourceRoot": "apps/reservations/src",
  "compilerOptions": {
    "deleteOutDir": true,
    "webpack": true,
    "tsConfigPath": "apps/reservations/tsconfig.app.json"
  },
  "projects": {
    "common": {
      "compilerOptions": {
        "tsConfigPath": "libs/common/tsconfig.lib.json"
      }
    },
    "reservations": {
      "type": "application",
      "root": "apps/reservations",
      "entryFile": "main",
      "sourceRoot": "apps/reservations/src",
      "compilerOptions": {
        "tsConfigPath": "apps/reservations/tsconfig.app.json"
      }
    }
  },
  "monorepo": true,
  "root": "apps/reservations"
  }
```

Ahora al haber eliminado el proyecto de `sleepr` en caso de levantar el servidor este ya no tendrá acceso a la base de datos, por lo que dentro del archivo `apps/reservations/src/reservations.module.ts` necesitamos importar el `DatabaseModule`:
```ts
import { Module } from '@nestjs/common';

import { DatabaseModule } from '@app/common';

import { ReservationsController } from './reservations.controller';
import { ReservationsService } from './reservations.service';

@Module({
  imports: [DatabaseModule],
  controllers: [ReservationsController],
  providers: [ReservationsService],
})
export class ReservationsModule {}
```

## Creación de un CRUD para reservaciones

### Scaffolding

Tras realizar la configuración de la app de reservaciones ahora necesitamos generar un recurso, podemos hacerlo ejecutando el siguiente comando:
```sh
nest g res reservations
```

Esto creará todo un endpoint CRUD para reservaciones, tras crearlo nos generará todo el contenido dentro del directorio `apps/reservations/src/reservations`, dado que este microservicio se encargará únicamente de las reservaciones podemos mover todo el contenido de `src/reservations` a `src` y eliminar el directorio `reservations`. Al moverlo de lugar tendremos que remplazar los antiguos archivos con los nuevos para que no se pierda la funcionalidad del boilerplate del CRUD que genero el comando.

Ahora tras haber configurado las validaciones crearemos el Schema de las reservaciones dentro del archivo `apps/reservations/src/models/reservation.schema.ts`:
```ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { AbstractDocument } from '@app/common';

@Schema({ versionKey: false })
export class ReservationDocument extends AbstractDocument {
  @Prop()
  timestamp: Date;

  @Prop()
  startDate: Date;

  @Prop()
  endDate: Date;

  @Prop()
  userId: string;

  @Prop()
  placeId: string;

  @Prop()
  invoiceId: string;
}

export const ReservationSchema =
  SchemaFactory.createForClass(ReservationDocument);
```

Tras crear el Schema ahora crearemos el repositorio en la ruta `apps/reservations/src/reservations.repository.ts`:
```ts
import { Injectable, Logger } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';

import { AbstractRepository } from '@app/common';
import { ReservationDocument } from './models/reservation.schema';

@Injectable()
export class ReservationsRepository extends AbstractRepository<ReservationDocument> {
  protected readonly logger = new Logger(ReservationsRepository.name);

  constructor(
    @InjectModel(ReservationDocument.name)
    reservationModel: Model<ReservationDocument>,
  ) {
    super(reservationModel);
  }
}
```

Ahora necesitamos registrar ese schema y repositorio dentro del módulo de reservaciones, pero para esto primero necesitamos modificar el archivo `libs/common/src/database/database.module.ts` para agregarle un método que permita agregar el schema por funcionalidad:
```ts
import { Module } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { ModelDefinition, MongooseModule } from '@nestjs/mongoose';

import { ConfigModule } from '@app/common/config';

@Module({
imports: [
  MongooseModule.forRootAsync({
    imports: [ConfigModule],
    useFactory: (configService: ConfigService) => ({
      uri: configService.get('MONGO_DB_URI'),
    }),
    inject: [ConfigService],
    }),
  ],
})
export class DatabaseModule {
  static forFeature(models: ModelDefinition[]) {
    return MongooseModule.forFeature(models);
  }
}
```

Ahora en el archivo `apps/reservations/src/reservations.module.ts` registramos tanto el schema (utilizando el nuevo método que creamos) y el repositorio:
```ts
import { DatabaseModule } from '@app/common';

import { ReservationsService } from './reservations.service';
import { ReservationsController } from './reservations.controller';
import { ReservationsRepository } from './reservations.repository';
import {
  ReservationDocument,
  ReservationSchema,
} from './models/reservation.schema';

@Module({
  imports: [
    DatabaseModule,
    DatabaseModule.forFeature([
      { name: ReservationDocument.name, schema: ReservationSchema },
    ]),
  ],
  controllers: [ReservationsController],
  providers: [ReservationsService, ReservationsRepository],
})
export class ReservationsModule {}
```


### CRUD functionality

Primero agregamos las propiedades de  `apps/reservations/src/dto/create-reservation.dto.ts`:
```ts
export class CreateReservationDto {
  startDate: Date;
  endDate: Date;
  placeId: string;
  invoiceId: string;
}
```

Now to add the CRUD functionality we need to add the methods to the `reservations.service.ts`:
```ts
import { Injectable } from '@nestjs/common';
import { CreateReservationDto } from './dto/create-reservation.dto';
import { UpdateReservationDto } from './dto/update-reservation.dto';
import { ReservationsRepository } from './reservations.repository';

@Injectable()
export class ReservationsService {
  constructor(
    private readonly reservationsRepository: ReservationsRepository,
  ) {}

  create(createReservationDto: CreateReservationDto) {
    return this.reservationsRepository.create({
      ...createReservationDto,
      timestamp: new Date(),
      userId: '1',
    });
  }

  findAll() {
    return this.reservationsRepository.find({});
  }

  findOne(id: string) {
    return this.reservationsRepository.findOne({ _id: id });
  }

  update(id: string, updateReservationDto: UpdateReservationDto) {
    return this.reservationsRepository.findOneAndUpdate(
      { _id: id },
      { $set: updateReservationDto },
    );
  }

  remove(id: string) {
    return this.reservationsRepository.findOneAndDelete({ _id: id });
  }
}
```

Y por último utilizamos el servicio dentro del controlador:
```ts
import {
  Body,
  Controller,
  Delete,
  Get,
  Param,
  Patch,
  Post,
} from '@nestjs/common';

import { ReservationsService } from './reservations.service';
import { CreateReservationDto } from './dto/create-reservation.dto';
import { UpdateReservationDto } from './dto/update-reservation.dto';
@Controller('reservations')
export class ReservationsController {
  constructor(private readonly reservationsService: ReservationsService) {}
  @Post()
  create(@Body() createReservationDto: CreateReservationDto) {
    return this.reservationsService.create(createReservationDto);
  }
  @Get()
  findAll() {
    return this.reservationsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.reservationsService.findOne(id);
  }

  @Patch(':id')
  update(
    @Param('id') id: string,
    @Body() updateReservationDto: UpdateReservationDto,
  ) {
    return this.reservationsService.update(id, updateReservationDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.reservationsService.remove(id);
  }
}
```

Ahora agregaremos validaciones para el DTO
![[Nest#Validaciones]]

Tras configurar estos paquetes para las validaciones ahora tendremos que modificar la clase agregando decoradores, lo que deja el archivo con el siguiente contenido:
```ts
import { IsDate, IsNotEmpty, IsString } from 'class-validator';
import { Type } from 'class-transformer';

export class CreateReservationDto {
  @IsDate()
  @Type(() => Date)
  startDate: Date;

  @IsDate()
  @Type(() => Date)
  endDate: Date;

  @IsString()
  @IsNotEmpty()
  placeId: string;

  @IsString()
  @IsNotEmpty()
  invoiceId: string;
}
```


## Dockerizando el microservicio

Para esto creamos un archivo `Dockerfile` dentro del directorio `apps/reservations` con el siguiente contenido:
```dockerfile
FROM node:alpine AS development
LABEL authors="angelcruz"

WORKDIR /usr/src/app

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm
RUN pnpm install

COPY . .

RUN pnpm run build

FROM node:alpine AS production
LABEL authors="angelcruz"

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm
RUN pnpm install

COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/reservations/main"]
```

Esto crea una imagen la cual esta dividida en dos etapas, la primera es para desarrollo y la segunda para producción, esto con la finalidad de que en producción no se instalen las dependencias de desarrollo. También es necesario crear un archivo `.dockerignore` con el siguiente contenido:
```
.git
node_modules
dist
```

Ahora modificaremos el archivo `.dockerignore` con el siguiente contenido:
```yml
version: '3'

services:
  db_service:
    container_name: sleeprDB
    image: mongo:5
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGODB_DATABASE: sleepr_db
    volumes:
      - ./mongo:/data/db

  reservations:
    depends_on:
      - db_service
    container_name: reservationsService
    build:
      context: .
      dockerfile: ./apps/reservations/Dockerfile
      target: development
    command: pnpm run start:dev reservations
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/ap
```

Y por último necesitamos cambiar el contenido del archivo `.env.example` para que quede de la siguiente manera:
```
MONGO_DB_URI=mongodb://db_service:27017/sleepr
```

Ahora para levantar el proyecto con docker ejecutamos los siguientes comandos dentro del directorio `apps/reservations`:
```
docker build ../../ -f Dockerfile -t sleepr_reservations

docker run sleepr_reservations
```



# Custom logging

Para realizar logs personalizados necesitamos instalar las siguientes dependencias:
```sh
pnpm i -E nestjs-pino pino-http pino-pretty
```

Después de esto crearemos un módulo para que se encargue de gestionar todo lo relacionado a los logs:
```sh
nest g mo logger
```

Ahora dentro de este módulo colocamos la siguiente configuración:
```ts
import { Module } from '@nestjs/common';
import { LoggerModule as PinoLoggerModule } from 'nestjs-pino';

@Module({
  imports: [
    PinoLoggerModule.forRoot({
      pinoHttp: {
        transport: {
          target: 'pino-pretty',
          options: {
            singleLine: true,
          },
        },
      },
    }),
  ],
})
export class LoggerModule {}
```

Ahora necesitamos agregar dicho modulo al common module con la finalidad de hacerlo disponible al resto de la aplicación `libs/common/src/common.module.ts`:
```ts
import { Module } from '@nestjs/common';

import { ConfigModule, DatabaseModule, LoggerModule } from '@app/common';

@Module({
  providers: [],
  exports: [],
  imports: [DatabaseModule, ConfigModule, LoggerModule],
})
export class CommonModule {}
```

Tras esto es necesario importarlo dentro del `reservations.module.ts`:
```ts
import { Module } from '@nestjs/common';

import { DatabaseModule, LoggerModule } from '@app/common';

import { ReservationsService } from './reservations.service';
import { ReservationsController } from './reservations.controller';
import { ReservationsRepository } from './reservations.repository';
import {
  ReservationDocument,
  ReservationSchema,
} from './models/reservation.schema';

@Module({
imports: [
  DatabaseModule,
    DatabaseModule.forFeature([
      { name: ReservationDocument.name, schema: ReservationSchema },
    ]),
    LoggerModule,
  ],
  controllers: [ReservationsController],
  providers: [ReservationsService, ReservationsRepository],
})
export class ReservationsModule {}
```

Y por último lo agregamos al archivo principal (`apps/reservations/src/main.ts`) para que se configure para este microservicio:
```ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { Logger } from 'nestjs-pino';

import { ReservationsModule } from './reservations.module';

async function bootstrap() {
  const app = await NestFactory.create(ReservationsModule);

  app.useGlobalPipes(new ValidationPipe());
  app.useLogger(app.get(Logger));

  await app.listen(3000);
}

bootstrap();
```


# Microservicio de autenticación

## Creación del endpoint para crear usuarios

Para ello necesitamos crear una nueva app con el Nest CLI:
```sh
nest g app auth
```

Ahora necesitamos crear el módulo y el controlador de usuarios, estos pertenecerán a la app de auth:
```sh
nest g mo users
nest g co users
nest g s users
```

Ahora dentro del directorio `auth/src/users/dto` creamos el DTO para crear un usuario:
```ts
import { IsEmail, IsStrongPassword } from 'class-validator';

export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsStrongPassword()
  password: string;
}
```

Y ahora creamos el Schema dentro del directorio `users/models`:
```ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';

import { AbstractDocument } from '@app/common';

@Schema({ versionKey: false })
export class UserDocument extends AbstractDocument {
  @Prop()
  email: string;

  @Prop()
  password: string;
}

export const UserSchema = SchemaFactory.createForClass(UserDocument);
```

Ahora necesitamos crear el repositorio:
```ts
import { Injectable, Logger } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';

import { AbstractRepository } from '@app/common';
import { UserDocument } from './models/user.schema';

@Injectable()
export class UsersRepository extends AbstractRepository<UserDocument> {
  protected readonly logger = new Logger(UsersRepository.name);

  constructor(
    @InjectModel(UserDocument.name)
    userModel: Model<UserDocument>,
  ) {
    super(userModel);
  }
}
```

Con esto realizado es hora de pasar al servicio:
```ts
import { Injectable } from '@nestjs/common';

import { CreateUserDto } from './dto/create-user.dto';
import { UsersRepository } from './users.repository';

@Injectable()
export class UsersService {
  constructor(private readonly usersRepository: UsersRepository) {}

  async create(createUserDto: CreateUserDto) {
    return this.usersRepository.create(createUserDto);
  }
}
```

Y ahora necesitamos implementar el controlador:
```ts
import { Body, Controller, Post } from '@nestjs/common';

import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Post()
  async createUser(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }
}
```

Por último necesitamos proveer la conexión con la base de datos en el `users.module`:
```ts
import { Module } from '@nestjs/common';

import { DatabaseModule, LoggerModule } from '@app/common';

import { UsersService } from './users.service';
import { UsersController } from './users.controller';
import { UsersRepository } from './users.repository';
import { UserDocument, UserSchema } from './models/user.schema';

@Module({
  imports: [
    DatabaseModule,
    DatabaseModule.forFeature([
      { name: UserDocument.name, schema: UserSchema },
    ]),
    LoggerModule,
  ],
  controllers: [UsersController],
  providers: [UsersService, UsersRepository],
})
export class UsersModule {}
```

Ahora dentro del archivo `auth.module.ts` agregamos el módulo para realizar los logs:
```ts
import { Module } from '@nestjs/common';

import { LoggerModule } from '@app/common';

import { AuthController } from './auth.controller';
import { AuthService } from './auth.service';
import { UsersModule } from './users/users.module';

@Module({
  imports: [UsersModule, LoggerModule],
  controllers: [AuthController],
  providers: [AuthService],
})
export class AuthModule {}
```

Y ahora dentro del archivo `auth/src/main.ts` agregamos el logger y la validación de los DTOs:
```ts
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { Logger } from 'nestjs-pino';

import { AuthModule } from './auth.module';

async function bootstrap() {
  const app = await NestFactory.create(AuthModule);

  app.useGlobalPipes(new ValidationPipe({ whitelist: true }));

  app.useLogger(app.get(Logger));

  await app.listen(3001);
}

bootstrap();
```


## Dockerizando el microservicio

Ahora vamos a dockerizar el microservicio, utilizando el siguiente código en el archivo `Dockerfile`:
```dockerfile
FROM node:alpine AS development
LABEL authors="angelcruz"

WORKDIR /usr/src/app

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm
RUN pnpm install

COPY . .

RUN pnpm run build

FROM node:alpine AS production
LABEL authors="angelcruz"

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm
RUN pnpm install

COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/auth/main"]
```

Y ahora modificamos el archivo `docker-compose.yml` de modo que tenga el siguiente contenido:
```yml
version: '3'

services:
  db_service:
    container_name: sleeprDB
    image: mongo:5
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGODB_DATABASE: sleepr_db
    volumes:
      - ./mongo:/data/db

  reservations:
    depends_on:
      - db_service
    container_name: reservationsService
    build:
      context: .
      dockerfile: ./apps/reservations/Dockerfile
      target: development
    command: pnpm run start:dev reservations
    ports:
      - "3000:3000"
    volumes:
      - .:/usr/src/app

  auth:
    depends_on:
      - db_service
    container_name: authService
    build:
      context: .
      dockerfile: ./apps/auth/Dockerfile
      target: development
    command: pnpm run start:dev auth
    ports:
      - "3001:3001"
    volumes:
      - .:/usr/src/app
```


## Agregando Password

Necesitamos ejecutar el siguiente comando:
```sh
pnpm i -E @nestjs/passport passport passport-local
```

Y también los tipos para trabajar en desarrollo
```sh
pnpm i -DE @types/passport-local
```

Por último agregamos las dependencias que nos permiten trabajar con una implementación utilizando JWT:
```sh
pnpm i -E @nestjs/jwt passport-jwt
```

Y sus respectivos tipos para trabajar en desarrollo
```sh
pnpm i -DE @types/passport-jwt
```

Ahora realizamos la siguiente configuración dentro del `auth.module`:
```ts
import { Module } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { JwtModule } from '@nestjs/jwt';

import { LoggerModule } from '@app/common';

import { AuthController } from './auth.controller';
import { AuthService } from './auth.service';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    UsersModule,
    LoggerModule,
    JwtModule.registerAsync({
      useFactory: (configService: ConfigService) => ({
        secret: configService.get<string>('JWT_SECRET'),
        signOptions: {
          expiresIn: configService.get<string>('JWT_EXPIRES_IN'),
        },
      }),
    }),
  ],
  controllers: [AuthController],
  providers: [AuthService],
})
export class AuthModule {}
```


# Separando los archivos de variables de entorno

Estamos trabajando con microservicios, pero las variables de entorno hasta ahora se han manejado de manera monolitica, para separarlos podemos mover tanto el archivo `.env` como el archivo `.env.example` a cada uno de los respectivos microservicios. Tras mover los archivos de lugar es necesario indicar en el docker-compose donde estarán ubicados los archivos `.env`:
```yml
version: '3'

services:
  ...

  reservations:
    ...
    env_file:
      - ./apps/reservations/.env
    ...

  auth:
    ...
    env_file:
      - ./apps/auth/.env
    ports:
    ...
```

Ahora eliminamos la librería de configuración








---

# Crear la imagen de Docker

Para ello es necesario desplazarse a la carpeta del microservicio y dentro ejecutar el comando `docker build`, en este caso crearemos la imagen de reservations:

```sh
cd apps/reservations
```

```sh
docker build -t reservations -f . ../../
```
Este comando crea la imagen con el nombre (tag) reservations utilizando el archivo (file) Dockerfile del directorio actual (reservations) bajo el contexto `../../` que es el root del proyecto de nest (Es necesario colocarlo así, pues los archivos que colocamos en el `Dockerfile` para copiarse están en referencia al directorio raíz).



