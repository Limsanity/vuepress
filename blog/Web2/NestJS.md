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

### Example

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