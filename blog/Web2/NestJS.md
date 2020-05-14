---
sidebar: auto
title: 'NestJS'
---

# NestJS

## Guard

### Concept

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

### Concept

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



## Interceptors

### Concept

- annotated with `Injectable()`, implement the `NestInterceptor` inerface
- bind extra logic before / after method execution
- transform the result returned from a function
- transform the exception thrown from a function
- extend the basic function behavior
- compeltely override a function depending on specific conditions e.g. caching purposes
- Aspect Oriented Programming

### Basic

- implements the `intercept()` method, which take two arguments.
  - one is the `ExecutionContext` instance
  - the second is a `CallHandler`

### Execution Context

- extend `ArgumentsHost`
- see detail in `ExecutionContext`

### Call handler

- implements `handle()` method, which use to invoke the route handler method
- `handle()` returns an `Observable`， so we can use RxJS operator like `map`、`tap` etc



## Custom providers

- DI is an IOC technique, where you delegate instantiation of dependencies to the IoC container, e.g. NestJS runtime system.
- three steps
  - `@Injectable`
  - annotate constuctor arguments
  - register provider with the Nest IoC container
- `providers` property takes an array of providers
  - provider item has `provide` and `useClass` and `useValue`  property
    - `provide` is a token
    - `useClass` indicate an working  instance
    - `useValue` is useful for injecting a constant value
- some use cases to use custom providers
  - want to create a custom instance instead of having Nest instantiate a class
  - want to re-use an exsting class in a second dependency
  - want to override a class with a mock version for testing
- `useValue` is useful for injecting a constant value, which should "look like" the specific class, that means same methods and so on.
- provide token can be a string as well as symbol instead of class.
  - If we need to inject a Non-Class-Based provider, we have to use `@Inject()` decorator.
- `useClass` syntax allows you to dynamically determin a class that a token should revole to, for example, different config provider in different environment.
- `useFactory` syntax allows for createing provider dynamically. A more complex factory can itself inject other providers it needs to compute its result, which has a pair of related mechanisms:
  - The factory function accpet arguments
  - `inject` property accpet array of providers that Nest will resolve and pass to fatory function, that means they should be the same order.
- `useExisting` syntax allows you to create aliases for exsting providers. Differenct provide token resolve the same instance with `SINGLETON` case.
- Non-service based providers.
- we can export a custom provider by provide token or provider object.



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