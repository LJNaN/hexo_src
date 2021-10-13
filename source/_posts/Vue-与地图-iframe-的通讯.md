---
title: Vue 与地图(iframe)的通讯
date: 2021-10-13 14:33:09
tags: [地图, Vue, 知识点]
categories: 知识点
---

`Vue`与`地图`、`iframe`不能直接通讯，地图不能拿到`Vue`里的数据，`Vue`也不能直接操作地图里的东西，这个时候怎么办呢？

这个时候我们就需要搭一座桥

#### 直接在`Window`对象下创建全局方法

```js
created() {
  window['MyFunc'] = () => {
    this.MyFunc()
  }
},
methods: {
  MyFunc() {
    // ...
  }
},
```

然后你就可以在你的`地图`、`iframe`文件中使用这个全局方法

```html
<!--地图或者 iframe的html-->
<botton onclick="window.parent['MyFunc']()">我是iframe里的按钮</botton>
```

> 其实还有一种把`iframe`放在`static`目录下的方法，但是上面那种方法自由度高得多，也不用担心其他诡异的bug
