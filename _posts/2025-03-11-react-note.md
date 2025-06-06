---
title: React 知识记录
tags: React
---

记录 react 基础知识点，防止遗忘。

<!--more-->

## state

## 数据传递方式

### props

### ref

### context

## 生命周期

## 高阶组件

## 渲染调优

### 避免子组件的重复渲染

使用`React.memo`、`useMemo`、`useCallback`来优化

- `React.memo`: `memo(Component, areEqual?)` 返回值是一个新的组件, 当上一次 props 和下一次 props 相同时(或者 areEqual 返回为真), 不会重新渲染, React 推荐用于当属性值未改变时跳过重新渲染

- `useMemo`: `const memoizedValue = useMemo(() => returnValue, [deps]);` 只在依赖项改变时重新计算值, React 推荐用于 在重新渲染时缓存计算结果

- `useCallback`: `const memoizedCallback = useCallback(fn, [deps]);` 只在依赖项改变时重新创建回调函数
有个 hack 的方式, 因为函数组件编译后本身会转换成一个类似 `React.createElement` 的函数, 所以可以利用这点达到和 `memo` 类似的效果

详见[react 渲染优化](https://codesandbox.io/p/sandbox/reactxuan-ran-you-hua-vwx9m4)

两者不同点:

1. 比较规则不同: `memo` 使用 === 来比较两次渲染的 `props`, `useMemo` 使用 `Object.is` 比较依赖项
2. 语义不同: `memo` 用于组件, `useMemo` 用于计算值

### PurComponent

## ErrorBoundary 和 Suspense

`ErrorBoundary`: 用于展示渲染子组件过程中，发生了报错，想在页面展示兜底内容或者报错信息
`Suspense`: 用于展示子组件依赖异步数据或者动态加载子组件代码的场景，在子组件未 ready 前，展示 loading 态

**原理**

这两个组件的实现都基于 `try...catch`, 用于捕获组件的 `throw`, `React.lazy` 返回的是一种特殊的对象，拥有独立的 `tag`, 在渲染时会单独处理，捕获到异常后会判断是 ``

## 组件库
