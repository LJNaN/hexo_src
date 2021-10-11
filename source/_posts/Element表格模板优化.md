---
title: Element表格模板优化
date: 2021-9-08 11:05:16
tags: [Element, 表格, iconfont, 封装]
categories: 知识点
---
## icon本地化

1. 在iconfont中下载生成的symbol代码，如

`//at.alicdn.com/t/font_8d5l8fzk5b87iudi.js`

<!--more-->

2. 加入通用css代码（引入一次就行）

```html
<style type="text/css">
    .icon {
       width: 1em; height: 1em;
       vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;
    }
</style>
```

3. 挑选相应图标并获取类名，应用于页面

```html
<svg class="icon" aria-hidden="true">
    <use xlink:href="#icon-xxx"></use>
</svg>
```

---

## 封装request

get在进行url拼接时使用全小写

post/delete/put 中的data数据使用 row/json 传递

```javascript
import request from '@/utils/request'
const baseurl = '/web/v1/'

function getRequest(url, data) {
  return request({
    url: `${baseurl}${url}`,
    method: 'get',
    params: data
  })
}

function postRequest(url, data) {
  return request({
    url: `${baseurl}${url}`,
    method: 'post',
    data: data,
    headers: {
      'Content-Type': 'application/json'
    }
  })
}

function putRequest(url, data) {
  return request({
    url: `${baseurl}${url}`,
    method: 'put',
    data: data,
    headers: {
      'Content-Type': 'application/json'
    }
  })
}

function deleteRequest(url, data) {
  return request({
    url: `${baseurl}${url}`,
    method: 'delete',
    data: data,
    headers: {
      'Content-Type': 'application/json'
    }
  })
}

export { getRequest, postRequest, putRequest, deleteRequest }

```

---

## 可移植性

本form页面主要是完成一个表格模板，要最大程度上保证其可移植性

### css

布局和样式方面最好使用ElementUI自带的组件，如

* Container布局容器 代替 div.myheader   div.mymain  div.myfooter
* Layout24栅格布局
* Button按钮样式 统一
* 尽可能少的css代码

### mock

测试时使用mock.js，发送对应的请求到mock.js上，后端部署好之后直接将url地址切换到后端上去。

```javascript
const Mock = require('mockjs')
module.exports = [
  {
    // 拿列表
    url: '/web/v1/formcomponent/items',
    type: 'get',
    response: config => {
      let params = config.query
      params.pageIndex = Number(params.pageIndex)
      params.pageSize = Number(params.pagesize)
      let items = []
      items = Mock.mock({
        ['items|' + params.pageSize]: [{
          id: '@integer(1, 50000)',
          roadNum: 'DL@integer(10000, 50000)',
          roadManager: '@cname()',
          roadManagerPhone: '1@integer(1000000000, 9999999999)',
          roadType: '@pick(["国道", "省道", "县道", "乡道"])',
          roadLength: '@integer(1, 5)',
          infoStart: '@csentence',
          infoEnd: '@csentence',
          countryPassed: '@county()、@county()、@county()',
          color: '@hex()',
          lineWidth: '@integer(1, 5)', 
          icon: '@pick(["icon-shan", "icon-suidao", "icon-tielu", "icon-heliu", "icon-qiaoliang", "icon-roadTransport"])',
          belong: '@county()'
        }]
      }).items
      return {
        code: 20000,
        data: {
          total: 1572,
          items
        }
      }
    }
  }, {
    // 删除
    url: '/web/v1/formcomponent/items',
    type: 'delete',
    response: config => {
      const params = config.body.id
      console.log('params: ', params);
      return {
        code: 20000,
        data: {
          errorMsg: 'ok'
        }
      }
    }
  }, {
    // 新增
    url: '/web/v1/formcomponent/item',
    type: 'post',
    response: config => {
      const params = config.body
      return {
        code: 20000,
        data: { errorMsg: 'ok' }
      }
    }
  }, {
    // 更新
    url: '/web/v1/formcomponent/item',
    type: 'put',
    response: config => {
      const params = config.body
      console.log('params: ', params);
      return {
        code: 20000,
        data: { errorMsg: 'ok' }
      }
    }
  }
]
```
