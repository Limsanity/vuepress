---
title: 'NestJS Question'
---

# NestJS Question

## Passport的封装

编写`LocalStrategy`需要继承`PassportStrategy(Strategy)`，`PassportStrategy(Strategy)`返回的类中，构造函数做的事情就是调用`passport.use`，同时将`LocalStrategy`中的`validate`方法放进callback中，而这个callback则传入真正的`LocalStrategy`的构造函数。自己编写的`LocalStrategy`需要作为providers传入，因此其初始化控制交给了Nest。至于`LocalAuthGuard`，需要继承`AuthGuard('local')`，这个local则指明了使用`LocalStrategy`作为验证的策略。



## Cookie签名

cookie签名需要一个secret，express中cookieOptions设置`signed: true`时，会读取`request.secret`进行签名，`cookie-parser`中间件负责设置`request.secret`。

