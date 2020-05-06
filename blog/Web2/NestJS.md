---
sidebar: auto
title: 'NestJS'
---

# NestJS

## Guard

### Conception

- CanActivate、canActivate
  - implements CanActivate
  - canActivate return synchronously or asynchronously
    - if return true, request will be processed
    - if return false, request will be denied
    - take single argument, which is `Execution Context` instance

- compare with middleware
- after middleware，before pipe and interceptor
- Execution Context
  - inherits from `ArgumentsHost`（see `exception filters`）

### Use case

#### Role-base authentication

- implements CanActivate
- override canActivate method
  - accept Execution Context instance as single argument
  - return synchronous or asynchronous boolean type result, such as
    - ture or false
    - Promise\<boolean\>
    - Observable\<boolean\>

#### Binding guard

- UseGuards
  - on class、methods
  - class or instance as argument
  - global guard
    - hybrid apps doesn't set up guard for gateway and micro service
    - standard micro service app does mount guard globally
    - global guard registered from outside of module cannot inject dependencies since this is done outside outside the context of any modules. 

#### Setting roles per handler

- @SetMetadata



## Pipes

### Conception

- annotated with `@Injectable()` and implement the `PipeTransform` interface
- two typical use cases
  - transformation
  - validation
- interpose a pipe before a method and the pipe receives the arguments destined for the method
- pipe run inside the exceptions zone

### Built-in pipes

- ValidationPipe
- ParseIntPipe
- ParseBoolPipe
- ParseArrayPipe
- ParseUUIDPipe

### PipeTransform Interface

- Every pipe has to provide the `transform` method, and accpet two parameters:
  - value
    - currently processed argument
  - metadata
    - type: `'body' | 'query' | 'param' | 'custom'`
      - indicate whether the argument is a body `@Body()`, query `@Query`, param `@Param()`, or custem parameter.
    - metatype
      - provides the metatype of the argument, e.g. `String`. The value is `undefined` if you either omit a type declaration in the route handler method signature, or use vanilla JavaScript.
    - data
      - The string passed to the decorator, e.g. `@Body('string')`. It's `undefined` if you leave the decorator parenthesis empty.
- TypeScript interfaces disappear during transiplaton. Thus, if a method parameter's type is declared as an interface instead of a class, the `metatype` value will be Object.

### Use case

#### Validation

##### Object schema validation

- install `@hapi/joi` for dependency and `@types/hapi__joi` for dev-dependency
- create a pipe file e.g. `joi-validation.pipe.ts`
  - implements `PipeTransform` 
  - constructor receive an `ObjectSchema` type argument
  - override transform method
    - use joi validate method for validating value
    - return value or `BadRequestException` instance
- create a schema file e.g. `createCatSchema` 
  - `Joi.object` for validating
- Binding pipes in Controller file
  - method-scoped, controller-scoped, global-scoped, param-scoped

##### Class validator

- install `class-validator` and `class-transformer` for dependencies
- use `class-validator` insde dto file e.g. `create-cat.dto.ts`
- create a pipe file e.g. `validation.pipe.ts`
  - implements `PipeTransform`
  - override transform method
    - essure value is a custom type by validating ` metatype`
    - use `plainToClass` method  (from `class-transformer`) for getting an object
    - use `validator` method (from `class-validator`) for validating an object

#### Transformation

- create pipe file e.g. `parse-int.pipe.ts`
- implements `PipeTransform`
- override `transform` method
  - return transformed value



## Authentication

### Local Strategy

#### Install Dependencies

- 对于所有的strategy都需要安装`@nestjs/passport`和`passport`包，然后是具体strategy对应的包如`passport-jwt`和`passport-local`。

#### Implement Strategy

- 创建module和service文件

- 编写service中的逻辑，并在module中引入service
- 创建strategy文件

- 编写strategy逻辑，需要继承`PassportStrategy(Strategy)`，并重写`validate`方法，在module中引入strategy
- root module中引入auth module，在controller中使用`UseGuard`实现鉴权



## Validation

### Built-in pipes

- ValidationPipe
  - make use of [class-validator](https://github.com/typestack/class-validator)
- ParseIntPipe
- ParseBoolPipe
- ParseArrayPipe
- ParseUUIDPipe

### Overview

#### Using the built-in ValidationPipe

- all `class-validator` options are available
- use concrete class in DTO instead of generic or interfaces in typescript
- `app.useGlobalPipes(new ValidationPipe())` for auto-validation
- disable detailed errors by passing option to `ValidationPipe` constructor
- set `whitelist: true` for filtering out properties which without any decorator in validation class, aka DTO
- set `forbidNonWhitelisted: true` and `whitelist: true` for denying processing request when no-whitelisted properties present
- set `transform: true` for transforming the plain JS Object comming from network to their DTO class, with the option enabled, `ValidationPipe` will also perform conversion of primitive types e.g. specify the `id` type as number in the method signature.
- alternatively, with auto-transformation disabled, you can use `parseIntPiep` etc for explicit conversion
- use `parseArrayPipe` and pass DTO class for validating array type. In addition, the `ParseArrayPipe` may come in handy when parsing query parameters because you can pass `seperator` option
- works the same for websockets and microservices