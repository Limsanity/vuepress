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