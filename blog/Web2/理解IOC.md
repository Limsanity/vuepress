---
sidebar: auto
title: '理解IOC'
---

# 理解IOC

## Reading

- [深入理解Typescript中的控制反转和依赖注入](https://jkchao.github.io/typescript-book-chinese/tips/metadata.html#%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC%E5%92%8C%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)
- [Dependency Injection in TypeScript](https://nehalist.io/dependency-injection-in-typescript/)
- [Dependency Injection in nodejs](https://medium.com/@magnusjt/dependency-injection-in-nodejs-9601a19c1f36)
- [IOC Container in nodejs](https://medium.com/@magnusjt/ioc-container-in-nodejs-e7aea8a89600)

## 什么是IOC

IOC（inversion of control）即控制反转，依赖注入是IOC的一种形式。

依赖注入表现为内部依赖由外部注入，例如：类Controller依赖类Service，但并不直接在Controller构造函数中主动实例化Service，而是在构造函数接受一个Service类型的参数，其实例化的工作交给外部。这种模式带来的好处就是松耦合。

## NestJS中的IOC

```ts
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}

// app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}

```

上述示例中AppController依赖于AppService，但是其构造函数中并没有直接实例化AppService，而是接受该类型的参数，并赋值给`this.appService`。实例化AppController的时候，Nest将根据AppController构造函数接受的参数类型，为其实例化相应的Service。

## 实现简单版本的依赖注入

```ts
import 'reflect-metadata'

type Constructor<T = any> = new (...args: any[]) => T

const Decorator = (): ClassDecorator => target => {}

class MyService {
  sayHello() {
    console.log('hello')
  }
}

@Decorator()
class MyController {
  constructor(private service: MyService) {}

  sayHello() {
    this.service.sayHello()
  }
}

const Factory = <T>(target: Constructor<T>): T => {
  const providers = Reflect.getMetadata('design:paramtypes', target)
  const args = providers.map((provider: any) => new provider())
  return new target(...args)
}

Factory(MyController).sayHello() // hello
```

[reflect-metada仅作用于使用了装饰器的类](https://stackoverflow.com/questions/48547005/why-is-reflect-metadata-only-working-when-using-a-decorator)。