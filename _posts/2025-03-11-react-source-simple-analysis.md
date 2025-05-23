---
title: React框架原理简单分析
tags: React
---

作为主流的前端UI构建库，不必过多介绍，这里只想把`react`代码如何构建`dom`、状态更新原理说清楚

<!--more-->

## 流程

### jsx(起点)

**`jsx`**

⬇️

`<div id="container"><h1>Hello World!</h1><MyComponent /></div>`

⬇️(编译后)

`React.createElement`

⬇️(jsx编译后代码执行)

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

### Reconciler(协调器)

上面只是`jsx`部分编译后，以及执行结果，我们来说一下`App.tsx`挂载入口函数(或者hooks)做了什么

```jsx
ReactDOM.createRoot(container).render(element)
```

⬇️(进入`Reconciler`协调器的逻辑)

- 构建 FiberRoot 和 RootFiber
- Render/Reconciliation
- Commit: 对 effectList 执行dom操作

```jsx
effectList = {

}
```

### 渲染(调度器)

我们先来了解一下 `Lane` 模型

每个fiber节点会记录自己刮起的更新所归属的Lane, 以便知道在更新调度时，优先更新哪些节点

比如，页面`header`区域的`input`已经加载好了，但是下面的`table`还在加载数据或渲染，此时用户在`input`输入文本时，`react`对`input`输入这个渲染任务优先级最高(`UserBlocking`)

会中断正在渲染的表格，开始执行`input`渲染

1. 双缓存技术: 每个 Fiber 节点都有一个 `alternate` 属性，指向对应的 Fiber 节点在另一棵树中的镜像
   1. **current 树**：当前屏幕上显示的内容对应的 Fiber 树
   2. **workInProgress 树**：正在构建的新 Fiber 树
2. 双缓存优点:
   1. **允许增量渲染**：可以在后台构建新树，而不影响当前显示
   2. **支持工作中断**：在构建 workInProgress 树时可以暂停和恢复
   3. **快速切换**：完成构建后，只需切换根节点的引用

[源码位置](https://github.com/facebook/react/blob/main/packages/react-reconciler/src/ReactFiber.js#L328)

```jsx
function createWorkInProgress(current, pendingProps) {
  let workInProgress = current.alternate;

  if (workInProgress === null) {
    // 首次渲染时创建新节点
    workInProgress = createFiber(
      current.tag,
      pendingProps,
      current.key,
      current.mode,
    );
    workInProgress.elementType = current.elementType;
    workInProgress.type = current.type;
    workInProgress.stateNode = current.stateNode;

    workInProgress.alternate = current;
    current.alternate = workInProgress;
  } else {
    // 更新时复用已有节点
    workInProgress.pendingProps = pendingProps;
    workInProgress.effectTag = NoEffect;
    workInProgress.nextEffect = null;
    workInProgress.firstEffect = null;
    workInProgress.lastEffect = null;
  }

  // 从 current 复制信息到 workInProgress
  workInProgress.childExpirationTime = current.childExpirationTime;
  workInProgress.expirationTime = current.expirationTime;
  workInProgress.child = current.child;
  workInProgress.memoizedProps = current.memoizedProps;
  workInProgress.memoizedState = current.memoizedState;
  workInProgress.updateQueue = current.updateQueue;

  return workInProgress;
}
```