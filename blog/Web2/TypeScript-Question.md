---
title: 'TypeScript Question'
---

# TypeScript Question

## Freshness

Freshness即严格对象字面量检查。举个例子：

```ts
function logName(something: { name: string }) {
  console.log(something.name);
}

var person = { name: 'matt', job: 'being awesome' };
var animal = { name: 'cow', diet: 'vegan, but has milk of own species' };
var random = { note: `I don't have a name property` };

logName(person); // okay
logName(animal); // okay
logName(random); // Error: property `name` is missing
logName({ name: 'cow', diet: 'vegan, but has milk of own species' }); // Error
```

上述例子中，当传入logName的参数是一个变量并且该变量存在一个额外property时并不会报错，而传入的一个对象字面量存在一个额外的property时会报错。

这是因为根据上下文来推断变量对象时，widened类型将会被使用，fresh object literal types失去freshness的情况包括：

- fresh数据类型被widened
- 类型断言

一个类型的Widened形式就是将原始数据中的null或者undefined的数据类型由Any类型替换。

```ts
var a = null;                 // var a: any  
var b = undefined;            // var b: any  
var c = { x: 0, y: null };    // var c: { x: number, y: any }  
var d = [ null, undefined ];  // var d: any[]
```

[TypeScript Widened类型](https://www.softwhy.com/article-8600-1.html)

[TypeScript类型兼容](https://www.softwhy.com/article-10195-1.html)

## Covariance and Contravariance

[reading](https://medium.com/@michalskoczylas/covariance-contravariance-and-a-little-bit-of-typescript-2e61f41f6f68)

判断两个变量是否兼容时，考虑subtypes和supertypes的关系。

判断两个函数是否兼容时，考虑函数的作用范围。例如函数A接收一个supertypes为参数，函数B接收一个subtypes为参数，通常subtypes会拥有更多的属性，也意味着通常函数B可能更为复杂，并不适用于supertypes，因此函数A可以赋值于函数B，相反则不行。