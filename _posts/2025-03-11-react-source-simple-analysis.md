---
title: React框架原理简单分析
tags: React
---

作为主流的前端UI构建库，不必过多介绍，这里只想把`react`代码如何构建`dom`、状态更新原理说清楚

<!--more-->

## jsx(起点)

**`jsx`**

⬇️

`<div id="container"><h1>Hello World!</h1><MyComponent /></div>`

编译后⬇️

`React.createElement`

`React.createElement`

⬇️

```js
{
  type: "div",
  key: null,
  ref: null,
  props: {
    id: "container",
    children: [
      {
        type: "h1",
        key: null,
        ref: null,
        props: { children: "Hello World" },
        // ...
      },
      {
        type: MyComponent,
        props: {
          // ...
        }
      }
    ]
  }
}
```

这样我们就得到了一个虚拟dom构成的树形结构，用于描述**静态的**`UI`信息

## 构建fiber
