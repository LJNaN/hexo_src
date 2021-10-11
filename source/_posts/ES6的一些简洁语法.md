---
title: ES6的一些简洁语法
date: 2021-10-09 17:33:45
tags: [ES6, JS, 优化]
categories: 知识点
---
> 这里 ES5 之后的语法简单的统称为 ES6

#### 一、从对象`obj`里取值

从前有一个obj

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
  d: 4,
  e: 5
}
```

以前的做法

```js
// 分别赋值
const a = obj.a
const b = obj.b

// 分别计算
const aANDb = obj.a + obj.b

// 分别命名
const ccc = obj.c
```

改进

```js
const {a, b, c: ccc, d, e} = obj || {} // ES6解构对象不能为 undefined 、null, 所以要给一个默认值

const aANDb = a + b

console.log(ccc)  // 3
```

#### 二、数据合并

##### 合并数组

从前有两个数组

```js
const a = [1,2,3]
const b = [1,5,6]
```

以前的做法，**去不了重**

```js
const c = a.concat(b) // [1,2,3,1,5,6]
```

使用 ES6 拓展运算符改进

```js
const c = [...new Set([...a, /...b])] // [1,2,3,5,6]
```

#### 未完待续

#### 参考

> [你会用ES6，那倒是用啊！](https://juejin.cn/post/7016520448204603423)