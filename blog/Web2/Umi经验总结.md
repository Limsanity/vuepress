---
title: 'Umi经验总结'
sidebar: auto
---

# Umi经验总结

## `.umirc.ts`中import的ts文件，不能只出现export语句

```ts
// .umirc.ts
import { xxx } from './A.ts'
xxx() // error

// A.ts
// 情况一
export * from './xxx.ts'

// 情况二
// would not cause error if
import { xxx } from './xxx.ts'
export { xxx }
```

umi启动时，会读取配置文件`.umirc.ts`，并递归分析其依赖文件，然后通过babel register注册。其解析依赖利用的是[crequire](https://github.com/seajs/crequire)，crequire无法解析上述示例情况一，造成相应的依赖文件未被编译而出现报错。



## Umi中动态设置router base

Umi官方文档写到可以通过设置`base`选项配置router base。其原理是通过插件机制在HTML中内联一段`window.routerBase=base`的脚本，运行时通过window.routerBase进行设置。如果希望动态设置router base，可以通过`headScript`配置选项内联一段简单的脚本。



## useModel原理浅析

在根组件外层嵌套一个Context Provider，用于传递一个[dispatcher](https://github.com/umijs/plugins/blob/master/packages/plugin-model/src/helpers/dispatcher.tsx)，dispatcher保存每个namespace的状态以及更新回调函数（即调用useModel中传入的第二个参数updater）。

为每个model创建一个Executor组件，Executor的作用在于每当namspace的状态发生了变更，就将新的状态传递给dispatcher，并触发dispatcher遍历执行对应namespace的所有更新回调。

调用useModel时，会将更新回调注册到dispatcher中，而更新回调只有在前后updater返回的数据不一致时，才会进行setState，对当前组件进行更新。