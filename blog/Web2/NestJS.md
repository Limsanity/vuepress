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