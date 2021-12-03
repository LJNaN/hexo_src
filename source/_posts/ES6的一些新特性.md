---
title: ES6的一些新特性
date: 2021-10-09 17:33:45
tags: [ES6, JS, 优化]
categories: 知识点
top: true
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
<!--more-->

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

#### 三、if 判断条件

以前的做法

```js
if (
  type === 1 ||
  type === 2 ||
  type === 3 ||
  type === 4 ||
) {
  // ...
}
```

使用ES6的数组`includes`方法

```js
const condition = [1,2,3,4]
if (condition.includes(type)) {
  // ...
}
```

#### 四、可选链 操作符 (?. 和 ??)

##### 可选链操作符 (?.)

以前

```js
let name = obj && obj.name;
// 避免了 obj === undefined || obj === null 引发的报错
```

使用 ?.

```js
let name = obj?.name;
```

> js会在尝试访问`obj.name`之前隐式的检查并确定`obj`既不是`null`也不是`undefined`。如果`obj`是`null`或者`undefined`,表达式将会短路计算直接返回`undefined`

##### 空值合并操作符 (??)

可以在使用可选链时设置一个默认值

```js
const obj = {
  name: '孙笑川'
}
const age = obj?.age ?? 18 // age: 18
```

#### 五、同时为多个变量赋值

以前

```js
let a = 1
let b = 2
let c = 3
```

改进

```js
let [a, b, c] = [1, 2, 3]
```

#### 六、循环写法

以前

```js
for (let i = 0; i < data.length, i++) {}
```

改进

```js
for (let i in data)
for (let i of data)
```

#### 七、默认参数值

```js
const add = (a = 1, b = 2) => { return a + b }
add(3, 5) // 8
add()     // 3
```

#### 八、延展操作符

以前

```js
const data = [1, 2, 3]
const newData = [4, 5, 6].concat(data)
```

改进

```js
const data = [1, 2, 3]
const newData = [4, 5, 6, ...data]
```

还可以用操作符来克隆

```js
const data = [1, 2, 3]
const newData = [...data]
```

#### 字符串转换为数字

以前

```js
let num1 = parseInt('123')
let num2 = parseFloat('123.4')
```

改进

```js
let num1 = +'123'
let num2 = +'123.4'
```

#### Object.entries()

把对象转换为数组

```js
const data = {
  a: 1,
  b: 2,
  c: 3
}
const arr = Object.entries(data)
console.log(arr)
// [
//   ['a', 1],
//   ['b', 2],
//   ['c', 3]
// ]
```

#### Object.values()

提取对象的所有 键值对 里的值

```js
const data = {
  a: 1,
  b: 2,
  c: 3
}
const arr = Object.values(data)
console.log(arr)
// [1, 2, 3]
```

#### 参考

> [你会用ES6，那倒是用啊！](https://juejin.cn/post/7016520448204603423)
> [可选链 操作符(?. 与 ??)](https://www.jianshu.com/p/94b3aa98c91f)
> [2021 年需要了解的 34 个 JavaScript 优化技巧](https://mp.weixin.qq.com/s/GNyXCpVqlzEJlAGzfEP0jA)
